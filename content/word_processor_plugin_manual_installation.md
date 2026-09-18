# Manually Installing the Zotero Word Processor Plugin

The Zotero Word plugins will be installed automatically into Word for most users. If you don't see a Zotero toolbar in Word, you should [attempt to reinstall](word_processor_plugin_troubleshooting#zotero_toolbar_doesn_t_appear) the plugin from the Cite → Word Processors pane of the Zotero preferences. If you receive an error or still don't see the plugin after trying to reinstall from the preferences, you can try the manual installation instructions below.

Note that, if you rely on manual installation, you may run into problems later due to the plugin in Word becoming outdated, so it's better to figure out why automatic installation isn't working (e.g., security software blocking the installation or an [incorrect Word Startup folder location](#locating_your_word_startup_folder)) and fix the underlying problem.

## Word for Windows

1.   Open the Zotero installation folder (`C:\Program Files\Zotero`).
2.   In the installation folder, open `integration\word-for-windows`, where you can find a copy of the Zotero.dotm file.
    -   If the folder is empty, the file was somehow deleted — possibly by security software — and you should reinstall Zotero.
    -   If the folder is empty immediately after reinstalling Zotero, you can download [Zotero.dotm](https://github.com/zotero/zotero-word-for-windows-integration/raw/main/install/Zotero.dotm), but your security software may delete the downloaded file as well, and you'll need to configure it not to do so.
    -   If you see two "Zotero" files without file extensions, your computer is set not to display file extensions, and you can determine which one is Zotero.dotm by right-clicking on each file and selecting Properties. One will say "Microsoft Word 97-2003 Template (.dot)" and one will say "Microsoft Word Template (.dotm)".
3.  Find your Word startup folder and copy the path to the clipboard:
    1.  In the Word ribbon, click the File tab, click Options, and then click Advanced.
    2.  Under General, click File Locations. The current Startup folder should be listed.
        -   In most cases, the Startup folder path should be the default location of `%AppData%\Microsoft\Word\STARTUP`. The path should not include "Zotero" in any way, and if it does you previously configured it incorrectly. If that's the case, you should reset the path to the default location.
    3.  Select the Startup folder path and click Modify, click in the whitespace to the right of the path in the location bar at the top of the window, copy the complete path to the clipboard with Ctrl-C, and then **click Cancel** to close the dialog without making changes.
4.  Open a new File Explorer window and paste the Startup folder path into the address bar. You should now have two folders open: the "install" folder containing Zotero.dotm and the Word startup folder.
5.  Close Word.
6.  Copy the Zotero.dotm file from "install" to your Word Startup folder. (Be sure to copy the file rather than moving it. If dragging, hold down Ctrl.)
7.   Restart Word to begin using the plugin.

## Word for Mac

1.   In Finder, press Cmd-Shift-G and navigate to `/Applications/Zotero.app/Contents/Resources/integration/word-for-mac`, where you can find a copy of the Zotero.dotm file. If the folder is empty, the file was somehow deleted — possibly by security software — and you should reinstall Zotero.
2.   Find your Word startup folder by following the [instructions below](#word_2016_and_2019_for_mac). You should now have two folders open: the Word startup folder and the "install" folder containing Zotero.dotm.
3.  Copy the Zotero.dotm file to your Word Startup folder. (Be sure to copy the file rather than moving it.)
4.   Start (or restart) Microsoft Word to begin using the plugin.

## LibreOffice

1.  Navigate to the Zotero application files:
    -   Mac: In Finder, press Cmd-Shift-G and paste in or `/Applications/Zotero.app/Contents/Resources/integration/libreoffice`
    -   Windows: Open the folder `C:\Program Files\Zotero\integration\libreoffice`
    -   Linux: Go to the directory where Zotero is installed and open `integration/libreoffice`
2.  Double-click the Zotero_OpenOffice_Integration.oxt file to install it. Alternatively, go to Tools → Extension Manager in LibreOffice, click Add, and select the .oxt from the above folder.

If you get an error, there's a problem with your LibreOffice installation, and you should follow the [troubleshooting steps](kb/word_processor_plugin_installation_error#libreoffice).

## Locating your Word Startup folder

Note: On non-English systems or in certain custom setups, these locations may be different.

#### Word 2007 or later for Windows

The default location of the Startup folder is `%AppData%\Microsoft\Word\STARTUP`.

If changes you make to the Startup folder aren't taking effect, you can confirm that Word isn't set to a different location. In the Word ribbon, click the File tab, click Options, and click Advanced. Under General, click File Locations. The Startup folder should be listed there. Select it and click Modify. In the window that opens, click the whitespace to the right of the path in the location bar at the top and copy the complete path to the clipboard by pressing Ctrl-C. **Click Cancel** to close the dialog without making changes. You can then open a new File Explorer dialog and paste the path into the address bar to open the Startup folder.

Note that the path should not include "Zotero" in any way, and if it does you previously configured it incorrectly. If that's the case, you should reset the path to the default location.

#### Word 2016 and 2019 for Mac

The default location of the Startup folder is `~/Library/Group Containers/UBF8T346G9.Office/User Content/Startup/Word`. (`~/Library` refers to the Library folder within your home directory.) You can open it from the Finder by pressing Cmd-Shift-G and copying in the path. Alternatively, to navigate to it in Finder, hold down Option on your keyboard, click the Go menu, and select Library (which is hidden by default), and then follow the rest of the path.

If changes you make to the Startup folder aren't taking effect, you can confirm that Word isn't set to a different location. In Word, open the "Word" menu in the top-left of the screen and select "Preferences". Click on "File Locations" under "Personal Settings" and click on "Startup" at the bottom of the list.

Generally, no location should be listed, causing Word to use the default location. If another location is listed (e.g., `/Applications/Microsoft Office 2011/Office/Startup/Word`, from an earlier version of Word), clearing the setting and letting Word use the default location may fix installation problems and allow Zotero to install the plugin automatically going forward.

Note that the path should not include "Zotero" in any way, and if it does you previously configured it incorrectly. If that's the case, you should reset the path so that it is blank and the default location is used.
