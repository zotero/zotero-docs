---
tags:
  - kb
  - basics
---

# Zotero Connector and Safari

## Installation

The Zotero Connector for Safari is bundled with the Zotero desktop app. (Unlike other browsers, Safari does not allow direct installation of browser extensions.) After opening Zotero for the first time, you can enable the Zotero Connector from the Extensions pane of the Safari settings ("Safari" menu → "Settings" → "Extensions", **not** "Safari" menu → "Safari Extensions…").

The Zotero Connector for Safari requires macOS 11 Big Sur or later, with macOS 13 Ventura or later required for full functionality.

Using an iPhone or iPad? You can save to the [Zotero iOS app](https://apps.apple.com/us/app/zotero/id1513554812) using the Share sheet in Safari and other browsers.

![safari-compatibility.png](/_media/kb/safari-compatibility.png){ width=700 }

<p id="can_t_see_the_save_button_save_button_flickering"></p>

## Extension not showing up? Save button missing or flickering?

A macOS bug can cause the Zotero Connector to disappear from Safari or stop working after the Zotero app is updated.

If you find that the extension isn't listed in the Safari settings or the toolbar button isn't appearing or working properly, you can likely fix it using one of the options below. Your data will not be affected.

Currently, the only known way to avoid this problem altogether is to switch to a browser with a more reliable extension framework such as Firefox, Chrome, or Edge.

### Fix 1: Delete and Reinstall Zotero

1.  Delete the Zotero app from Applications
2.  Redownload it from the [download page](/download)
3.  Start Zotero

It may also help to restart your computer in between deleting the app and restoring it, though this isn't usually necessary.

### Fix 2: Compress and Uncompress Zotero

This option avoids redownloading the app.

1.  In Finder, go to the Applications folder and compress the Zotero app (right-click → Compress “Zotero”)
2.  Delete the app
3.  Double-click the ZIP file
4.  Delete Zotero.zip
5.  Start Zotero

### Fix 3: Force Extension Reloading (advanced)

1.  Go to Safari → Settings → Advanced
2.  Enable "Show features for web developers" at the bottom
3.  Click the new Developer tab that appears and toggle "Allow Unsigned Extensions" on and off. The Zotero Connector is signed, so it is not necessary to leave "Allow Unsigned Extensions" enabled.

## Limitations

Due to technical limitations of the Safari extension framework, some features available in Firefox, Chrome, and Edge aren't available in Safari:

-   Automatic RIS/BibTeX import
-   Automatic CSL installation from sites other than the [Zotero Styles Page](https://www.zotero.org/styles), GitHub, and Gitee.

Other differences:

-   It's not possible to right-click on the toolbar button to access secondary translators. Instead, right-click on the page itself.


