# Zotero Web API File Uploads

In addition to providing ways to [read](dev/web_api/v3/basics#read_requests) and [write](dev/web_api/v3/write_requests) online library data, the Zotero Web API allows you to upload attachment files.

The exact process depends on whether you are adding a new attachment file or modifying an existing one and whether you are performing a full upload or uploading a binary diff.

In Zotero 10+, the [local API](dev/web_api/v3/local_api#file_uploads) supports the same full-upload flow, with the file going to Zotero rather than to S3. Binary diffs are not supported locally.

## 1a) Create a new attachment

### i. Get attachment item template

    GET /items/new?itemType=attachment&linkMode={imported_file,imported_url,linked_file,linked_url}

    {
      "itemType": "attachment",
      "linkMode": "imported_url",
      "title": "",
      "accessDate": "",
      "url": "",
      "note": "",
      "tags": [],
      "relations": {},
      "contentType": "",
      "charset": "",
      "filename": "",
      "md5": null,
      "mtime": null
    }

### ii. Create child attachment item

    POST /users/<userID>/items
    Content-Type: application/json
    Zotero-Write-Token: <token>

    [
      {
        "itemType": "attachment",
        "parentItem": "ABCD2345",
        "linkMode": "imported_url",
        "title": "My Document",
        "accessDate": "2012-03-14T17:45:54Z",
        "url": "http://example.com/doc.pdf",
        "note": "",
        "tags": [],
        "relations": {},
        "contentType": "application/pdf",
        "charset": "",
        "filename": "doc.pdf",
        "md5": null,
        "mtime": null
      }
    ]

`md5` and `mtime` can be edited directly in personal libraries for WebDAV-based file syncing. They should not be edited directly when using Zotero File Storage, which provides an atomic method (detailed below) for setting the properties along with the corresponding file.

Top-level attachments can be created by excluding the `parentItem` property or setting it to `false`. Though the API allows all attachments to be made top-level items for backward-compatibility, it is recommended that only file attachments (`imported_file`/`linked_file`) and PDF imported web attachments (`imported_url` with content type `application/pdf`) be allowed as top-level items, as in the Zotero client.

## 1b) Modify an existing attachment

### i. Retrieve the attachment information

    GET /users/<userID>/items/<itemKey>

    {
      "key": "ABCD2345",
      "version": 124,
      "library": { ... },
      ...
      "data": {
        "key": "ABCD2345",
        "version": 124,
        "itemType": "attachment",
        "linkMode": "imported_file",
        "title": "My Document",
        "note": "",
        "tags": [],
        "relations": {},
        "contentType": "text/plain",
        "charset": "utf-8",
        "filename": "doc.txt",
        "md5": "4fa38e3f2c360ca181e633d02bab91f5",
        "mtime": "1331171741767"
      }
    }

### ii. Download the existing file

    GET /users/<userID>/items/<itemKey>/file

Check the `ETag` header of the response to make sure it matches the attachment item's `md5` value. If it doesn't, check the attachment item again. If the attachment item still has a different hash, the latest version of the file may be available only via WebDAV, not via Zotero File Storage, and it is up to the client how to proceed.

Save the file as `filename` and set the modification time to the `mtime` provided in the attachment item.

### iii. Make changes to the file

Note that to perform a faster partial upload using a binary diff, you must save a copy of the file before changes are made.

## 2) Get upload authorization

    POST /users/<userID>/items/<itemKey>/file
    Content-Type: application/x-www-form-urlencoded
    If-None-Match: *

    md5=<hash>&filename=<filename>&filesize=<bytes>&mtime=<milliseconds>

For existing attachments, use `If-Match: <hash>` in place of `If-None-Match: *`, where &lt;hash&gt; is the previous MD5 hash of the file (as provided in the `ETag` header when downloading it).

Note that `mtime` must be provided in milliseconds, not seconds.

A successful `200` response returns one of two possible JSON objects:

    {
      "url": ...,
      "contentType": ...,
      "prefix": ...,
      "suffix": ...,
      "uploadKey": ...
    }

or

    { "exists": 1 }

In the latter case, the file already exists on the server and was successfully associated with the specified item. No further action is necessary.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>200 OK</code></td><td>The upload was authorized or the file already exists.</td></tr>
    <tr><td><code>403 Forbidden</code></td><td>File editing is denied.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The file has changed remotely since retrieval (i.e., the provided ETag no longer matches). Conflict resolution is left to the client.</td></tr>
    <tr><td><code>413 Request Entity Too Large</code></td><td>The upload would exceed the storage quota of the library owner.</td></tr>
    <tr><td><code>428 Precondition Required</code></td><td>If-Match or If-None-Match was not provided.</td></tr>
    <tr><td><code>429 Too Many Requests</code></td><td>Too many unfinished uploads. Try again after the number of seconds specified in the <code>Retry-After</code> header.</td></tr>
  </tbody>
</table>

## 3a) Full upload

### i. POST file

Concatenate `prefix`, the file contents, and `suffix` and POST to `url` with the `Content-Type` header set to `contentType`.

`prefix` and `suffix` are strings containing multipart/form-data. In some environments, it may be easier to work directly with the form parameters. Add `params=1` to the upload authorization request above to retrieve the individual parameters in a `params` array, which will replace `contentType`, `prefix`, and `suffix`.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>201 Created</code></td><td>The file was successfully uploaded.</td></tr>
  </tbody>
</table>

### ii. Register upload

    POST /users/<userID>/items/<itemKey>/file
    Content-Type: application/x-www-form-urlencoded
    If-None-Match: *

    upload=<uploadKey>

For existing attachments, use `If-Match: <hash>`, where &lt;hash&gt; is the previous MD5 hash of the file, provided as the `md5` property in the attachment item.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The upload was successfully registered.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The file has changed remotely since retrieval (i.e., the provided ETag no longer matches).</td></tr>
  </tbody>
</table>

After the upload has been registered, the attachment item will reflect the new metadata (`filename`, `mtime`, `md5`). The `Zotero-Library-Version` header of the response will reflect the new version of the item.

## 3b) Partial upload

    PATCH /users/<userID>/items/<itemKey>/file?algorithm={xdelta,vcdiff,bsdiff}&upload=<uploadKey>
    If-Match: <previous-value-of-md5-property>

    <Binary diff of old and new versions>

For best results, we recommend using Xdelta version 3 with the "`-9 -S djw`" flags. bsdiff takes significantly longer to generate diffs. 'vcdiff' is an alias for 'xdelta', as Xdelta3 can process diffs in VCDIFF format.

Clients may wish to automatically fall back to a full upload — possibly with some form of warning — if HTTP PATCH is not supported by a user's proxy server (indicated, in theory, by a `405 Method Not Allowed`).

After the upload has finished, the attachment item will reflect the new metadata. The `Zotero-Library-Version` header of the response will reflect the new version of the item.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The patch was successfully applied.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked; the patched file does not match the provided MD5 hash or file size</td></tr>
    <tr><td><code>428 Precondition Required</code></td><td>If-Match or If-None-Match was not provided.</td></tr>
    <tr><td><code>429 Too Many Requests</code></td><td>Too many unfinished uploads. Try again after the number of seconds specified in the <code>Retry-After</code> header.</td></tr>
  </tbody>
</table>
