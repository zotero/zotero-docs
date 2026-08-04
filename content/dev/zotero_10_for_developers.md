# Zotero 10 for Developers

Zotero 10, currently in [beta](https://www.zotero.org/support/beta_builds), includes changes to internal APIs and data storage that may require updates to your plugin. This page covers the changes most likely to affect plugins. New user-facing features are covered in the beta announcements in the [Zotero Forums](https://forums.zotero.org/) and aren't repeated here.

Zotero 10 uses the same Firefox 140 ESR base as Zotero 9, so there are no changes to the Mozilla platform itself.

If you have questions about any of these changes, please post to [zotero-dev](https://groups.google.com/g/zotero-dev).

## Updating plugin compatibility

After confirming that your plugin is compatible, update `strict_max_version` in your manifest.json to `10.0.*`. If no changes are required, you can simply update `strict_max_version` in your plugin's update manifest without releasing a new version.

Note that beta and source builds no longer enforce `strict_max_version`, either at install time or for updates, so you can test a plugin with a lower max version on the Zotero 10 beta without modifying it. Stable builds still enforce it.

## Multiple selection in the collections list

Zotero 10 allows multiple collections, searches, libraries, and other rows to be selected at once, which changes several APIs that previously assumed a single selected row.

### Singular selection getters

Singular selection getters now throw, with an error message naming the replacement, so that plugins don't silently try to operate on one arbitrary row of a multi-row selection:

| Removed | Replacement |
| --- | --- |
| `ZoteroPane.getCollectionTreeRow()` | `ZoteroPane.getCollectionTreeRows()` |
| `ZoteroPane.getSelectedLibraryID()` | `ZoteroPane.getSelectedLibraryIDs()` (new) |
| `ZoteroPane.getSelectedCollection()` | `ZoteroPane.getSelectedCollections()` |
| `ZoteroPane.getSelectedSavedSearch()` | `ZoteroPane.getSelectedSavedSearches()` |
| `ZoteroPane.getSelectedGroup()` | filter `ZoteroPane.getCollectionTreeRows()` by `isGroup()` |
| `CollectionTree#getSelectedLibraryID()` | `CollectionTree#getSelectedLibraryIDs()` (new) |
| `CollectionTree#getSelectedCollection()` | `CollectionTree#getSelectedCollections()` |
| `CollectionTree#getSelectedSearch()` | `CollectionTree#getSelectedSearches()` |
| `CollectionTree#getSelectedGroup()` | filter `CollectionTree#getSelectedRows()` by `isGroup()` |

```js
// Zotero 9
let collection = ZoteroPane.getSelectedCollection();

// Zotero 10
let collections = ZoteroPane.getSelectedCollections();
```

The plural getters return arrays and are safe to call with any selection. Note that collections and saved searches can now be selected together, so don't assume that a selected row that isn't a library is a collection.

### Items list view mode

`ItemTree#collectionTreeRow` no longer exists. If your plugin used it to check what kind of view the items list is showing, use the new `viewMode` property, a string derived from the whole selection:

```js
// Zotero 9
if (ZoteroPane.itemsView.collectionTreeRow.isTrash()) { ... }

// Zotero 10
if (ZoteroPane.itemsView.viewMode == 'trash') { ... }
```

Library roots, collections, and saved searches all produce a `viewMode` of `'default'` — they differ in what they contain, not in how the view behaves. Special views produce their row type (e.g., `'trash'`, `'duplicates'`, `'unfiled'`, `'feed'`).

If you need the actual selected rows, use `ZoteroPane.itemsView.collectionTreeRows`, an array in selection order.

### Menu contexts

If your plugin registers menus with `Zotero.MenuManager`, note that the `collectionTreeRow` property of the `main/library/collection` and `main/library/item` contexts throws when read. Use `collectionTreeRows`, which contains the full selection.

### Library headers in the items list

When more than one collection row is selected, the items list contains library header and spacer rows in addition to item rows. If your plugin walks the list via `getRow()` or `_rows`, check `row.isObjectRow` before treating a row as an item. `getSortedItems()` already filters these rows out.

## Search changes

### Search API

- Searches can now express [much more complicated logic](https://forums.zotero.org/discussion/132515/available-for-beta-testing-more-advanced-advanced-search) using condition groups. Wrap conditions in `groupStart`/`groupEnd` conditions with a `joinMode`. Use `resultLevel` to specify what the search returns (`item`, `attachment`, `note`, or `annotation`) or, within a group, the level at which the group's conditions match (e.g., 'items that have a single annotation matching these conditions').
- `Zotero.Search#addCondition()` throws if the legacy `required` parameter is truthy. Use a condition group instead.
- The `fulltextWord` condition was removed. Use `fulltextContent`, which is now backed by a real full-text index and is fast enough for general use.
- The `childNote` condition is deprecated; saved searches containing it are automatically migrated to `note` with a `resultLevel` set to `item`.
- New conditions and operators are available (annotation properties, item/tag counts, `isEmpty`/`isNotEmpty`, and more).

### Advanced Search window

Advanced Search moved into the main window, and `chrome://zotero/content/advancedSearch.xhtml` no longer exists.

### Full-text search

Full-text content search was rewritten on SQLite FTS5. The `fulltextWords` and `fulltextItemWords` tables were dropped from zotero.sqlite; the content and note indexes live in a separate database attached as `ftindex`. Various `Zotero.FullText` methods were removed or replaced.

## Undo/redo

Zotero 10 supports undo/redo for modifications to existing objects — creating or permanently deleting an object isn't undoable (trashing is, since it just sets the object's `deleted` flag). To make a user-initiated change from your plugin undoable, pass an action label when saving:

```js
await item.saveTx({
	undoAction: 'undo-action-edit-metadata',
	undoActionArgs: { count: 1 }
});
```

`undoAction` is a Fluent message ID providing the "Undo/Redo <action>" menu labels. `undoActionArgs` is only needed when the message takes variables — most of Zotero's built-in `undo-action-*` strings vary by `$count`, as above, but a string without variables needs no args. To use a string from your plugin's FTL file, add it to Zotero's string bundle with `Zotero.ftl.addResourceIds(['my-plugin.ftl'])` (and remove it with `removeResourceIds()` on shutdown) — undo labels are formatted via `Zotero.ftl`, not window l10n.

If you save multiple objects in your own transaction, call `Zotero.UndoHistory.stageAction(action, args)` inside the transaction instead: each save inside the transaction records its changes automatically; `stageAction()` adds the label that makes them land on the undo stack as a single step at commit. Saves without an action label aren't added to the undo stack.

## Local HTTP server and local API

If your plugin or an external tool talks to Zotero's local HTTP server (port 23119), several security hardening changes may affect it:

- Requests must send a `Host` header of `localhost`, `127.0.0.1`, or `[::1]`; anything else gets a 400.
- Requests that look like they come from a browser — a `User-Agent` starting with `Mozilla/` or any `Origin` header — are dropped without a response unless they send a `Zotero-Allowed-Request` header or come from the connector. This check previously applied only to CORS-simple content types, so, e.g., a JSON POST that worked before may now be rejected — add the `Zotero-Allowed-Request` header. Custom endpoints can opt out with `allowRequestsFromUnsafeWebContent = true`.

The local API (`/api/`) now supports [write requests](https://www.zotero.org/support/dev/web_api/v3/write_requests). Every response includes a `Zotero-Server-ID` header with a stable ID identifying the Zotero instance. Cache it and pass it back in a `Zotero-Server-ID` request header to confirm you're still talking to the same instance — optional on reads, required on writes. If the ID changes, discard or reconcile any locally cached data: object versions from one instance have no relation to versions from another instance or from the web API.

Version fields in local API responses — object JSON, `format=versions`, `since=` filtering, and `Last-Modified-Version` — now report a new local version rather than the synced version, which didn't reflect local changes and was 0 for unsynced objects.

## Item data validation

Two operations that previously corrupted data now throw:

- `item.setType()` and `item.setField('itemTypeID')` throw when converting a regular item to or from an attachment, note, or annotation.
- The `attachmentFilename`/`attachmentPath` setters throw if a stored-file path contains a slash. Stored-file paths must be a bare filename after the `storage:` prefix. A schema update strips full paths written by older third-party tools.

## Database changes

- **WAL mode is enabled**. If your plugin or external tool reads zotero.sqlite directly (which it probably shouldn't — use the [local API](/support/dev/web_api/v3/local_api) instead!), it must account for the `-wal` and `-shm` files — copying the main database file alone can produce a stale or inconsistent snapshot.
- Several tables gained accent-normalized shadow columns (e.g., `nameNormalized` on `tags`) to support accent-insensitive search.
- New APIs for plugins with their own databases: `Zotero.DB.loadExtension()` loads a bundled SQLite extension (e.g., FTS5); `Zotero.DB.onIdle()` registers a callback for Zotero's idle-maintenance window; and `Zotero.DB.addCorruptionHandler()` lets code with an attached database rebuild it when it's the source of a corruption error.

## Items list refactor

The `ItemTree` class was split into several classes: `ItemTree` handles the virtualized table and columns, `ItemTreeRow` subclasses handle row data and rendering, and `CollectionViewItemTree` renders the items belonging to a collection tree selection.

`ZoteroPane.itemsView` is a `CollectionViewItemTree`, and most methods plugins use — `getSelectedItems()`, `selectItems()`, `getSortedItems()`, `refresh()` — work as before. But some methods moved from `ItemTree` to `CollectionViewItemTree` (e.g., `deleteSelection()`, `changeCollectionTreeRow()`), so if your plugin creates its own tree with `ItemTree.init()`, check that the methods you call still exist on the class you're instantiating.

## Cookie management

`Zotero.CookieSandbox` no longer exists. If your plugin used it to isolate cookies for HTTP requests, use `Zotero.HTTP.newCookieContext()`, which creates an isolated cookie jar backed by a Mozilla `userContextId`:

```js
let cookieContext = Zotero.HTTP.newCookieContext();
await Zotero.HTTP.request('GET', url, { userContextId: cookieContext.id });
let cookies = cookieContext.getCookies('example.com');
// Remove the context's cookies when finished
cookieContext.dispose();
```

`translate.setCookieSandbox()` still exists but now takes the context's numeric ID. `Zotero.Utilities.Internal.getFileFromDocument()` and `saveURI()` no longer accept a `cookieSandbox` parameter, and `Zotero.HTTP.doGet()`'s fourth argument is ignored.

## Localization

Plugin FTL registration was reworked. All plugins' strings are consolidated into a single localization source with proper per-locale fallback: for each Zotero locale your plugin doesn't ship, the closest available locale (same language, then en-US, then first available) is used. Among other things, this fixes plugins that ship only a non-English locale showing that locale for all users, and competing registrations across plugins causing strings to fail to resolve.

## Other changes

- `Zotero.HTTP.download()` was rewritten to stream via `fetch()` and now returns a `Response` object.
- Plugin `prefs.js` files are now loaded with the script cache disabled, so changes to default prefs take effect after a plugin update rather than requiring an app restart.
- `Zotero.MenuManager` now correctly removes a plugin's menu DOM elements when the plugin shuts down.
- `node.ownerGlobal` was removed. Use `node.documentGlobal` instead.
