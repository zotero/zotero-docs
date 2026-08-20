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

## Platform Changes

### Mozilla Platform

The following list includes nearly all Mozilla changes that affected Zotero code. You may encounter other breaking changes if you use APIs not used in Zotero. [Searchfox](https://searchfox.org/) is the best resource for identifying current correct usage in Mozilla code and changes between Firefox 140 and Firefox 153.

Most of these changes fail silently rather than throwing, so a plugin that appears to load fine may still have dead handlers or elements stuck in the wrong state.

-   `hidden`, `collapsed`, `selected`, `disabled`, and `checked` became boolean XUL attributes matched on presence alone ([bug 1979014](https://bugzilla.mozilla.org/show_bug.cgi?id=1979014), [bug 2008041](https://bugzilla.mozilla.org/show_bug.cgi?id=2008041)) — the UA sheet's `[hidden="true"]` is now `[hidden]`, and any value, `"false"` included, means true ([example](https://github.com/zotero/zotero/commit/f245f5c2b26dbf5c313073e4c9a3040b5a2cf807), [example](https://github.com/zotero/zotero/commit/567cc2d43abe04815a40a32f592d5dedab4be64e), [example](https://github.com/zotero/zotero/commit/6fbe5e4135eb836c36ee18daa5fcf5ac8ac5e0c2))
    -   `setAttribute()` stringifies its value, so `setAttribute('hidden', false)` sets `hidden="false"` and hides the element — use `toggleAttribute()` or the boolean `hidden`/`disabled`/`checked` properties
    -   Reflected boolean properties such as `hidden` write an empty value, so `elem.hidden = true` gives `hidden=""` rather than `hidden="true"`. Anything set that way reads back empty: `getAttribute('hidden') == 'true'` → `hasAttribute('hidden')`, and `[hidden="true"]` → `[hidden]` in CSS and `querySelector()` alike
-   `<browser remote="false">` now creates a *remote* browser, since `isRemoteBrowser` is `hasAttribute("remote")`. Omit the attribute for a non-remote browser.
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

Some other Firefox 153 changes that would have affected plugins are handled in Zotero, either by restoring the previous behavior or by providing a replacement, and need no changes in plugins: the removal of `<search-textbox>` and of `wantdropmarker` support on `<toolbarbutton>`, restrictions on loading `chrome:` DTDs, and new restrictions on `eval()` in the parent process.

One of these is temporary. Firefox now applies a baseline `script-src chrome: resource: moz-src:` CSP to every `chrome:` document ([bug 2038660](https://bugzilla.mozilla.org/show_bug.cgi?id=2038660)), which blocks inline `<script>`s and inline event handlers. Zotero currently disables it with `security.chrome_baseline_csp.enabled`, but we intend to move our own inline scripts and event handlers into separate files and drop the pref, so plugins should do the same.
