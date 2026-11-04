# Debug Output Logging

If you've been asked to provide a Debug ID (which is different from a [Report ID](reporting_problems)) to help troubleshoot a problem, follow these simple steps:

### Zotero

1.  In the Help menu, go to Debug Output Logging and select Enable, or, to generate debug output from Zotero startup, select "Restart with Logging Enabled…". (If you're not able to access the Help menu, see [Reporting Startup Errors](reporting_problems#reporting_startup_errors) instead.)
2.  Immediately perform the relevant action (syncing, saving, importing, etc.) and reproduce the problem you're experiencing.
3.  Before doing anything else, return to Help -> Debug Output Logging and click Submit Output, which will disable logging and submit the output to zotero.org. A window should pop up containing a Debug ID (e.g., “D12345678”). Click "Copy to Clipboard" and paste the Debug ID into your forum thread.

If submitting output fails, you can return to the Debug Output Logging menu and select View Output, go to File -> "Save…", choose Format: "Text Files", and save the output to a file, which you can email to support@zotero.org with a link to your forum thread. It can be helpful to ZIP the file before emailing it.

### Zotero Connectors (Firefox, Chrome, and Safari)

1.  Open the Zotero Connector preferences
    -   **Firefox:** right-click on the Zotero Connector extension button and click "Preferences".
    -   **Chrome:** right-click on the Zotero Connector extension button and click "Options".
    -   **Safari:** right-click anywhere on a webpage and select "Zotero Preferences..."
2.  In the Advanced tab of the Zotero Connector preferences, under "Debug Output Logging", check the box next to "Enable Logging". Do not close this tab.
3.  Immediately perform all the relevant actions (e.g., import an item from a web page).
4.  Go back to the Advanced tab of the Zotero Connector preferences and click Submit Output.
5.  You will be provided with a Debug ID (e.g., "D12345678"). Please post the Debug ID to the forums.
6.  Uncheck the box next to "Enable Logging."

### Zotero for iOS

1.  Tap Back in the top-left corner until you see the list of libraries.
2.  Tap the Gear icon.
3.  Tap "Debug Output Logging" and then "Start Logging".
4.  Close the settings window.
5.  Immediately perform all relevant actions (e.g., pulling down on the items list to trigger a sync or switching to the browser and trying to save).
6.  When you're done, tap the circular stop button in the bottom-left corner of the screen.
7.  In the alert that pops up, tap Copy to copy the Debug ID to the clipboard, and then paste it into your forum thread.

### Zotero for Android

1.  From the items list, tap Collections in the top-left corner and then Libraries.
2.  Tap the Gear icon.
3.  Tap "Debug Output Logging" and then "Start Logging".
4.  Close the settings window.
5.  Immediately perform all relevant actions (e.g., pulling down on the items list to trigger a sync or switching to the browser and trying to save).
6.  When you're done, tap the circular stop button in the bottom-left corner of the screen.
7.  In the alert that pops up, tap Copy to copy the Debug ID to the clipboard, and then paste it into your forum thread.

## Logging to a Terminal Window

If you'd like to regularly follow Zotero's debug output in real-time, it may be preferable to have Zotero log to a terminal window using the `-ZoteroDebugText` flag (`-debug-text` in Zotero 11+).

### macOS

-   Open Terminal via Spotlight or from /Applications/Utilities.
-   Go to the Terminal menu and open Settings. In Profiles → Window, make sure Scrollback is set to “Limit to available memory”.
-   Paste `/Applications/Zotero.app/Contents/MacOS/zotero -ZoteroDebugText` into the Terminal window.
-   Press Return

You can add ` > ~/Desktop/zotero-debug.txt` to the end of the command to redirect the output to a file on your desktop.

### Windows

-   Open cmd.exe, Cygwin shell, or another terminal
-   Paste `"C:\Program Files\Zotero\zotero.exe" -ZoteroDebugText` into the console window. It may be necessary to add ` -console` as well.
-   Press Enter.

Due to limitations of the available console windows on Windows, you may have a better experience using `-ZoteroDebug` (`-debug` in Zotero 11+) instead to use Zotero's internal debug output window.

### Linux

Start Zotero via the command line, adding the `-ZoteroDebugText` command-line flag.

### Logging to a File

To capture output when Zotero is crashing or hanging, you can use `-ZoteroDebugText > zotero-debug.txt` to redirect output to a file.

#### Developer Note

To enable Zotero debug output permanently, set extensions.zotero.debug.log to true in the Zotero config editor, accessible from the Advanced pane of the Zotero preferences, and then start Zotero from the command line.
