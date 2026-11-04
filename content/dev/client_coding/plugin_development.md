# Zotero Plugin Development

*This page is a work in progress.*

## Introduction to Zotero Plugins

Zotero plugins run within the Zotero desktop app and interact with Zotero's internal [JavaScript API](dev/client_coding/javascript_api) and internal Firefox APIs.

If you plan to write a plugin, you can start by taking a look at the [official sample plugin](https://github.com/zotero/make-it-red) as well as [existing third-party plugins](plugins).

## Alternatives to Zotero Plugins

Depending on your use case, it may be easier to create an external tool that uses the [Web API](dev/web_api) to access Zotero libraries, read from (not write to!) [the Zotero client's SQLite database](dev/client_coding/direct_sqlite_database_access), [run ad hoc JavaScript](dev/client_coding/javascript_api#running_ad_hoc_javascript_in_zotero) within Zotero, or interact with [one of the other APIs](dev/client_coding) that the Zotero client exposes (e.g., for word processor integration).

The [Zotero Plugins page](plugins) (which takes an expansive view of the term "plugin") can be very helpful in helping you develop your own Zotero-based tools.

## Setting Up a Plugin Development Environment

When developing a Zotero client plugin, it's helpful to have Zotero run the plugin directly from source. After creating your plugin's source directory with sample code, you can tell Zotero to load the plugin by creating an extension proxy file. (This is a technique that used to be possible for Firefox extension development, though it's since been discontinued in Firefox.)

1.  Close Zotero.
2.  Create a text file in the 'extensions' directory of your [Zotero profile directory](kb/profile_directory) named after the extension id (e.g., myplugin@mydomain.org). The file contents should be the absolute path to the root of your plugin source code directory, where your install.rdf or bootstrap.js file is located.
3.  Open prefs.js in the Zotero profile directory in a text editor and delete the lines containing `extensions.lastAppBuildId` and `extensions.lastAppVersion`. Save the file and restart Zotero. This will force Zotero to read the 'extensions' directory and install your plugin from source, after which you should see it listed in Tools → Add-ons. This is only necessary once.
4.  Whenever you make changes to your plugin code, start up Zotero from the command line and pass the `-purgecaches` flag to force Zotero to re-read any cached files. (This may no longer be necessary with Zotero 7.) You'll likely want to make an alias or shell script that also includes the `-ZoteroDebugText` (`-debug-text` in Zotero 11+) and `-jsconsole` flags and perhaps `-p <Profile>`, where `<Profile>` is the name of a development profile.
