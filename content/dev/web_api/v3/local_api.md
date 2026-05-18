# Zotero Local API

Recent versions of the Zotero desktop client expose a local implementation of the [Zotero Web API](dev/web_api/v3/basics) on `localhost:23119` under `/api/`, serving data from the user's local database. Because nothing touches the network, the local API works offline, has no rate limits, and is typically much faster than the Web API. It is intended for code running on the user's own computer, like utilities and command-line tools, that would otherwise need to read directly from the SQLite database or fall back to the Web API.

The local API must be enabled in Zotero's preferences (Settings → Advanced → "Allow other applications on this computer to communicate with Zotero"). Requests will return `403 Forbidden` if it is not enabled.

The base URL is

    http://localhost:23119/api/

Most endpoints documented on the [Basics](dev/web_api/v3/basics) page work identically when accessed under that prefix. The notable differences from the Web API are:

-   Only API version 3 is supported, and only one version will ever be supported at a time. If a future version is released and your client needs to work against both old and new copies of Zotero, request `/api/` first and read the `Zotero-API-Version` response header to determine which version the running client speaks before making further requests.
-   Write requests are currently unsupported. Only `GET` is accepted. (An upcoming version of Zotero will support write requests.)
-   There is no authentication. Anyone with access to the loopback interface can read the user's library, so do not forward the port or otherwise expose it externally.
-   Only data for the locally logged-in user is available. Pass `0` as the user ID or the user's actual numeric ID, which can be found on the [API Keys](/settings/keys) page. Requests for any other user ID return `400`.
-   Group metadata is limited to what's needed to identify the group; permissions, member lists, and similar details are not included.
-   Atom is not supported. Requests with `format=atom` or `content=atom` return `501 Not Implemented`.
-   Item type and field endpoints (see [Item Types and Fields](dev/web_api/v3/types_and_fields)) return localized names in the user's locale. The `locale` query parameter is ignored. `/api/creatorFields` is the exception and always returns English names, matching the Web API.
-   Results are not paginated by default. The local API will return the full set of matching objects in one response, since nothing has to be transferred over the network. The `limit` and `start` parameters still work if you want them, and `Link` headers are still included.
-   The implementation aims to match the Web API's documented behavior but does not attempt to replicate every implementation detail. Sort order on equal keys, quicksearch matching, and the exact JSON produced for an object may differ in minor ways. Clients that depend on undefined behavior or unusual corner cases of the Web API should be tested against both implementations.

The local API also supports a few things the Web API does not:

-   `<userOrGroupPrefix>/searches/<searchKey>/items` returns the items matching a saved search. The Web API exposes search metadata but does not actually execute searches.
-   `<userOrGroupPrefix>/items/<itemKey>/file` returns a `302` redirect to a `file://` URL for the attachment on disk, and `/file/view` does the same. `/file/view/url` returns the URL as plain text rather than redirecting.

Responses include a `Zotero-Schema-Version` header reflecting the schema version of the local Zotero instance, which may lag behind or run ahead of the version served by the Web API.

See [Basics](dev/web_api/v3/basics) for an overview of the API as a whole.
