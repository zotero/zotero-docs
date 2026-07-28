# Zotero Local API

Recent versions of the Zotero desktop client expose a local implementation of the [Zotero Web API](dev/web_api/v3/basics) on `localhost:23119` under `/api/`, serving data from the user's local database. Because nothing touches the network, the local API works offline, has no rate limits, and is typically much faster than the Web API. It is intended for code running on the user's own computer, like utilities and command-line tools, that would otherwise need to read directly from the SQLite database or fall back to the Web API.

The local API must be enabled in Zotero's preferences (Settings → Advanced → "Allow other applications on this computer to communicate with Zotero"). Requests will return `403 Forbidden` if it is not enabled.

The base URL is

    http://localhost:23119/api/

Most endpoints documented on the [Basics](dev/web_api/v3/basics), [Write Requests](dev/web_api/v3/write_requests) (in Zotero 10), [File Uploads](dev/web_api/v3/file_upload), and [Full-Text Content](dev/web_api/v3/fulltext_content) pages work identically when accessed under that prefix. The notable differences from the Web API are:

-   Only API version 3 is supported, and only one version will ever be supported at a time. If a future version is released and your client needs to work against both old and new copies of Zotero, request `/api/` first and read the `Zotero-API-Version` response header to determine which version the running client speaks before making further requests.
-   Read requests require no authentication. Zotero 10 supports write requests with a local API key, which the user grants through a confirmation dialog. See [Authorizing Writes](#authorizing_writes).
-   Since reads are unauthenticated, applications running locally can read the user's library. Do not forward the port or otherwise expose it externally.
-   In Zotero 10, every response includes a `Zotero-Server-ID` header identifying the Zotero instance, and all object versions are local to that instance, with no relation to Web API versions. See [Server ID](#server_id) and [Object Versions](#object_versions).
-   Only data for the locally logged-in user is available. Pass `0` as the user ID or the user's actual numeric ID, which can be found on the [API Keys](/settings/keys) page. Requests for any other user ID return `400`.
-   Group metadata is limited to what's needed to identify the group; permissions, member lists, and similar details are not included. Group metadata is also read-only, since it's maintained by the server.
-   Atom is not supported. Requests with `format=atom` or `content=atom` return `501 Not Implemented`.
-   Item type and field endpoints (see [Item Types and Fields](dev/web_api/v3/types_and_fields)) return localized names in the user's locale. The `locale` query parameter is ignored. `/api/creatorFields` is the exception and always returns English names, matching the Web API.
-   Results are not paginated by default. The local API will return the full set of matching objects in one response, since nothing has to be transferred over the network. The `limit` and `start` parameters still work if you want them, and `Link` headers are still included.
-   Partial (binary diff) file uploads are not supported. `PATCH <userOrGroupPrefix>/items/<itemKey>/file` will fail with `405 Method Not Allowed`. Upload the full file instead.
-   The implementation aims to match the Web API's documented behavior but does not attempt to replicate every implementation detail. Sort order on equal keys, quicksearch matching, and the exact JSON produced for an object may differ in minor ways. Clients that depend on undefined behavior or unusual corner cases of the Web API should be tested against both implementations.

The local API also supports a few things the Web API does not:

-   `<userOrGroupPrefix>/searches/<searchKey>/items` returns the items matching a saved search. The Web API exposes search metadata but does not actually execute searches.
-   `<userOrGroupPrefix>/items/<itemKey>/file` returns a `302` redirect to a `file://` URL for the attachment on disk, and `/file/view` does the same. `/file/view/url` returns the URL as plain text rather than redirecting.
-   In Zotero 10, `POST /api/local/authorize` requests permission to make changes.

Responses include a `Zotero-Schema-Version` header reflecting the schema version of the local Zotero instance, which may lag behind or run ahead of the version served by the Web API.

## Server ID

In Zotero 10, every local API response includes a `Zotero-Server-ID` header containing a string that identifies the Zotero instance being talked to:

    Zotero-Server-ID: sPMHtLD6HHBd

The ID is stored in the Zotero database, so it follows the user's data rather than the installation, and it stays the same across restarts and upgrades. A different ID means a different database and therefore a different set of object versions and keys.

Clients should read the ID from any response — a bare `GET /api/` is enough — cache it, and pass it back in the `Zotero-Server-ID` request header:

-   On read requests, the header is optional. If provided, it must match the current server, or the request is rejected with `412 Precondition Failed`.
-   On write requests, including `POST /api/local/authorize`, the header is required. A write without it is rejected with `428 Precondition Required`.

A `412` means the client is talking to a Zotero instance other than the one its cached data came from. Discard cached data, including versions, and start over with the new ID.

Clients that store data between runs should partition it by server ID. The Web API does not currently provide or validate `Zotero-Server-ID`, but it will in the future, so treat Web API data as belonging to its own partition.

## Object Versions

In Zotero 10, data object versions returned by the local API are maintained locally, not by the sync server. They're incremented once per library per transaction whenever an object in that library is saved or deleted, whether the change comes from the user, a sync, or a local API write.

Local versions in Zotero 10 therefore have **no relation** to Web API versions, and no relation to the local versions reported by any other Zotero instance. This applies everywhere versions appear: the `version` property of object JSON, `Last-Modified-Version` response headers, `format=versions` responses, `?since=` filtering, and the `If-Unmodified-Since-Version` and per-object `version` preconditions used for writes.

Earlier versions of Zotero reported synced versions from the local API, which were `0` for objects that had never been synced and didn't change when an object was modified locally. Local versions are usually much lower than the synced versions those releases returned, so local API clients should discard any stored versions rather than compare them to new ones.

Group metadata is the exception: `<userOrGroupPrefix>/groups` and `/groups/<groupID>` report the synced group version, which has no local counterpart. Objects *within* group libraries — their items, collections, and searches — use local versions like all others.

## Authorizing Writes

Local API keys (supported in Zotero 10) are unrelated to zotero.org API keys and can't be created in advance. A client asks for one at runtime:

    POST /api/local/authorize
    Content-Type: application/json
    Zotero-Server-ID: <serverID>

    { "appName": "My Application" }

Zotero shows a dialog naming the application and asking the user to allow the change, with "Allow", "Always Allow", and "Deny" buttons. On approval, the response is

    { "key": "<32-character key>", "remember": false }

`remember` is `true` if the user chose "Always Allow", in which case the key can be reused indefinitely. Otherwise the key is single-use: the first write request that successfully validates it consumes it, and a subsequent write needs a new key. Clients should always be prepared to handle a `401` from a write request by authorizing again.

If the user denies the request, the response is `403 Forbidden` with `{ "denied": true }`. To limit prompt spam, no more than five requests that would show a dialog are accepted per minute; further requests return `429 Too Many Requests` with a `Retry-After` header.

Pass the key with each write request, the same way as a Web API key:

1.  As an HTTP header in the form `Zotero-API-Key: <key>` (recommended)
2.  As an HTTP header in the form `Authorization: Bearer <key>`
3.  As a URL query parameter, in the form `key=<key>` (not recommended)

A write request with no key or an unrecognized key returns `401 Unauthorized` with a `WWW-Authenticate: Zotero-API-Key realm="Zotero Local API"` header.

Keys are stored with the user's profile, and unlike Web API keys, they aren't scoped: a key allows writes to any library the user can edit. Users can revoke all remembered authorizations at any time with the "Clear Write Authorizations" button in Settings → Advanced.

## Write Requests

In Zotero 10, `POST`, `PUT`, `PATCH`, and `DELETE` are supported for items, collections, and saved searches. The local API also supports tag deletion, full-text writes, and file uploads. Requests and responses follow the same format as the Web API, described in [Write Requests](dev/web_api/v3/write_requests).

`Zotero-Write-Token` is supported, but tokens are cached in memory, so they're forgotten when Zotero restarts.

Changes made through the local API are ordinary local changes. They're visible in the Zotero UI immediately and will be uploaded to zotero.org on the next sync if the library is synced.

## File Uploads

In Zotero 10, the three-phase [file upload](dev/web_api/v3/file_upload) flow works locally, with the uploads going to Zotero itself rather than to S3:

1.  `POST <userOrGroupPrefix>/items/<itemKey>/file` with `md5`, `filename`, `filesize`, and `mtime` parameters, and an `If-Match` or `If-None-Match` header, exactly as in the Web API. The response contains a `url` pointing at `/api/local/uploads/<uploadKey>` on the local server, along with `uploadKey`, `contentType`, and empty `prefix` and `suffix` strings. If the file on disk already matches the given MD5, the response is `{ "exists": 1 }` and no upload is needed.
2.  `POST` the file contents to `url`. A successful upload returns `201 Created`. The received bytes must hash to the `md5` provided in the previous step, or the response is `400`. This request doesn't need `Zotero-Server-ID` or an API key, since the upload key authorizes it. Upload keys expire after an hour.
3.  `POST` to the file endpoint again with `upload=<uploadKey>`, repeating the `If-Match` or `If-None-Match` header. Zotero moves the uploaded file into the attachment's storage directory and updates the item, returning `204 No Content` with the new library version in `Last-Modified-Version`.

Uploads are only accepted for stored-file attachments (`imported_file` and `imported_url`); other attachment types return `400`. Files must be under 4 GB.

## Full-Text Content

`PUT <userOrGroupPrefix>/items/<itemKey>/fulltext` sets an attachment's full-text content as documented in [Full-Text Content](dev/web_api/v3/fulltext_content), and `POST <userOrGroupPrefix>/fulltext` accepts an array of up to 10 entries, each with a `key` property, returning the same result object as other multi-object writes. Bulk writes require `If-Unmodified-Since-Version`.

The versions returned by `GET <userOrGroupPrefix>/fulltext?since=<version>` and in `Last-Modified-Version` are local versions, so partitioning is essential. Items whose content type Zotero doesn't index return `400`.

See [Basics](dev/web_api/v3/basics) for an overview of the API as a whole.
