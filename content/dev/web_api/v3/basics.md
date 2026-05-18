# Zotero Web API Documentation

This page documents read requests available in the [Zotero Web API](dev/web_api/v3/), providing read-only access to online Zotero libraries. The Zotero desktop client exposes the same endpoints as a [local API](dev/web_api/v3/local_api) that serves data from the local database, which is useful for code running on the user's computer.

## Base URL

The base URL for all API requests is

    https://api.zotero.org

All requests must use HTTPS. (The local API uses `http://localhost:23119/api/` instead; see [Local API](dev/web_api/v3/local_api).)

## API Versioning

Multiple versions of the API are available, and production code should always request a specific version. This page documents API version 3, which is currently the default and recommended version.

Clients can request a specific version in one of two ways:

1.  Via the `Zotero-API-Version` HTTP header (`Zotero-API-Version: 3`)
2.  Via the `v` query parameter (`v=3`)

Use of the `Zotero-API-Version` header is recommended for production code. The `v` parameter can be used for easier debugging and sharing of API requests or in clients that can't pass arbitrary HTTP headers.

The API version of a response will be returned in the `Zotero-API-Version` response header.

### Version Transitions

At most times, API changes are made in a backwards-compatible manner. Occasionally, however, backwards-incompatible changes may need to be made. When this occurs, a new API version will be made available without changing the default version for clients that don't request a specific version. After a transition period, the new API version will become the default. If an API version is discontinued, clients requesting the discontinued version will receive the oldest available version. Announcements regarding API version transitions will always be made ahead of time on [zotero-dev](https://groups.google.com/group/zotero-dev).

## Authentication

Authentication is not required for read access to public libraries.

Accessing non-public libraries requires use of an API key. Third-party developers should [use OAuth to establish credentials](dev/web_api/v3/oauth) or instruct their users to create dedicated keys for use with their services. End users can create API keys via [their Zotero account settings](/settings/keys/new).

API keys can be included in requests in one of three ways:

1.  As an HTTP header in the form `Zotero-API-Key: P9NiFoyLeZu2bZNvvuQPDWsd`
2.  As an HTTP header in the form `Authorization: Bearer P9NiFoyLeZu2bZNvvuQPDWsd`
3.  As a URL query parameter, in the form `key=P9NiFoyLeZu2bZNvvuQPDWsd` (not recommended)

Use of an HTTP header is recommended, as it allows use of URLs returned from the API (e.g., for pagination) without modification.

`Authorization: Bearer` is also the authentication mechanism for OAuth 2.0. While Zotero currently supports only OAuth 1.0a, when support for OAuth 2.0 is added, clients will no longer need to extract the API key from the OAuth response and pass it to the API separately.

The [local API](dev/web_api/v3/local_api) does not use authentication.

## Local API

The Zotero desktop client exposes a local implementation of this API on `localhost:23119` under `/api/`, serving data from the user's local database. See [Local API](dev/web_api/v3/local_api) for details. The sections below note where local API behavior diverges from the Web API.

## Resources

### User and Group Library URLs

Requests for data in a specific library begin with `/users/<userID>` or `/groups/<groupID>`, referred to below as &lt;userOrGroupPrefix&gt;. User IDs are different from usernames and can be found on the [API Keys](/settings/keys) page and in OAuth responses. Group IDs are different from group names and can be retrieved from `/users/<userID>/groups`.

#### Collections

| URI                                                           | Description                                                |
|---------------------------------------------------------------|------------------------------------------------------------|
| &lt;userOrGroupPrefix&gt;/collections                              | Collections in the library                                 |
| &lt;userOrGroupPrefix&gt;/collections/top                          | Top-level collections in the library                       |
| &lt;userOrGroupPrefix&gt;/collections/&lt;collectionKey&gt;             | A specific collection in the library                       |
| &lt;userOrGroupPrefix&gt;/collections/&lt;collectionKey&gt;/collections | Subcollections within a specific collection in the library |

#### Items

| URI                                                         | Description                                                 |
|-------------------------------------------------------------|-------------------------------------------------------------|
| &lt;userOrGroupPrefix&gt;/items                                  | All items in the library, excluding trashed items           |
| &lt;userOrGroupPrefix&gt;/items/top                              | Top-level items in the library, excluding trashed items     |
| &lt;userOrGroupPrefix&gt;/items/trash                            | Items in the trash                                          |
| &lt;userOrGroupPrefix&gt;/items/&lt;itemKey&gt;                       | A specific item in the library                              |
| &lt;userOrGroupPrefix&gt;/items/&lt;itemKey&gt;/children              | Child items under a specific item                           |
| &lt;userOrGroupPrefix&gt;/publications/items                     | Items in My Publications                                    |
| &lt;userOrGroupPrefix&gt;/collections/&lt;collectionKey&gt;/items     | Items within a specific collection in the library           |
| &lt;userOrGroupPrefix&gt;/collections/&lt;collectionKey&gt;/items/top | Top-level items within a specific collection in the library |

#### Searches

(Note: Only search metadata is currently available, not search results.)

| URI                                        | Description                            |
|--------------------------------------------|----------------------------------------|
| &lt;userOrGroupPrefix&gt;/searches              | All saved searches in the library      |
| &lt;userOrGroupPrefix&gt;/searches/&lt;searchKey&gt; | A specific saved search in the library |

#### Tags

| URI                                                              | Description                                                            |
|------------------------------------------------------------------|------------------------------------------------------------------------|
| &lt;userOrGroupPrefix&gt;/tags                                        | All tags in the library                                                |
| &lt;userOrGroupPrefix&gt;/tags/<url+encoded+tag>                     | Tags of all types matching a specific name                             |
| &lt;userOrGroupPrefix&gt;/items/&lt;itemKey&gt;/tags                       | Tags associated with a specific item                                   |
| &lt;userOrGroupPrefix&gt;/collections/&lt;collectionKey&gt;/tags           | Tags within a specific collection in the library                       |
| &lt;userOrGroupPrefix&gt;/items/tags                                  | All tags in the library, with the ability to filter based on the items |
| &lt;userOrGroupPrefix&gt;/items/top/tags                              | Tags assigned to top-level items                                       |
| &lt;userOrGroupPrefix&gt;/items/trash/tags                            | Tags assigned to items in the trash                                    |
| &lt;userOrGroupPrefix&gt;/collections/&lt;collectionKey&gt;/items/tags     | Tags assigned to items in a given collection                           |
| &lt;userOrGroupPrefix&gt;/collections/&lt;collectionKey&gt;/items/top/tags | Tags assigned to top-level items in a given collection                 |
| &lt;userOrGroupPrefix&gt;/publications/items/tags                     | Tags assigned to items in My Publications                              |

### Other URLs

<table>
<thead>
<tr class="header">
<th>URI</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>/keys/&lt;key&gt;</td>
<td>The user id and privileges of the given API key.<br />
Use the <code>DELETE</code> HTTP method to delete the key. This should generally be done only by a client that created the key originally using <a href="/dev/web_api/v3/OAuth">OAuth</a>.</td>
</tr>
<tr class="even">
<td>/users/&lt;userID&gt;/groups</td>
<td>The set of groups the current API key has access to, including public groups the key owner belongs to even if the key doesn't have explicit permissions for them.</td>
</tr>
</tbody>
</table>

## Read Requests

The following parameters affect the format of data returned from read requests. All parameters are optional.

### General Parameters

The following parameters are valid for all read requests:

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Values</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>format</code></td>
<td><code>atom</code>, <code>bib</code>, <code>json</code>, <code>keys</code>, <code>versions</code>, <a href="#export_formats">export format</a></td>
<td><code>json</code> (or <code>atom</code> if the <code>Accept</code> header includes <code>application/atom+xml</code>)</td>
<td><code>atom</code> will return an Atom feed suitable for use in feed readers or feed-reading libraries.<br />
<code>bib</code>, valid only for item requests, will return a formatted bibliography as XHTML. <code>bib</code> mode is currently limited to a maximum of 150 items.<br />
<code>json</code> will return a JSON array for multi-object requests and a single JSON object for single-object requests.<br />
<code>keys</code>, valid for multi-object requests, will return a newline-separated list of object keys. <code>keys</code> mode has no default or maximum limit.<br />
<code>versions</code>, valid for multi-object collection, item, and search requests, will return a JSON object with Zotero object keys as keys and object versions as values. Like <code>keys</code>, <code>versions</code> mode has no default or maximum limit.<br />
Export formats, valid only for item requests, produce output in the specified format.</td>
</tr>
</tbody>
</table>

The [local API](dev/web_api/v3/local_api) does not support `format=atom`; requests using it return `501 Not Implemented`. All other formats are supported.

### Parameters for "format=json"

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Values</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>include</code></td>
<td><code>bib</code>, <code>citation</code>, <code>data</code>, <a href="#export_formats">export format</a><br />
Multiple formats can be specified by using a comma as the delimiter (<code>include=data,bib</code>).</td>
<td><code>data</code></td>
<td>Formats to include in the response:<br />
<code>bib</code>, valid only for item requests, will return a formatted reference for each item.<br />
<code>citation</code>, valid only for item requests, will return a formatted citation for each item.<br />
<code>data</code> (the default) will include all writeable fields in JSON format, suitable for modifying and sending back to the API.<br />
Export formats, valid only for item requests, will return data in the specified format for each item.</td>
</tr>
</tbody>
</table>

### Parameters for "format=atom"

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Values</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>content</code></td>
<td><code>bib</code>, <code>citation</code>, <code>html</code>, <code>json</code>, <a href="#export_formats">export formats</a>, <code>none</code><br />
Multiple formats can be specified by using a comma as the delimiter (<code>content=json,bib</code>).</td>
<td><code>html</code></td>
<td>The format of the Atom response's <code>&lt;content&gt;</code> node:<br />
<code>html</code> (the default) will return an XHTML representation of each object, useful for display in feed readers and for parsing by XML tools.<br />
<code>json</code>, currently valid only for item and collection requests, will return a JSON representation of all the item's fields.<br />
<code>bib</code>, valid only for item requests, will return a formatted reference for each item.<br />
<code>citation</code>, valid only for item requests, will return a formatted citation for each item.<br />
Export formats, valid only for item requests, will return data in the specified format for each item.<br />
If additional data is not required, use <code>none</code> to decrease the response size.<br />
If multiple formats are requested, <code>&lt;content&gt;</code> will contain multiple <code>&lt;zapi:subcontent&gt;</code> elements (in the http://zotero.org/ns/api namespace), each with a <code>zapi:type</code> attribute matching one of the specified content parameters.</td>
</tr>
</tbody>
</table>

The Atom parameters above do not apply to the [local API](dev/web_api/v3/local_api), which does not produce Atom output.

### Parameters for "format=bib", "include/content=bib", "include/content=citation"

| Parameter  | Values  | Default                     | Description                                                                                                                                                                                                                                   |
|------------|---------|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `style`    | string  | `chicago-note-bibliography` | Citation style to use for formatted references. Can be either the file name (without the .csl extension) of one of the styles in the [Zotero Style Repository](https://www.zotero.org/styles/) (e.g., `apa`) or the URL of a remote CSL file. |
| `linkwrap` | boolean | `0`                         | Set to `1` to return URLs and DOIs as links                                                                                                                                                                                                   |
| `locale`   | string  | `en-US`                     | Bibliography locale. See the [available CSL locales](https://github.com/citation-style-language/locales). Note that some styles use a fixed locale and cannot be localized.                                                                   |

Note the difference between `format=bib` and `include=bib`/`content=bib`. `format=bib` returns a formatted bibliography as XHTML, sorted according to the rules of the selected style. `include=bib` (valid only for `format=json` (the default format mode) and `format=atom`) returns an individual formatted reference within the JSON `data` block or Atom &lt;content&gt; block for each item, with the results or feed sorted according to the query parameters. `format=bib` processes the entire feed you are requesting without regard for any limit arguments, so it is generally a good idea to use it only with collections or tags.

### Item Export Formats

The following bibliographic data formats can be used as `format`, `include`, and `content` parameters for items requests:

| Parameter           | Description                                                                                                        |
|---------------------|--------------------------------------------------------------------------------------------------------------------|
| `bibtex`            | BibTeX                                                                                                             |
| `biblatex`          | BibLaTeX                                                                                                           |
| `bookmarks`         | Netscape Bookmark File Format                                                                                      |
| `coins`             | COinS                                                                                                              |
| `csljson`           | [Citation Style Language data format](https://github.com/citation-style-language/schema/blob/master/csl-data.json) |
| `csv`               | CSV                                                                                                                |
| `mods`              | MODS                                                                                                               |
| `refer`             | Refer/BibIX                                                                                                        |
| `rdf_bibliontology` | [Bibliographic Ontology](http://bibliontology.com/) RDF                                                            |
| `rdf_dc`            | Unqualified Dublin Core RDF                                                                                        |
| `rdf_zotero`        | Zotero RDF                                                                                                         |
| `ris`               | RIS                                                                                                                |
| `tei`               | Text Encoding Initiative (TEI)                                                                                     |
| `wikipedia`         | Wikipedia Citation Templates                                                                                       |

## Searching

### Search Parameters

| Parameter  | Values                          | Default | Description                                                                                                                                                                    |
|------------|---------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `itemKey`  | string                          | null    | A comma-separated list of item keys. Valid only for item requests. Up to 50 items can be specified in a single request.                                                        |
| `itemType` | [search syntax](#search_syntax) | null    | Item type search                                                                                                                                                               |
| `q`        | string                          | null    | Quick search. Searches titles and individual creator fields by default. Use the `qmode` parameter to change the mode. Currently supports phrase searching only.                |
| `since`    | integer                         | `0`     | Return only objects modified after the specified library version, returned in a previous `Last-Modified-Version` header. See [Syncing](dev/web_api/v3/syncing) for more info. |
| `tag`      | [search syntax](#search_syntax) | null    | Tag search                                                                                                                                                                     |

The [local API](dev/web_api/v3/local_api) accepts the same search parameters but uses Zotero's local quicksearch implementation, so the set of items returned by a given `q` value may not match the Web API exactly.

### Search Parameters (Items Endpoints)

| Parameter        | Values                           | Default                  | Description                                                                                                                  |
|------------------|----------------------------------|--------------------------|------------------------------------------------------------------------------------------------------------------------------|
| `includeTrashed` | `0`, `1`                         | `0` (except in `/trash`) | Include items in the trash                                                                                                   |
| `qmode`          | `titleCreatorYear`, `everything` | `titleCreatorYear`       | Quick search mode. To include full-text content, use `everything`. Searching of other fields will be possible in the future. |

### Search Parameters (Tags Endpoints)

| Parameter | Values                   | Default    | Description                                                          |
|-----------|--------------------------|------------|----------------------------------------------------------------------|
| `qmode`   | `contains`, `startsWith` | `contains` | Quick search mode. To perform a left-bound search, use `startsWith`. |

### Search Parameters (Tags-Within-Items Endpoints)

These parameters can be used to search against items when returning tags within items. In such cases, the main parameters (`q`, `qmode`, `tag`) apply to the tags themselves, as the primary objects of the request.

| Parameter   | Values                          | Default    | Description                           |
|-------------|---------------------------------|------------|---------------------------------------|
| `itemQ`     | string                          | null       | Same as `q` in an `items` request     |
| `itemQMode` | `contains`, `startsWith`        | `contains` | Same as `qmode` in an `items` request |
| `itemTag`   | [search syntax](#search_syntax) | null       | Same as `tag` in an `items` request   |

### Search Syntax

`itemType` and `tag` parameters support Boolean searches:

Examples:

-   `itemType=book`
-   `itemType=book || journalArticle` (OR)
-   `itemType=-attachment` (NOT)
-   `tag=foo`
-   `tag=foo bar` (tag with space)
-   `tag=foo&tag=bar` (AND)
-   `tag=foo bar || bar` (OR)
-   `tag=-foo` (NOT)
-   `tag=\-foo` (literal first-character hyphen)

Be sure to URL-encode search strings if required by your client or library.

## Sorting and Pagination

### Sorting and Pagination Parameters

The following parameters are valid only for multi-object read requests such as `<userOrGroupPrefix>/items`, with the exception of `format=bib` requests, which do not support sorting or pagination.

| Parameter   | Values                                                                                                                                                                                                                        | Default                               | Description                                                                                                 |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------|-------------------------------------------------------------------------------------------------------------|
| `sort`      | `dateAdded`, `dateModified`, `title`, `creator`, `itemType`, `date`, `publisher`, `publicationTitle`, `journalAbbreviation`, `language`, `accessDate`, `libraryCatalog`, `callNumber`, `rights`, `addedBy`, `numItems` (tags) | `dateModified` (`dateAdded` for Atom) | The name of the field by which entries are sorted                                                           |
| `direction` | `asc`, `desc`                                                                                                                                                                                                                 | varies by `sort`                      | The sorting direction of the field specified in the `sort` parameter                                        |
| `limit`     | integer 1-100\*                                                                                                                                                                                                               | `25`                                  | The maximum number of results to return with a single request. Required for export formats.                 |
| `start`     | integer                                                                                                                                                                                                                       | `0`                                   | The index of the first result. Combine with the limit parameter to select a slice of the available results. |

The [local API](dev/web_api/v3/local_api) does not impose a default or maximum `limit`. If `limit` is omitted, all matching objects are returned in one response. Pagination parameters and `Link` headers still work for clients that want them.

#### Total Results

Responses for multi-object read requests will include a custom HTTP header, `Total-Results`, that provides the total number of results matched by the request. In the Web API, the actual number of results provided in a given response will be no more than 100.

#### Link Header

When the total number of results matched by a read request is greater than the current limit, the API will include pagination links in the HTTP `Link` header. Possible values are `rel=first`, `rel=prev`, `rel=next`, and `rel=last`. For some requests, the header may also include a `rel=alternate` link for the relevant page on the Zotero website.

    GET https://api.zotero.org/users/12345/items?limit=30

    Link: <https://api.zotero.org/users/12345/items?limit=30&start=30>; rel="next",
     <https://api.zotero.org/users/12345/items?limit=30&start=5040>; rel="last",
     <https://www.zotero.org/users/12345/items>; rel="alternate"

(Newlines are inserted here for clarity.)

## Caching

For efficient usage of the API, clients should make conditional GET requests whenever possible. Multi-object requests (e.g., `/users/1/items`) return a `Last-Modified-Version` header with the current library version. If a `If-Modified-Since-Version: <libraryVersion>` header is passed with a subsequent multi-object read request and data has not changed in the library since the specified version, the API will return `304 Not Modified` instead of `200`. (Single-object conditional requests are not currently supported, but will be supported in the future.)

While a conditional GET request that returns a `304` should be fast, some clients may wish or need to perform additional caching on their own, using stored data for a period of time before making subsequent conditional requests to the Zotero API. This makes particular sense when the underlying Zotero data is known not to change frequently or when the data will be accessed frequently. For example, a website that displayed a bibliography from a Zotero collection might cache the returned bibliography for an hour, after which time it would make another conditional request to the Zotero API. If the API returned a `304`, the website would continue to display the cached bibliography for another hour before retrying. This would prevent the website from making a request to the Zotero API every time a user loaded a page.

In addition to making conditional requests, clients downloading data for entire Zotero libraries should use `?since=` to request only objects that have changed since the last time data was downloaded.

See [Syncing](dev/web_api/v3/syncing) for more information on library and object versioning.

Conditional requests work the same way against the [local API](dev/web_api/v3/local_api), but local responses are already inexpensive to produce, so aggressive caching on the client side is less important. The `?since=` parameter is also supported and is the preferred way to fetch only changed objects from a large local library.

## Rate Limiting

Clients accessing the Zotero API should be prepared to handle two forms of rate limiting: backoff requests and hard limiting.

### Backoff

If the API servers are overloaded, the API may include a `Backoff: <seconds>` HTTP header in responses, indicating that the client should perform the minimum number of requests necessary to maintain data consistency and then refrain from making further requests for the number of seconds indicated. `Backoff` can be included in any response, including successful ones.

### 429 Too Many Requests

If a client has made too many requests within a given time period or is making too many concurrent requests, the API may return `429 Too Many Requests`, potentially with a `Retry-After: <seconds>` header. Clients receiving a `429` should wait at least the number of seconds indicated in the header before making further requests, or to perform an exponential backoff if `Retry-After` isn't provided. They should also reduce their overall request rate and/or concurrency to avoid repeatedly getting 429s, which may result in stricter throttling or temporary blocks.

For clients repeatedly hitting rate limits, the best solution is often to use a different request flow, such as using [batched writes](dev/web_api/v3/write_requests#updating_multiple_objects) rather individually updating a large number of objects. Clients should also generally make no more than 4 concurrent requests.

`Retry-After` can also be included with `503 Service Unavailable` responses when the server is undergoing maintenance.

## Example GET Requests and Responses

Several examples of read request URLs and their responses:

<table>
  <thead>
    <tr><th colspan="2">Multi-object JSON response: top-level items in a collection</th></tr>
  </thead>
  <tbody>
    <tr><td>Request</td><td><https://api.zotero.org/users/475425/collections/9KH9TNSJ/items/top?v=3></td></tr>
    <tr><td>Response</td><td><https://gist.github.com/6eeace93a5c29775d39c></td></tr>
  </tbody>
</table>

<table>
  <thead>
    <tr><th colspan="2">Single-object JSON response: individual item</th></tr>
  </thead>
  <tbody>
    <tr><td>Request</td><td><https://api.zotero.org/users/475425/items/X42A7DEE?v=3></td></tr>
    <tr><td>Response</td><td><https://gist.github.com/f1030b9609aadc51ddec></td></tr>
  </tbody>
</table>

<table>
  <thead>
    <tr><th colspan="2">Multi-object JSON response: collections for a user</th></tr>
  </thead>
  <tbody>
    <tr><td>Request</td><td><https://api.zotero.org/users/475425/collections?v=3></td></tr>
    <tr><td>Response</td><td><https://gist.github.com/0bc17ca64ee7d3bc9063></td></tr>
  </tbody>
</table>

<table>
  <thead>
    <tr><th colspan="2">Atom feed: items in a library</th></tr>
  </thead>
  <tbody>
    <tr><td>Request</td><td><https://api.zotero.org/users/475425/items?format=atom&v=3></td></tr>
    <tr><td>Response</td><td><https://gist.github.com/24172188ea79efa210b5></td></tr>
  </tbody>
</table>

<table>
  <thead>
    <tr><th colspan="2">Formatted bibliography: items in a collection</th></tr>
  </thead>
  <tbody>
    <tr><td>Request</td><td><https://api.zotero.org/users/475425/collections/9KH9TNSJ/items?format=bib></td></tr>
    <tr><td>Response</td><td><https://gist.github.com/77bc2413cce4c219f862></td></tr>
  </tbody>
</table>

The [local API](dev/web_api/v3/local_api) does not currently apply rate limiting.

## HTTP Status Codes

Successful GET requests will return a `200 OK` status code.

[Conditional GET requests](#caching) may return `304 Not Modified`.

For any read or write request, the server may return a `400 Bad Request`, `404 Not Found`, or `405 Method Not Allowed` for an invalid request and `500 Internal Server Error` or `503 Service Unavailable` for a server-related issue. [Authentication](#authentication) errors (e.g., invalid API key or insufficient privileges) will return a `403 Forbidden`.

Passing an `Expect` header, which is unsupported, will result in a `417 Expectation Failed` response.

[Library/object versioning](dev/web_api/v3/syncing#version_numbers) or [Zotero-Write-Token](dev/web_api/v3/write_requests#zotero_write_token) errors will result in `412 Precondition Failed` or `428 Precondition Required`.

`429 Too Many Requests` indicates that the client has been [rate-limited](#rate_limiting).

The [local API](dev/web_api/v3/local_api) returns `403 Forbidden` when the local API preference is not enabled and `501 Not Implemented` when a request asks for something it does not support (Atom output, or an API version other than 3 on an endpoint other than `/api/`).

## Additional API Documentation

-   [Local API](dev/web_api/v3/local_api)
-   [Write Requests](dev/web_api/v3/write_requests)
-   [File Uploads](dev/web_api/v3/file_upload)
-   [Syncing](dev/web_api/v3/syncing)
-   [OAuth Authentication](dev/web_api/v3/oauth)
-   [Changes from API Version 2](dev/web_api/v3/changes_from_v2)
