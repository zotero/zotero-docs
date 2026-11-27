# Zotero 11 for Developers

Zotero 11 includes an internal upgrade of the Mozilla platform on which Zotero is based, incorporating changes from Firefox 140 through Firefox 153.

For previous changes, see [Zotero 10 for Developers](/support/dev/zotero_10_for_developers).

## Feedback

If you have questions about anything on this page or encounter other problems while updating your plugin, let us know on the [dev list](https://groups.google.com/g/zotero-dev). Please don't post to the Zotero Forums about Zotero 11 at this time.

## Dev Builds

<span style="color: red; font-weight: bold">WARNING:</span> These are test builds based on Firefox 153 intended solely for use by Zotero plugin developers and **should not be used in production**. We strongly recommend using a [separate profile and data directory](https://www.zotero.org/support/kb/multiple_profiles) for development.

These `dev` channel builds will stop updating once a beta is available with these changes.

  * [Mac](https://www.zotero.org/download/client/dl?channel=dev&platform=mac)
  * [Linux x86_64](https://www.zotero.org/download/client/dl?channel=dev&platform=linux-x86_64)
  * [Linux ARM64](https://www.zotero.org/download/client/dl?channel=dev&platform=linux-arm64)
  * [Windows 64-bit ZIP](https://www.zotero.org/download/client/dl?channel=dev&platform=win-x64-zip)
  * [Windows 64-bit Installer](https://www.zotero.org/download/client/dl?channel=dev&platform=win-x64)
  * [Windows ARM64 ZIP](https://www.zotero.org/download/client/dl?channel=dev&platform=win-arm64-zip)
  * [Windows ARM64 Installer](https://www.zotero.org/download/client/dl?channel=dev&platform=win-arm64)
  * [Windows 32-bit ZIP](https://www.zotero.org/download/client/dl?channel=dev&platform=win32-zip)
  * [Windows 32-bit Installer](https://www.zotero.org/download/client/dl?channel=dev&platform=win32)

## Updating plugin compatibility

**Do not** update your plugin to declare compatibility with Zotero 11 at this time. We'll make an announcement on the [dev list](https://groups.google.com/g/zotero-dev) when Zotero 11 is feature-frozen and it's time to test for compatibility.

Note that beta, dev, and source builds no longer enforce `strict_max_version`, either at install time or for updates, so you can test a plugin with a lower max version on one of thoes builds without modifying it. Stable builds still enforce it.

## Multiple Windows and Library Tabs

Zotero 11 supports multiple main windows (File → New Window) and multiple library tabs within a window (File → New Library Tab), each with its own collections pane, items list, tag selector, and item pane. Code that assumes a single main window with a single library view may act on the wrong window or tab, or fail when a window has no library view at all.

Where possible, use the plugin APIs (`Zotero.MenuManager`, `Zotero.ItemPaneManager`, `Zotero.ItemTreeManager`, etc.), which add your UI to every window and library view. In their callbacks, use the items and context they pass you rather than `ZoteroPane` or `Zotero.getActiveZoteroPane()`.

### Main windows

-   All library tabs in a window can be closed, so a main window might have no library view. Code that iterates `Zotero.getMainWindows()` should check `win.ZoteroPane?.libraryView`.
-   The `onMainWindowLoad` and `onMainWindowUnload` plugin hooks now follow a window's library views rather than the window itself: `onMainWindowLoad` is called when a window's first library view loads, and `onMainWindowUnload` is called when its last library view is destroyed or the window closes. The same window can go through this cycle more than once, so `onMainWindowUnload` should fully undo what `onMainWindowLoad` did.
-   `Zotero.getActiveZoteroPane()` returns the most recent window's `ZoteroPane` if it has a library view and otherwise the most recently activated one that does. `Zotero.getZoteroPanes()` returns the `ZoteroPane` of every main window.
-   Opening a reader or note in a new window now opens a main window with the reader or note as its only tab. The separate reader window (`reader.xhtml`) and note window (`note.xhtml`) were removed, and reader and note tabs can be moved between windows.
    -   `Zotero.Reader.open()` with `openInWindow` now returns the new window's `ReaderTab` instead of a `ReaderWindow`
    -   The `reader/menubar/*` targets of `Zotero.MenuManager` now apply to the main window's menubar while a reader tab is selected. They're deprecated — use `main/menubar/*` with `enableForTabTypes` instead.

### Library views

-   The library UI moved into a `<library-view>` element. `ZoteroPane.libraryView` is the window's active library view (the selected or most recently selected one), and `ZoteroPane.libraryViews` lists all of them.
-   `ZoteroPane.collectionsView`, `itemsView`, `itemPane`, and `tagSelector`, and `ZoteroPane` methods that use them (e.g., `getSelectedItems()`), resolve to the active library view each time they're accessed. Async code that should stay with the view it started in, even if the user switches tabs in the meantime, should capture the view before awaiting:

        :::js
        let libraryView = ZoteroPane.libraryView;
        await doSomethingSlow();
        libraryView.itemsView.selectItems(ids);

-   For backward compatibility, a window's first library view keeps the original element ids. Other views append their tab's id to each element id (e.g., `zotero-items-tree-tab-k3Xq9TzA`), and every element in a view also gets a class matching its base id.
    -   `document.getElementById('zotero-items-tree')` always returns the first view's element, even when another library tab is selected. Use `libraryView.getElement('zotero-items-tree')` to find an element in a given view.
    -   CSS rules should match the class as well as the id: `#zotero-items-toolbar` → `:is(#zotero-items-toolbar, .zotero-items-toolbar)`
    -   Changes you make directly to the library view's DOM in `onMainWindowLoad` apply only to the window's first library view
-   Background library tabs are unloaded after 24 hours, or when too many are loaded, and recreated when they're selected again, so a view's elements can be destroyed and recreated. A window's last loaded library view is never unloaded.
-   Each window keeps its own item pane section order (`sidenav.order`), pinned item pane section (`pinnedPane`), quick search mode (`search.quicksearch-mode`), and items list columns. The prefs hold the most recently changed values and are used for new windows. To read, change, or observe the value for a specific window, use `Zotero.Prefs.getForWindow(win, name)`, `Zotero.Prefs.setForWindow(win, name, value)`, and `Zotero.Prefs.observeForWindow(win, name, handler)`.

### Tabs

-   A window can have any number of library tabs, and a library tab's id is no longer always `zotero-pane`. Replace `Zotero_Tabs.selectedID == 'zotero-pane'` with `Zotero_Tabs.selectedIsLibrary`, and use `Zotero_Tabs.isLibraryTab(id)` for other tabs.
-   Library tabs that aren't loaded have the type `library-unloaded`, so `Zotero_Tabs.selectedType == 'library'` misses them
-   Any tab can now be pinned, and pinned tabs come first, so the first tab isn't necessarily a library tab and a library tab can be at any position. `Zotero_Tabs.add()` takes a `pinned` option and keeps an `index` you pass after the pinned tabs (or among them for a pinned tab), and `Zotero_Tabs.setPinned(id, pinned)` pins or unpins an existing tab.

## Platform Changes

### Mozilla Platform

The following list includes nearly all Mozilla changes that affected Zotero code. You may encounter other breaking changes if you use APIs not used in Zotero. [Searchfox](https://searchfox.org/) is the best resource for identifying current correct usage in Mozilla code and changes between Firefox 140 and Firefox 153.

Many of these changes fail silently rather than throwing, so a plugin that appears to load fine may still have dead handlers or elements stuck in the wrong state.

You can use `Zotero.platformMajorVersion` in your code to test what Firefox version the current Zotero build is based on.

-   `hidden`, `collapsed`, `selected`, `disabled`, and `checked` became boolean XUL attributes matched on presence alone ([bug 1979014](https://bugzilla.mozilla.org/show_bug.cgi?id=1979014), [bug 2008041](https://bugzilla.mozilla.org/show_bug.cgi?id=2008041)) — the UA sheet's `[hidden="true"]` is now `[hidden]`, and any value, `"false"` included, means true ([example](https://github.com/zotero/zotero/commit/f245f5c2b26dbf5c313073e4c9a3040b5a2cf807), [example](https://github.com/zotero/zotero/commit/567cc2d43abe04815a40a32f592d5dedab4be64e), [example](https://github.com/zotero/zotero/commit/6fbe5e4135eb836c36ee18daa5fcf5ac8ac5e0c2), [example](https://github.com/zotero/zotero/commit/c522eaf009036a69b86da00513952986f63fe25d))
    -   `setAttribute()` stringifies its value, so `setAttribute('hidden', false)` sets `hidden="false"` and hides the element — use `toggleAttribute()` or the boolean `hidden`/`disabled`/`checked` properties
    -   `toggleAttribute(name, force)` toggles the attribute when `force` is `undefined`, so pass an actual boolean (e.g., `!!item.disabled`) ([example](https://github.com/zotero/zotero/commit/3e32faa0ac53aa83092f216389ac244d901a656a))
    -   Reflected boolean properties such as `hidden` write an empty value, so `elem.hidden = true` gives `hidden=""` rather than `hidden="true"`. Anything set that way reads back empty: `getAttribute('hidden') == 'true'` → `hasAttribute('hidden')`, and `[hidden="true"]` → `[hidden]` in CSS and `querySelector()` alike
-   `<browser remote="false">` now creates a *remote* browser, since `isRemoteBrowser` is `hasAttribute("remote")`. Omit the attribute for a non-remote browser ([example](https://github.com/zotero/zotero/commit/d649bc4804044270acd15e59c2319bfac2774cc2)).
-   `ownerGlobal` → `documentGlobal` ([bug 2033243](https://bugzilla.mozilla.org/show_bug.cgi?id=2033243)) — the attribute was renamed and moved from `EventTarget` to `Node`, so `element.ownerGlobal` is now `undefined` rather than throwing ([example](https://github.com/zotero/zotero/commit/5ea624338440a7a5f1994b4c2f96a1fb8fb97248))
-   `XPCOMUtils.defineLazyServiceGetter()` and `defineLazyServiceGetters()` require an `nsIID` rather than an interface name: `"nsIIOService"` → `Ci.nsIIOService` ([example](https://github.com/zotero/zotero/commit/09249bfb7307b181b6eb0eaa12c0a90f53f289dc))
-   The XUL checkbox `CheckboxStateChange` event was removed ([bug 2009806](https://bugzilla.mozilla.org/show_bug.cgi?id=2009806)) — listen for `command` instead ([example](https://github.com/zotero/zotero/commit/bcd8ad20ec1a484d715a2e447eed6b0ca779d276)). `command` fires only on user interaction, so assigning to `.checked` in code no longer notifies your handler.
-   The icon element inside `<menuitem>` and `<menu>` changed from a XUL `<image>` to an `<html:img>`. A XUL `<image>` painted `list-style-image`; the `<html:img>` renders the `image` attribute or `content: var(--menuitem-icon)` instead.
    -   Set `--menuitem-icon` on the `<menuitem>` or `<menu>`. Keep `list-style-image` too for native macOS menus, which still read it ([example](https://github.com/zotero/zotero/commit/300305a9fb89b237d2f7bd6057a70fb4b32bedb7))
    -   Plugins that add menus through `Zotero.MenuManager` don't need to change anything
-   `Cu.Sandbox` freezes built-ins by default for system-principal sandboxes ([bug 2017957](https://bugzilla.mozilla.org/show_bug.cgi?id=2017957)). Since a plugin's bootstrap scope is such a sandbox, built-ins are frozen there: assignments to `Promise`, `Array.prototype`, `JSON.parse`, and the like now fail silently, or throw in strict mode. Your plugin's own globals and objects are unaffected. If your plugin creates its own sandbox and needs to modify built-ins in it, pass `freezeBuiltins: false` ([example](https://github.com/zotero/zotero/commit/a32dd2b95928183491c29da0c755a06820e75271)).
- Login-manager access to stored login records is now asynchronous. findLogins() now throws and searchLogins() was removed; use await searchLoginsAsync({...}) instead. Other synchronous methods, such as addLogin(), modifyLogin(), and removeLogin(), were all also removed; use their Async-suffixed equivalents ([example](https://github.com/zotero/zotero/commit/3bb3b4f2c9ff8da62b8681360e6921bf9eb9f057)). Code that cannot await must use cached state rather than reading login records synchronously.
-   `<wizard>` no longer has `extra1` and `extra2` buttons — create them yourself if you need them ([example](https://github.com/zotero/zotero/commit/f53d4b5eca8ecb24be622212f0880a69da245ead))
-   `oncommand` was added to `GlobalEventHandlers` for the Invoker Commands API ([bug 1974578](https://bugzilla.mozilla.org/show_bug.cgi?id=1974578)), so `elem.oncommand = "someFunction()"` — previously an inert expando write — now sets the WebIDL event handler, and a string becomes `null`, wiping any handler compiled from the attribute ([example](https://github.com/zotero/zotero/commit/b5ceb6fa650c91c7059fe69ecd32117044182725))
-   `<input type="search">` now gets a native clear button in chrome documents regardless of `layout.forms.input-type-search.enabled` ([bug 1655503](https://bugzilla.mozilla.org/show_bug.cgi?id=1655503)), which will double up with one you draw yourself ([example](https://github.com/zotero/zotero/commit/503bb03aec5cf663c327cc3e049834bced6da243))
-   `dataTransfer.types` no longer includes `application/x-moz-file` for file drops ([bug 1993095](https://bugzilla.mozilla.org/show_bug.cgi?id=1993095)) — check for `Files` instead ([example](https://github.com/zotero/zotero/commit/2530b2f106ca288bdce49d2191b996ad592c163c))
-   The `--panel-background` CSS variable was renamed to `--panel-background-color` ([example](https://github.com/zotero/zotero/commit/f2cda652c9b44a5fd6fa069f660252a73a0c10cd)), and `--arrowpanel-background` was removed ([example](https://github.com/zotero/zotero/commit/1ae616718f437b583256804d875bc8d2a077b81d))

Some other Firefox 153 changes that would have affected plugins are handled in Zotero, either by restoring the previous behavior or by providing a replacement, and need no changes in plugins: the removal of `<search-textbox>` and of `wantdropmarker` support on `<toolbarbutton>`, restrictions on loading `chrome:` DTDs, and new restrictions on `eval()` in the parent process.

One of these is temporary. Firefox now applies a baseline `script-src chrome: resource: moz-src:` CSP to every `chrome:` document ([bug 2038660](https://bugzilla.mozilla.org/show_bug.cgi?id=2038660)), which blocks inline `<script>`s and inline event handlers. Zotero currently disables it with `security.chrome_baseline_csp.enabled`, but we intend to move our own inline scripts and event handlers into separate files and drop the pref, so plugins should do the same.

Similarly, `Services.scriptloader` now refuses to load scripts from `jar:file:` and `file:` URIs unless `allowUnsafeURL: true` is passed ([bug 1974213](https://bugzilla.mozilla.org/show_bug.cgi?id=1974213)). Zotero passes it when loading a plugin's `bootstrap.js` and `prefs.js` and preference pane scripts, and it currently enables `security.allow_unsafe_subscript_loads` so that scripts a plugin loads itself from its XPI still work, but that pref will be disabled in the future. Load your own scripts with `Services.scriptloader.loadSubScriptWithOptions(url, { target, allowUnsafeURL: true })` instead of `loadSubScript()`.

## Other Changes

-   Command-line flags now use kebab case without a "Zotero" prefix: `-ZoteroDebug` → `-debug`, `-ZoteroDebugText` → `-debug-text`, `-ZoteroSkipBundledFiles` → `-skip-bundled-files`. The old names still work but log a deprecation warning.
-   Data loading for items and other data objects was reworked to reduce memory use and speed up startup. Plugins shouldn't rely on data-object internals (e.g., underscore-prefixed properties and methods), but if yours does, check that it still works.
