---
tags:
  - kb
  - entry
---

# Troubleshooting Problems Saving to Zotero

When you're unable to save high-quality metadata from a particular website, most likely either the page isn't supported by an existing [Zotero translator](translators) or the page layout recently changed, breaking Zotero's ability to recognize data. On some sites, most notably Google Scholar, you may also be running into [site access limits](kb/site_access_limits).

If the problem occurs across multiple sites, there may be a problem with your installation, and you should try these steps:

1.  If you don't see a "Save to Zotero" button in your browser toolbar at all, **make sure you've installed the Zotero Connector** in your browser. The Zotero Connector should be listed in the browser's Extensions pane. You can install the appropriate Zotero Connector from the [Zotero download page](/download/). If the extension is installed but not visible, you may need to [pin it to your toolbar](kb/no_toolbar_button).
2.  If you [only see a gray webpage icon](kb/page_not_recognized), **make sure you're looking at a [supported site](translators)**. If you're not sure, try a Wikipedia article or an Amazon book page.
3.  If you're looking at a supported site, **try reloading the page** and **make sure the page has fully loaded**. Pressing your browser's stop button or pressing Esc on your keyboard can help if something on the page has stalled.
4.  If the gray webpage icon appears on all sites, **try restarting your browser**.
5.  If you don't have the Zotero program open, **try opening Zotero before saving**. (If you haven't installed Zotero, you can do so from the [download page](/download/).) While the Connector can save directly to your online library, you'll usually get better results saving to Zotero directly. If the Zotero Connector reports that Zotero is unavailable and tries to save to zotero.org, see [Zotero Unavailable](kb/connector_zotero_unavailable).
6.  **Check for Zotero Connector updates** from the Extensions pane of your browser, in case a new version of the Connector is available that you haven't yet received.
7.  Make sure you have an **up-to-date version of your browser**. See [System Requirements](system_requirements) for supported browser versions.
8.  If you're saving with Zotero open, **[check your Zotero version](kb/zotero_version)** to make sure you have the latest version. We can only troubleshoot translators that fail for the current version of Zotero available from the [download page](https://www.zotero.org/download).
9.  Check **[Known Translator Issues](known_translator_issues)** to see if problems have been noted on the site you're trying to save from.
10. **Hover your cursor over the "Save to Zotero" button.** The tooltip shows which translator, if any, Zotero will use for the page you are viewing. If the displayed translator is incorrect, post the site name, a URL, and the name of the incorrectly detected translator to the Zotero Forums. Note that "Web Page", "Embedded Metadata," "DOI," and "COinS" are [generic translators](kb/default_translators), and Zotero may not be able to save full metadata or PDFs from all sites on which they appear. A problem with a primary translator may cause a generic translator to be used instead.
11. Make sure you've given the Zotero Connector **permission to access all websites**. In Chrome or Edge, right-click on the Save to Zotero button, select "This Can Read and Change Site Data", and make sure it's set to "On All Sites". In Safari, go to the Websites tab of the Safari settings, select Zotero Connector under Extensions, and make sure "For other websites" is set to "Allow". The [Zotero privacy policy](privacy#zotero_connector) explains why this is necessary.
12. If you are having chronic problems getting the Zotero Connector to work across multiple sites, you may have an extension conflict. Try **uninstalling and reinstalling the Zotero Connector**. If that doesn't help, try **disabling all extensions** except the Zotero Connector. If this solves the problem, re-enable the extensions one-by-one until you find the conflict, and then post the name of the extension that was causing the issue to the forums.
13. In rare cases where you're seeing a webpage icon on all sites or saving is failing on all sites, there may be a problem with your translators. Disable all third-party plugins, restart Zotero, and select "**Reset Translators**" from the Advanced pane of the Zotero preferences (if applicable), and then do the same for the Zotero Connector (right-click on the save button → Options/Preferences → Advanced tab). This isn't necessary as a regular troubleshooting step.
14. **If none of these solutions solve your problem**, we'll need additional information to further debug your issue. Please create a new thread in the [Zotero Forums](http://forums.zotero.org/) — or use your existing thread if you've already created one — and provide the following:
    -   The **exact URL of a page that isn't working** (even if none are)
    -   A **[Debug ID](debug_output#zotero_connectors_firefox_chrome_and_safari)** from the Zotero Connector for reloading the page and attempt to save or, if you're only getting a webpage icon, for reloading the page.
    -   What it says in the popup when you try to save (e.g., "Saving to My Library" or "Saving to zotero.org")
    -   The **name of the translator** from step 10, if applicable


