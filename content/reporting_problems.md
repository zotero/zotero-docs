# Reporting Zotero Problems

Is something in Zotero not working correctly for you? Here's the information you'll need to provide in the [Zotero Forums](/forum) to allow Zotero developers and others to help you most effectively.

**Please provide both #1 and #2 listed below.**

### 1. Provide a Report ID

#### Zotero

If you're experiencing a problem, before restarting Zotero, open the Help menu in Zotero and select "Report Errors…".

In the window that pops up, submit the error report, and then copy the numeric Report ID (not the contents of the report) and paste it into your Zotero Forums thread. Error reports aren't reviewed unless referred to in the forums, since the reports generally aren't helpful without context and/or follow-up.

If you're not able to provide a Report ID, be sure to include your operating system and [Zotero version](kb/zotero_version) in your forum post.

**Alternative:** If you're reporting a reproducible problem with a particular operation, a [Debug ID](debug_output), which logs activity over a specific period of time, may be more useful than a Report ID. It's always fine to provide a Debug ID instead of a Report ID if you think it might be helpful, but you can also wait for a Zotero developer to request one.

#### Zotero Connector

If you're experiencing a problem with the Zotero Connector, before restarting the browser, open the Zotero Connector preferences:

-   **Chrome:** Right-click on the "Save to Zotero" button and click Preferences/Options, or type chrome://extensions into the address bar, press Enter, click "Details" under Zotero Connector, and then click "Extension options".
-   **Safari:** Right-click anywhere on a webpage and select "Zotero Preferences..."
-   **Firefox:** Type <about:addons> into the address bar, press return, and click the “Preferences” button next to the entry for “Zotero Connector”. Click on the "Advanced" Tab.

In the Report Errors section, click "Submit Report". A dialog box should pop up containing a Report ID. Post the Report ID to the Zotero Forums.

If you are unable to provide a Report ID, be sure to include your [Zotero version](kb/zotero_version), Zotero Connector version, browser, and operating system in your forum post.

#### Zotero for iOS

Generating a Report ID manually isn't possible on iOS, but you can provide a [Debug ID](debug_output#zotero_for_ios) for a specific action. The app may also automatically provide a Report ID if a crash occurs.

### 2. Provide Steps to Reproduce

In addition to your Report ID, you'll need to explain exactly what's happening and, ideally, how to consistently reproduce it: if you can, there's a very good chance we can fix it quickly or tell you how to fix it. If you're not able to reproduce the problem, explain what happened and what you were doing when it occurred.

Post a message to the forums with the following info:

1.  The exact steps you took to reproduce the problem, including specific URLs or files you accessed, any text you entered, buttons or other interface elements that you clicked on, etc.
2.  What happened, including **exact error messages** or other relevant text that you see on the screen
3.  What you expected to happen (unless you're reporting a clear error message)

Note that, after a problem occurs, other things in Zotero may temporarily break, so it's important to restart Zotero and try to repeat what you did before it occurred.

#### Bad:

> I can't add unfiled items to a collection after selecting a tag. Does anyone know how to fix this?

#### Good:

> Report ID: 1892199645

>

> When I drag an item from the Unfiled Items collection into another collection while a tag is selected in the bottom left, Zotero tells me it has to be restarted.

>

> Steps to reproduce:

>

> 1\. Start Zotero.
>
> 2\. In My Library, click the New Item menu and select Book.
>
> 3\. With the new item selected, add the tag "Foo" from the Tags tab in the right-hand pane.
>
> 4\. Click on "Unfiled Items" in the left pane.
>
> 5\. Click on "Foo" in the tag selector.
>
> 6\. Drag the item from the middle pane to another collection.

>

> Zotero displays the message "An error has occurred. Please restart Zotero." in the middle pane.

#### Bad:

> I can't figure out how to add a web page to Zotero.

#### Good:

> Report ID: 19672347

>

> I can't figure out how to add a web page to Zotero.

>

> Steps to reproduce:

>

> 1\. Click the "New Item" button in the Zotero toolbar.
>
> 2\. Open the "More" menu.

>

> I expected to see "Web Page" in this menu. I see "Artwork", "Audio Recording", "Bill", etc., but nothing about adding a web page. How can I add a web page?

## Reporting Startup Errors

For serious problems that prevent you from using the reporting wizard — such as Zotero not opening at all — we may need more information:

1\. First, be sure you've tried restarting your computer. Many startup errors will go away after a computer restart.

2a. If you're able to access the Zotero Help menu, go to "Help -> Debug Output Logging -> Restart with Logged Enabled…".

2b. If you can't access the Help menu, or if the problem doesn't occur during a restart, start Zotero via the command line. The steps for that depend on your platform:

##### macOS

1.  Open Terminal via Spotlight or from /Applications/Utilities.
2.  In the terminal window that opens, paste the following and press Return. `/Applications/Zotero.app/Contents/MacOS/zotero -ZoteroDebug -safe-mode`

If you can't select the logging window, you can press Cmd-\` (backtick, above Tab) to cycle between windows until it's selected.

##### Windows

1.  Press Windows-R or search for "run" to open the Run dialog.
2.  Click Browse and locate the Zotero application directory. This is typically "C:\\Program Files\\Zotero\\".
3.  Select the "zotero.exe" file and click Open.
4.  The complete path to the "zotero.exe" file will be displayed in the text box. Add " -ZoteroDebug -safe-mode" to the end, after any closing quotes, with a space before the hyphen. For example: `"C:\Program Files\Zotero\zotero.exe" -ZoteroDebug -safe-mode`
5.  Click OK.

##### Linux

-   From a terminal window, run `./zotero -ZoteroDebug -safe-mode` within the Zotero program directory.

  
3a. If you used "Restart with Logging Enabled…", you should be able to return to the Help menu, submit the output, and copy the given Debug ID to your forums post.

3b. If you used the command line, Zotero should start with a separate debug output window. Once it has stopped logging activity, click "Submit…" in the top section, click the clipboard icon to copy the Debug ID to the clipboard, and then paste the Debug ID into your forums post.

#### Alternative: Error Console

If the above steps don't work, repeat the steps above, try replacing `-ZoteroDebug` with `-jsconsole`. Zotero should open with a separate Error Console window showing errors that have occurred. Right-click on any lines with a pink background, select Copy, and paste them into your forums post.

#### Alternative: Logging to the Terminal

If Zotero is crashing, such that the debug window opened by `-ZoteroDebug` closes as well, you can [log to a terminal window](debug_output#logging_to_a_terminal_window) instead. Upload the debug output somewhere and provide a link in your forums thread or email the output to support@zotero.org with a link to your forum thread.
