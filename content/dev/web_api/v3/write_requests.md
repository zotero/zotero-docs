# Zotero Web API Write Requests

This page documents the write methods of the [Zotero Web API](dev/web_api/v3/). See the [Basics](dev/web_api/v3/basics) page for basic information on accessing the API, including possible HTTP status codes not listed here.

An [API key](dev/web_api/v3/basics#authentication) with write access to a given library is necessary to use write methods.

In Zotero 10+, the [local API](dev/web_api/v3/local_api) supports the same write methods against the local database. Writes there use a local API key granted by the user at runtime and check preconditions against local object versions; see [Local API](dev/web_api/v3/local_api#write_requests) for the differences.

## JSON Object Data

By default, objects returned from the API in `format=json` mode include a `data` property containing "editable JSON" — that is, all the object fields that can be modified and sent back to the server:

    {
      "key": "ABCD2345",
      "version": 1,
      "library": { ... },
      "links": { ... },
      "meta": { ... },
      "data": {
        "key": "ABCD2345",
        "version": 1,
        "itemType": "webpage",
        "title": "Zotero Quick Start Guide",
        "creators": [
            {
                "creatorType": "author",
                "name": "Center for History and New Media"
            }
        ],
        "abstractNote": "",
        "websiteTitle": "Zotero",
        "websiteType": "",
        "date": "",
        "shortTitle": "",
        "url": "https://www.zotero.org/support/quick_start_guide",
        "accessDate": "2014-06-12T21:28:55Z",
        "language": "",
        "rights": "",
        "extra": "",
        "dateAdded": "2014-06-12T21:28:55Z",
        "dateModified": "2014-06-12T21:28:55Z",
        "tags": [],
        "collections": [],
        "relations": {}
      }
    }

There are two ways to make changes to the provided JSON:

### Programmatic Approach

For programmatic access to the API, the recommended approach is to extract the editable JSON from the `data` property, make changes as necesssary, and upload just the editable JSON back to the API. For new items, an [empty template](dev/web_api/v3/types_and_fields#getting_a_template_for_a_new_item) of the editable JSON can be retrieved from the API.

This approach reduces upload bandwidth by sending only the data that is actually processed by the server. (For an even more efficient upload, the HTTP `PATCH` method, discussed below, can be used to send only the fields that have changed.) The examples in this documentation assume programmatic access.

### REST Approach

For more casual access, the API supports standard REST behavior, allowing the entire downloaded JSON to be reuploaded. This allows edits to be performed without writing a single line of code:

    $ URL="https://api.zotero.org/users/1234567/items"
    $ API_KEY="P9NiFoyLeZu2bZNvvuQPDWsd"
    $ curl -H "Zotero-API-Key: $API_KEY" $URL > items.json
    $ vi items.json  # edit the item data
    $ curl -H "Zotero-API-Key: $API_KEY" -d @items.json -v $URL

In this example, a JSON array of items is being saved to a text file, modified in a text editor, and then POSTed back to the same URL.

This approach allows a complicated task such as batch editing to be performed using only cURL and a text editor. Any objects modified in the text file will be updated on the server, while unmodified objects will be left unchanged.

A similar process can be used with PUT for individual objects:

    $ URL="https://api.zotero.org/users/1234567/items/ABCD2345"
    $ API_KEY="P9NiFoyLeZu2bZNvvuQPDWsd"
    $ curl -H "Zotero-API-Key: $API_KEY" $URL > item.json
    $ vi items.json  # edit the item data
    $ curl -H "Zotero-API-Key: $API_KEY" -X PUT -d @item.json -v $URL

Note that when uploading full JSON, only the `data` property is processed. All other properties (`library`, `links`, `meta`, etc.) are ignored.

## Item Requests

### Creating an Item

When creating a new item, first get empty JSON for the item type with an [item template request](dev/web_api/v3/types_and_fields#getting_a_template_for_a_new_item) (or use a cached version of the template). Then modify it and resubmit it to the server in an array:

    POST <userOrGroupPrefix>/items
    Content-Type: application/json
    Zotero-Write-Token: <write token> or If-Unmodified-Since-Version: <last library version>

    [
      {
        "itemType" : "book",
        "title" : "My Book",
        "creators" : [
          {
            "creatorType":"author",
            "firstName" : "Sam",
            "lastName" : "McAuthor"
          },
          {
            "creatorType":"editor",
            "name" : "John T. Singlefield"
          }
        ],
        "tags" : [
          { "tag" : "awesome" },
          { "tag" : "rad", "type" : 1 }
        ],
        "collections" : [
          "BCDE3456", "CDEF4567"
        ],
        "relations" : {
          "owl:sameAs" : "http://zotero.org/groups/1/items/JKLM6543",
          "dc:relation" : "http://zotero.org/groups/1/items/PQRS6789",
          "dc:replaces" : "http://zotero.org/users/1/items/BCDE5432"
        }
      }
    ]

All properties other than `itemType`, `tags`, `collections`, and `relations` are optional.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>200 OK</code></td><td>The request completed. See the response JSON for status of individual writes.</td></tr>
    <tr><td><code>400 Bad Request</code></td><td>Invalid type/field; unparseable JSON</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The version provided in <code>If-Unmodified-Since-Version</code> is out of date, or the provided <code>Zotero-Write-Token</code> has already been submitted.</td></tr>
    <tr><td><code>413 Request Entity Too Large</code></td><td>Too many items submitted</td></tr>
  </tbody>
</table>

`200 OK` response:

    {
      "success": {
        "0": "<itemKey>"
      },
      "unchanged": {},
      "failed": {},
      }
    }

See [Creating Multiple Objects](#creating_multiple_objects) for more information on the response format.

### Creating Multiple Items

See [Creating Multiple Objects](#creating_multiple_objects).

### Updating an Existing Item

To update an existing item, first retrieve the current version of the item:

    GET <userOrGroupPrefix>/items/<itemKey>

The editable data, similar to the item data shown above in [Creating an Item](#creating_an_item), will be found in the `data` property in the response.

The API supports two ways of modifying item data: by uploading full item data (`PUT`) or by sending just the data that changed (`PATCH`).

#### Full-item updating (PUT)

With `PUT`, you submit the item's complete editable JSON to the server, typically by modifying the downloaded editable JSON — that is, the contents of the `data` property — directly and resubmitting it:

    PUT <userOrGroupPrefix>/items/<itemKey>
    Content-Type: application/json

    {
      "key": "ABCD2345",
      "version": 1,
      "itemType" : "book",
      "title" : "My Amazing Book",
      "creators" : [
        {
          "creatorType":"author",
          "firstName" : "Sam",
          "lastName" : "McAuthor"
        },
        {
          "creatorType":"editor",
          "name" : "Jenny L. Singlefield"
        }
      ],
      "tags" : [
        { "tag" : "awesome" },
        { "tag" : "rad", "type" : 1 }
      ],
      "collections" : [
        "BCDE3456", "CDEF4567"
      ],
      "relations" : {
        "owl:sameAs" : "http://zotero.org/groups/1/items/JKLM6543",
        "dc:relation" : "http://zotero.org/groups/1/items/PQRS6789",
        "dc:replaces" : "http://zotero.org/users/1/items/BCDE5432"
      }
    }

All properties other than `itemType`, `tags`, `collections`, and `relations` are optional. Any existing fields not specified will be removed from the item. If `creators`, `tags`, `collections`, or `relations` are empty, any associated creators/tags/collections/relations will be removed from the item.

#### Partial-item updating (PATCH)

With `PATCH`, you can submit just the properties that have actually changed, for a potentially much more efficient operation. Properties not included in the uploaded JSON are left untouched on the server. To clear a property, pass an empty string or an empty array as appropriate.

    PATCH <userOrGroupPrefix>/items/<itemKey>
    If-Unmodified-Since-Version: <last item version>

    {
      "date" : "2013"
      "collections" : [
        "BCDE3456", "CDEF4567"
      ]
    }

This would add a `date` field to the item and add it in the two specified collections if not already present. Array properties are interpreted as complete lists, so omitting a collection key would cause the item to be removed from that collection.

The `PATCH` behavior is also available when [modifying multiple items](dev/web_api/v3/write_requests#updating_multiple_objects) via `POST`.

#### Both PUT and PATCH

Notes and attachments can be made child items by assigning the parent item's key to the `parentItem` property. If parent and child items are created in the same `POST` request, the child items must appear after the parent item in the array of items, with a locally created [item key](#object_keys).

The item's current version number is included in the `version` JSON property, as well as in the `Last-Modified-Version` header of single-item requests. `PUT` and `PATCH` requests must include the item's current version number in either the `version` property or the `If-Unmodified-Since-Version` header. (`version` is included in responses from the API, so clients that simply modify the editable data do not need to bother with a version header.) If the item has been changed on the server since the item was retrieved, the write request will be rejected with a `412 Precondition Failed` error, and the most recent version of the item will have to be retrieved from the server before changes can be made. See [Version Numbers](dev/web_api/v3/syncing#version_numbers) for more on library and object versioning.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The item was successfully updated.</td></tr>
    <tr><td><code>400 Bad Request</code></td><td>Invalid type/field; unparseable JSON</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The item has changed since retrieval (i.e., the provided item version no longer matches).</td></tr>
  </tbody>
</table>

### Updating Multiple Items

See [Updating Multiple Objects](#updating_multiple_objects).

### Deleting an Item

    DELETE <userOrGroupPrefix>/items/<itemKey>
    If-Unmodified-Since-Version: <last item version>

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The item was deleted.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The item has changed since retrieval (i.e., the provided item version no longer matches).</td></tr>
    <tr><td><code>428 Precondition Required</code></td><td><code>If-Unmodified-Since-Version</code> was not provided.</td></tr>
  </tbody>
</table>

### Deleting Multiple Items

Up to 50 items can be deleted in a single request.

    DELETE <userOrGroupPrefix>/items?itemKey=<key>,<key>,<key>
    If-Unmodified-Since-Version: <last library version>

    204 No Content
    Last-Modified-Version: <library version>

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The items were deleted.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The library has changed since the specified version.</td></tr>
    <tr><td><code>428 Precondition Required</code></td><td><code>If-Unmodified-Since-Version</code> was not provided.</td></tr>
  </tbody>
</table>

## Collection Requests

### Creating a Collection

    POST <userOrGroupPrefix>/collections
    Content-Type: application/json
    Zotero-Write-Token: <write token> or If-Unmodified-Since-Version: <last library version>

    [
      {
        "name" : "My Collection",
        "parentCollection" : "QRST9876"
      }
    ]

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>200 OK</code></td><td>The request completed without a general error. See the response JSON for status of individual writes.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The version provided in <code>If-Unmodified-Since-Version</code> is out of date, or the provided <code>Zotero-Write-Token</code> has already been submitted.</td></tr>
  </tbody>
</table>

### Updating an Existing Collection

    GET <userOrGroupPrefix>/collections/<collectionKey>

Editable JSON will be found in the `data` property.

    PUT <userOrGroupPrefix>/collections/<collectionKey>
    Content-Type: application/json

    {
      "key" : "DM2F65CA",
      "version" : 156,
      "name" : "My Collection",
      "parentCollection" : false
    }

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>200 OK</code></td><td>The collection was updated.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The collection has changed since retrieval (i.e., the provided collection version no longer matches).</td></tr>
  </tbody>
</table>

### Collection-Item Membership

Items can be added to or removed from collections via the `collections` property in the item JSON.

### Deleting a Collection

    DELETE <userOrGroupPrefix>/collections/<collectionKey>
    If-Unmodified-Since-Version: <last collection version>

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The collection was deleted.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The collection has changed since retrieval (i.e., the provided ETag no longer matches).</td></tr>
  </tbody>
</table>

### Deleting Multiple Collections

Up to 50 collections can be deleted in a single request.

    DELETE <userOrGroupPrefix>/collections?collectionKey=<collectionKey>,<collectionKey>,<collectionKey>
    If-Unmodified-Since-Version: <last library version>

    204 No Content
    Last-Modified-Version: <library version>

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The collections were deleted.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The library has changed since the specified version.</td></tr>
  </tbody>
</table>

## Saved Search Requests

### Creating a Search

    POST <userOrGroupPrefix>/searches
    Content-Type: application/json
    Zotero-Write-Token: <write token> or If-Unmodified-Since-Version: <last library version>

    [
      {
        "name": "My Search",
        "conditions": [
          {
            "condition": "title",
            "operator": "contains",
            "value": "foo"
          },
          {
            "condition": "date",
            "operator": "isInTheLast",
            "value": "7 days"
          }
        ]
      }
    ]

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>200 OK</code></td><td>The request completed without a general error. See the response JSON for status of individual writes.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The version provided in <code>If-Unmodified-Since-Version</code> is out of date, or the provided <code>Zotero-Write-Token</code> has already been submitted.</td></tr>
  </tbody>
</table>

### Deleting Multiple Searches

Up to 50 searches can be deleted in a single request.

    DELETE <userOrGroupPrefix>/searches?searchKey=<searchKey>,<searchKey>,<searchKey>
    If-Unmodified-Since-Version: <last library version>

    204 No Content
    Last-Modified-Version: <library version>

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The searches were deleted.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The library has changed since the specified version.</td></tr>
  </tbody>
</table>

## Tag Requests

### Deleting Multiple Tags

Up to 50 tags can be deleted in a single request.

    DELETE <userOrGroupPrefix>/tags?tag=<URL-encoded tag 1> || <URL-encoded tag 2> || <URL-encoded tag 3>
    If-Unmodified-Since-Version: <last library version>

    204 No Content
    Last-Modified-Version: <library version>

## Setting Requests

Synced settings are library-level values that Zotero clients keep in sync, such as colored tags and the last-read position in attachments. Unlike other objects, settings are identified by name rather than by an object key, and their JSON consists only of a `value` and a `version`:

    GET <userOrGroupPrefix>/settings

    200 OK
    Content-Type: application/json
    Last-Modified-Version: <library version>

    {
      "tagColors": {
        "value": [
          {
            "name": "Important",
            "color": "#FF6666"
          }
        ],
        "version": 1234
      },
      "lastPageIndex_u_ABCD2345": {
        "value": 12,
        "version": 1240
      }
    }

`GET <userOrGroupPrefix>/settings/<settingKey>` returns a single setting's `value` and `version`, with the setting's version in `Last-Modified-Version`, or `404 Not Found` if the setting doesn't exist. The `since` parameter and the `If-Modified-Since-Version` header work as they do for other objects (see [Syncing](dev/web_api/v3/syncing#version_numbers)).

### Available Settings

Only the following settings can be written. Writing any other setting returns `400 Bad Request`.

| Setting                                         | Value                                                                                                                                                                                                 |
|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `tagColors`                                     | Non-empty array of `{ "name": "<tag>", "color": "<color>" }` objects, in display order                                                                                                                |
| `feeds`                                         | Object containing the user's feeds, keyed by feed URL                                                                                                                                                 |
| `lastPageIndex_u_<itemKey>`                     | Last position in an attachment in the user's library: an integer page index, a string location (e.g., an EPUB CFI), or a percentage between 0 and 100 with at most one decimal place (for snapshots) |
| `lastPageIndex_g<groupID>_<itemKey>`            | Last position in an attachment in a group library, in the same format                                                                                                                                |
| `lastRead_g<groupID>_<itemKey>`                 | Integer timestamp of when the user last opened an attachment in a group library                                                                                                                       |
| `lastReadAloudPosition_u_<itemKey>`             | Object containing the last Read Aloud position in an attachment in the user's library                                                                                                                |
| `lastReadAloudPosition_g<groupID>_<itemKey>`    | Object containing the last Read Aloud position in an attachment in a group library                                                                                                                   |
| `attachmentRenameTemplate`                      | Non-empty string containing the template used to rename attachment files                                                                                                                             |
| `autoRenameFiles`                               | Boolean indicating whether attachment files are renamed automatically                                                                                                                                |
| `autoRenameFilesFileTypes`                      | Non-empty string containing a comma-separated list of the MIME types of files that are renamed automatically (e.g., `application/pdf,application/epub+zip`)                                          |
| `readerCustomThemes`                            | Non-empty array of custom reader themes                                                                                                                                                              |

Values can be at most 30,000 characters long (as serialized JSON for non-string values). To clear a setting, delete it; don't write an empty value.

`lastPageIndex_*` and `lastRead_*` settings are stored in the user's library, even for attachments in group libraries, and are rejected with `400` in group libraries.

In group libraries, `attachmentRenameTemplate`, `autoRenameFiles`, and `autoRenameFilesFileTypes` can only be changed by group owners and admins. Other group members receive `403 Forbidden` when writing or deleting them.

### Creating or Updating a Setting

    PUT <userOrGroupPrefix>/settings/<settingKey>
    Content-Type: application/json
    If-Unmodified-Since-Version: <setting version>

    {
      "value": [
        {
          "name": "Important",
          "color": "#FF6666"
        }
      ]
    }

    204 No Content
    Last-Modified-Version: <library version>

Updating an existing setting requires either an `If-Unmodified-Since-Version` header or a `version` property in the JSON, and returns `412 Precondition Failed` if the setting has been modified since that version. A new setting can be created without a version, or with version `0` to ensure that it doesn't already exist. A nonzero version for a setting that doesn't exist returns `404 Not Found`.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>204 No Content</code></td><td>The setting was saved, or the value was unchanged.</td></tr>
    <tr><td><code>400 Bad Request</code></td><td>The setting name isn't supported, or the value is invalid for the setting.</td></tr>
    <tr><td><code>403 Forbidden</code></td><td>The setting can only be changed by group admins.</td></tr>
    <tr><td><code>404 Not Found</code></td><td>A nonzero version was provided for a setting that doesn't exist.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The setting has changed since the specified version.</td></tr>
    <tr><td><code>428 Precondition Required</code></td><td>The setting exists, and neither <code>If-Unmodified-Since-Version</code> nor a JSON <code>version</code> property was provided.</td></tr>
  </tbody>
</table>

### Creating or Updating Multiple Settings

Up to 270 settings can be written in a single request.

    POST <userOrGroupPrefix>/settings
    Content-Type: application/json
    If-Unmodified-Since-Version: <last library version>

    {
      "tagColors": {
        "value": [
          {
            "name": "Important",
            "color": "#FF6666"
          }
        ]
      },
      "lastPageIndex_u_ABCD2345": {
        "value": 12,
        "version": 1240
      }
    }

    204 No Content
    Last-Modified-Version: <library version>

`If-Unmodified-Since-Version` is optional and checks the library version, returning `412 Precondition Failed` if the library has changed. Each setting can also include a `version` property, which is checked against that setting's version as in single-setting writes.

Unlike other multi-object writes, a settings write normally succeeds or fails as a whole: if any setting is invalid, the API returns an error for the request and no settings are saved.

The exception is a write to a group library that includes admin-only settings (see [Available Settings](#available_settings)) by a member who isn't a group admin. The API saves the other settings and returns `200 OK` with a [write report](#creating_multiple_objects) listing each restricted setting under `failed` with code `403`. Entries are indexed by the setting's position in the request object, with the setting name as `key`. The saved settings are not listed in the report.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>200 OK</code></td><td>Some settings could not be written because they're restricted to group admins. See the response JSON for details.</td></tr>
    <tr><td><code>204 No Content</code></td><td>The settings were saved.</td></tr>
    <tr><td><code>400 Bad Request</code></td><td>A setting name isn't supported, or a value is invalid for its setting.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The library or a setting has changed since the specified version.</td></tr>
    <tr><td><code>413 Request Entity Too Large</code></td><td>More than 270 settings were provided.</td></tr>
  </tbody>
</table>

### Deleting a Setting

    DELETE <userOrGroupPrefix>/settings/<settingKey>

    204 No Content
    Last-Modified-Version: <library version>

Deleting a setting that doesn't exist returns `404 Not Found`.

### Deleting Multiple Settings

Up to 100 settings can be deleted in a single request.

    DELETE <userOrGroupPrefix>/settings?settingKey=<settingKey>,<settingKey>,<settingKey>
    If-Unmodified-Since-Version: <last library version>

    204 No Content
    Last-Modified-Version: <library version>

`If-Unmodified-Since-Version` is optional and returns `412 Precondition Failed` if the library has changed since the specified version.

Deleted settings are listed under `settings` in `<userOrGroupPrefix>/deleted?since=<version>`.

## Multi-Object Requests

### Creating Multiple Objects

Up to 50 collections, saved searches, or items can be created in a single request by including multiple objects in an array:

    POST <userOrGroupPrefix>/collections
    Content-Type: application/json
    Zotero-Write-Token: <write token> or If-Unmodified-Since-Version: <last library version>

    [
      {
        "name" : "My Collection",
        "parentCollection": "QRST9876"
      },
      {
        "name": "My Other Collection"
      }
    ]

For [syncing](dev/web_api/v3/syncing) objects with predetermined keys, an [object key](#object_keys) can also be provided with new objects.

`200` response:

    Content-Type: application/json
    Last-Modified-Version: <library version>

    {
      "successful": {
        "0": "<saved object>",
        "2": "<saved object>"
      },
      "unchanged": {
        "4": "<objectKey>"
      }
      "failed": {
        "1": {
          "key": "<objectKey>",
          "code": <HTTP response code>,
          "message": "<error message>"
        },
        "3": {
          "key": "<objectKey>",
          "code": <HTTP response code>,
          "message": "<error message>"
        },
      }
    }

The keys of the `successful`, `unchanged`, and `failed` objects are the numeric indexes of the Zotero objects in the uploaded array. The `Last-Modified-Version` is the version that has been assigned to any Zotero objects in the `successful` object — that is, objects that were modified in this request.

<table>
  <thead>
    <tr><th colspan="2">Common responses</th></tr>
  </thead>
  <tbody>
    <tr><td><code>200 OK</code></td><td>The objects were uploaded.</td></tr>
    <tr><td><code>409 Conflict</code></td><td>The target library is locked.</td></tr>
    <tr><td><code>412 Precondition Failed</code></td><td>The version provided in <code>If-Unmodified-Since-Version</code> is out of date, or the provided <code>Zotero-Write-Token</code> has already been submitted.</td></tr>
  </tbody>
</table>

#### Updating Multiple Objects

Up to 50 collections, saved searches, or items can be updated in a single request.

Follow the instructions in [Creating Multiple Objects](#creating_multiple_objects), but include a `key` and `version` property in each object. If modifying editable JSON returned from the API, these properties will already exist and shouldn't be modified. As an alternative to individual `version` properties, the last-known library version can be passed via the `If-Unmodified-Since-Version` HTTP Header.

Items can also include `dateAdded` and `dateModified` properties containing timestamps in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) (e.g., "2014-06-10T13:52:43Z"). If `dateAdded` is included with an existing item, it must match the existing `dateAdded` value or else the API will return a 400 error. If a new `dateModified` time is not included with an update to existing item, the item's `dateModified` value will be set to the current time. Editable JSON returned from the API includes `dateAdded` and `dateModified` in the correct format, so clients that are content with server-set modification times can simply ignore these properties.

    POST <userOrGroupPrefix>/collections
    Content-Type: application/json

    [
      {
        "key": "BD85JEM4",
        "version": 414,
        "name": "My Collection",
        "parentCollection": false
      },
      {
        "key": "MNC5SAPD",
        "version": 416
        "name": "My Subcollection",
        "parentCollection": "BD85JEM4"
      }
    ]

The response is the same as that in [Creating Multiple Objects](#creating_multiple_objects).

Note that `POST` follows `PATCH` semantics, meaning that any properties not specified will be left untouched on the server. To erase an existing property, include it with an empty string or `false` as the value.

## Object Keys

While the server will automatically generate valid keys for uploaded objects, in some situations, such as when [syncing](dev/web_api/v3/syncing) or creating a parent and child item in the same request, it may be desirable or necessary to create object keys locally.

Local object keys should conform to the pattern `/[23456789ABCDEFGHIJKLMNPQRSTUVWXYZ]{8}/`.

## Zotero-Write-Token

    Zotero-Write-Token: 19a4f01ad623aa7214f82347e3711f56

`Zotero-Write-Token` is an optional HTTP header, containing a client-generated random 32-character identifier string, that can be included with unversioned write requests to prevent them from being processed more than once (e.g., if a user clicks a form submit button twice). The Zotero server caches write tokens for successful requests for 12 hours, and subsequent requests from the same API key using the same write token will be rejected with a `412 Precondition Failed` status code. If a request fails, the write token will not be stored.

If using [versioned write requests](dev/web_api/v3/syncing#version_numbers) (i.e., those that include an `If-Unmodified-Since-Version` HTTP header or individual object `version` properties), `Zotero-Write-Token` is redundant and should be omitted.

In Zotero 10+, the [local API](dev/web_api/v3/local_api) also caches write tokens for 12 hours, but only in memory, so a token is forgotten if Zotero restarts.

## Examples

See the [Syncing](dev/web_api/v3/syncing) page for an example workflow that puts together read and write methods for complete and efficient syncing of Zotero data via the API.
