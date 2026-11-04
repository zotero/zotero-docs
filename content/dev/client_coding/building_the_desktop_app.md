# Building the Zotero Desktop App

**Note:** If you don't plan to make any code changes yourself, and only wish to run the latest prerelease versions of Zotero, you can just install a [beta build](beta_builds).

## Build Steps

***Windows users:** The following commands assume a POSIX-compliant system. To build Zotero on Windows, please follow the [Windows-specific steps](dev/client_coding/building_the_desktop_app_windows_notes) first.*

1.  Make sure you have Git and Git LFS installed. `git lfs` shouldn't show an error.
2.  Clone the Zotero source code: `git clone --recursive https://github.com/zotero/zotero zotero-client`
3.  Change to the source code repo: `cd zotero-client`
4.  Check your system for the necessary tools and install any that are missing: `app/scripts/check_requirements`This script checks all requirements for the official Zotero distribution system. If you're only creating a custom build, you only need the build requirements listed at the top, not the distribution requirements.
5.  Install Node.js modules using Node 18 or later: `npm i`
6.  Build and run Zotero: `app/scripts/build_and_run -r`

The build will be placed in the `app/staging/` folder in unpackaged form.

To build Zotero for another platform, first prepare Zotero's JavaScript source by running `npm run build` (or keep it prepared automatically as you make changes with `npm start`) and then run `app/scripts/dir_build` with the `-p` flag for the desired platform (e.g., `app/scripts/dir_build -p w` for Windows).

If running `npm run build` manually, you may need to set the environment variable `NODE_OPTIONS=--openssl-legacy-provider` to avoid an `ERR_OSSL_EVP_UNSUPPORTED` error.

## Running Your Custom Build

After you've built the client, you can continue to run it from `app/staging/` or move the app directory to a location of your choosing and start it normally.

For development, you'll generally want to leave the app in the staging directory and use the `build_and_run` [helper script](#helper_script), but you can also call the launcher directly:

#### Mac

`app/staging/Zotero.app/Contents/MacOS/zotero`

#### Linux

`app/staging/Zotero_linux-x86_64/zotero`

#### Windows

`app/staging/Zotero_win-x64/zotero.exe`

### Command-line Flags

In most cases, you'll want to use the [helper script](#helper_script) below instead of passing these flags directly to the launcher, but it's helpful to know the available options, which also work with release and beta builds:

-   `-ZoteroDebugText` or `-ZoteroDebug` (`-debug-text` or `-debug` in Zotero 11+) enable [real-time debug output](debug_output#logging_to_a_terminal_window) from `Zotero.debug()`
-   `-jsconsole` will open the Error Console (Tools → Developer → Error Console)
-   `-jsdebugger` starts the Firefox Browser Toolbox for [debugging](dev/client_coding/developer_tools), if Zotero was built with devtools enabled; not available in release builds
-   `-ZoteroSkipBundledFiles` (`-skip-bundled-files` in Zotero 11+) skips style and translator initialization, which speeds up startup time after rebuilding if you're working on something that doesn't require styles or translators
-   `-skip-feed-updates` (Zotero 11+) disables automatic feed updates

### Helper Script

For local development, you'll want to use the [build_and_run](https://raw.githubusercontent.com/zotero/zotero/master/app/scripts/build_and_run) helper script, which automates rebuilding Zotero and starting it with debug output enabled and the error console open. Create an alias for `app/scripts/build_and_run` with an appropriate name (e.g., `zotero-source`), or create a shell script in your path that selects a preexisting development [profile](kb/multiple_profiles) and forwards command-line parameters:

``` sh
#!/bin/bash -e
export ZOTERO_PROFILE="Dev"
~/zotero-client/app/scripts/build_and_run "$@"
```

You can then run, e.g., `zotero-source -r` to rebuild Zotero with your changes and start it up.

Options:

-   `-r` Rebuild (calling \`npm run build\` automatically if \`npm start\` isn't running)
-   `-d` Include Firefox Developer Tools in a rebuild and open the Browser Toolbox
-   `-b` Skip bundled styles and translators — avoids extra work at startup if not needed

## Running Zotero Plugins

See [Setting Up a Plugin Development Environment](dev/client_coding/plugin_development#setting_up_a_plugin_development_environment).

## Packaging

### Mac

-   In Keychain Access, create `build` keychain with password
-   Import Developer ID certificate, public key, and private key
-   Set up app-specific password for Apple developer account
-   Create a `config-custom.sh` file in `app/`: `SIGN=1
    KEYCHAIN=build
    KEYCHAIN_PASSWORD=<keychain password>
    NOTARIZATION_BUNDLE_ID=<bundle id (e.g., org.foo.App) — only used for notification emails>
    NOTARIZATION_USER="<username of Apple developer account>"
    NOTARIZATION_PASSWORD="<app-specific password for Apple developer account>"
    BUILD_PLATFORMS=m
    `

### Windows

-   Verify that git is set to check out files without modifying line endings. Run `git config --global core.autocrlf`; the response should be either empty (meaning `false`) or `input`. If set to `true` (as it may be if you selected the default option while installing the Windows version of git), change this with `git config --global core.autocrlf false`. See the [git manual](http://git-scm.com/docs/git-config) for details on this setting.
-   [Install NSIS 3](https://nsis.sourceforge.io/Download) and install the necessary plugins by running `app/win/download-nsis-plugins`.
-   The [Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-sdk/) is needed for the included `signtool.exe` utility. You can uncheck all other installation options. Adjust the path in `app/config.sh` to point to the correct version of the Windows SDK.

## Installation and Packaging Notes

**Note:** These instructions are not applicable on macOS, which handles protocol handler and MIME type registration through the bundled Info.plist file. Additionally, they are only applicable on Windows if for some reason you want to run Zotero without running the installer.

#### Protocol handler registration

Zotero should be registered as the system's default protocol handler for the `zotero://` protocol.

#### File extensions and MIME types

Zotero should be registered as a handler for the following file extensions and MIME types:

<table>
<thead>
<tr class="header">
<th>File type description</th>
<th>File extension(s)</th>
<th>MIME type(s)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Research Information Systems Document</td>
<td>ris</td>
<td>application/x-research-info-systems<br />
application/x-endnote-refer<br />
text/x-research-info-systems<br />
text/application/x-research-info-systems<br />
text/ris<br />
ris</td>
</tr>
<tr class="even">
<td>ISI Common Export Format Document</td>
<td>ciw, isi</td>
<td>application/x-inst-for-Scientific-info</td>
</tr>
<tr class="odd">
<td>Metadata Object Description Schema Document</td>
<td>mods</td>
<td>application/mods+xml</td>
</tr>
<tr class="even">
<td>Resource Description Framework Document</td>
<td>rdf</td>
<td>application/rdf+xml</td>
</tr>
<tr class="odd">
<td>BibTeX Document</td>
<td>bib, bibtex</td>
<td>application/x-bibtex<br />
text/x-bibtex</td>
</tr>
<tr class="even">
<td>MARC Record</td>
<td>mrc, marc</td>
<td>application/marc</td>
</tr>
<tr class="odd">
<td>CSL Citation Style</td>
<td>csl</td>
<td>vnd.citationstyles.style+xml</td>
</tr>
</tbody>
</table>

## Distribution

Distribution steps are beyond the scope of this page, but see `app/scripts/build_and_deploy` for an example of the necessary steps.
