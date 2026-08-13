# Zotero Version History

Changes in released versions of Zotero 9.0 are documented on this page. To follow development, see the [commit log on GitHub](https://github.com/zotero/zotero/commits/). More recent changes may be available in a [beta build](beta_builds).

## Changes in 9.0.6 (July 7, 2026)

- Citation dialog optimizations
  - Allow clicking "Accept" button while item-details panel is open
  - Allow accepting the dialog via Cmd-Return / Ctrl-Enter from inside item-details panel
- Fixed detection of local file changes in group libraries (since 9.0)
- [Windows] Fixed "Custom UI Runtime Error in Zotero.dotm" in Word
- [LibreOffice] Fixed citing in tables at the very start of the document
- Fixed settings search not filtering and not highlighting results
- Disable locale selector when parent style specifies `default-locale`
- File renaming fixes
  - Fixed renaming not triggering on parent item type changes
  - Don't crash on unbalanced `{{endif}}` in template engine
- Fixed guidance dialog not showing on corrupted login manager
- [Security] Updated Mozilla platform to [140.12.0esr](https://www.mozilla.org/en-US/security/advisories/mfsa2026-58/)

## Changes in 9.0.5 (June 10, 2026)

*Zotero for Windows only*

- Fixed broken Word integration after Microsoft's June 9 Windows security updates (KB5094126)

## Changes in 9.0.4 (May 22, 2026)

- Fixed broken search/indexing after app had been open for a while
- Citation dialog: Restored sorting of libraries by number of cited items
- Read Aloud: Increased maximum reading speed to 3x
- [Mac/Safari] Fixed error in Google Doc with edited citations
- Reader fixes
  - Fixed shortcut keys on keyboards with alternative layouts
  - EPUB: Fixed outline navigation for some files
  - EPUB: Fixed page number not being recognized for some files
  
## Changes in 9.0.3 (May 6, 2026)

-   [Windows] Fixed Word plugin buttons not responding if Zotero.dotm couldn't be updated in Word Startup folder
-   Improved ISBN lookup
-   Fixed captcha loop on ScienceDirect in Find Full Text

## Changes in 9.0.2 (April 30, 2026)

-   [Security] Additional protections to prevent websites from triggering actions in Zotero or detecting that Zotero is installed
-   [Word for Windows] Fixed inline font getting lost when inserting rich text
-   Fixed possible incorrect localization due to plugin conflicts
-   Offer to reset data directory at startup if database cannot be read
-   Read Aloud: Fixed handling of short sentences when using Annotate Sentence
-   Reader: Fixed importing of non-ASCII characters from KOReader EPUB annotations
-   Reader: Fixed displaying of certain EPUB images
-   [Security] Updated Mozilla platform to [140.10.0esr](https://www.mozilla.org/en-US/security/advisories/mfsa2026-32/)
-   Miscellaneous bug fixes

## Changes in 9.0.1 (April 21, 2026)

-   Read Aloud improvements
    -   Toggle Read Aloud when pressing R or L key on keyboard
    -   Fixed potential failure starting reading
    -   Dismiss sentence-annotation popup by clicking outside of it
    -   Handle output device disconnection on Windows
-   Fixed context menu sometimes not working after plugin removal
-   Fixed failure to load items at startup with some databases
-   Fixed "{$version}" sometimes showing in post-upgrade banner
-   Fixed word processor plugin error after citing and then deleting an Audio Recording, Presentation, or Video Recording item
-   Preserve entered text when pasting into item tags field
-   Fixed link to related item sometimes not working
-   Added an icon for the Relate Items menu option
-   Fixed tag swatches showing in wrong order for selected item rows
-   Fixed plugin default preferences not getting updated after a plugin update
-   Reader: Fixed display of HTML emails exported from Thunderbird

## Changes in 9.0 (April 10, 2026)

**See the [Zotero 9 announcement](/blog/zotero-9) for more details.**

-   **Read Aloud**
    -   Listen to PDFs, EPUBs, and webpage snapshots with high-quality, natural-sounding voices
    -   Annotate the last sentence with a single click or H/U on keyboard
    -   Last reading position is saved and synced across devices
    -   Start reading from any paragraph by clicking in the left margin while Read Aloud is activated
-   **"Recently Read" collection**
    -   New virtual collection in each library showing items you've read recently, synced across devices
    -   "Last Read" column available in all views
    -   "Attachment Last Read" search condition
-   **Insert annotations directly into word processor documents**
    -   New "Add Annotation" button in word processor plugins lets you insert annotations directly into your document with active Zotero citations
-   **"Added By" and "Modified By" for group libraries**
    -   View the group members who added and/or last edited items
    -   Shown in item pane, and can be added as columns in the items list
-   **Per-group file renaming settings**
    -   Group admins can configure file-renaming templates for each group library
-   **Performance improvements**
    -   Reduced startup memory usage by ~20%
    -   Detect external local attachment changes during sync without scanning all files
    -   Avoid repeated checks for remotely missing files during sync
    -   [Mac] Use APFS cloning for file copies, including automatic database backups, to save potentially hundreds of megabytes or gigabytes of local disk space
-   **Web-based login flow**
    -   Log in to your Zotero account via the browser instead of entering credentials in the app
    -   Allows use of password managers
    -   Allows for two-factor authentication (currently [testable](https://forums.zotero.org/discussion/comment/510383/#Comment_510383); available by default soon)
-   Improved RTF Scan
    -   Support for citations with extended and non-Latin characters in authors and titles
    -   Support for additional quotation mark styles
    -   Support for citations with title but no author
    -   Support for documents with multiple character encodings
-   Reader changes
    -   Improved theme handling for scanned PDFs
    -   Support for fixed-layout EPUBs
    -   Don't apply dark-mode styling to printed PDFs
-   Items list improvements
    -   "+"/"-" keys now expand/collapse all rows one level at a time
    -   Added Director as fallback for Creator for Video Recording items
    -   Fixed Title column not being recoverable when hidden by a plugin
-   Added "Citation Key" column to items list
-   Search by citation key in search bar ("Title, Creator, Year" mode) and citation dialog
-   "View Online" button for PMID/PMCID to open PubMed and PubMed Central directly from the item pane
-   Fixed "Rename File to Match Parent Item" button sometimes not appearing
-   Don't merge trashed attachments
-   Improved keyboard navigation in drop-down menus
-   Exclude ©, ®, and ™ from emoji detection
-   Show account email addresses in sync settings
-   Various other improvements and bug fixes

## Older Changes

[View the 8.0 changelog](8.0_changelog)
