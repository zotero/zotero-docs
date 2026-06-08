---
tags:
  - pref
---

# Search

For information on how to use Zotero's search features, see [Searching](searching). This page describes the settings in the Search pane of the Zotero preferences.

The Search preferences pane is used to configure and manage Zotero's [PDF/EPUB/HTML full-text search](searching#PDF_Fulltext_Indexing) feature.

![](/_media/preferences_search.png){ width=600 }

### Full-Text Cache

Zotero creates an index to allow the full text contents of PDF and plain-text attachments in your library to be searched with Quick Search ("Everything" option) and Advanced Search (via "Attachment Content").

**Note:** At this time, PDF, EPUB, and HTML full text content (and plain text files) can be indexed by Zotero. Other document types (e.g., .docx, .odt) cannot be indexed by Zotero.

This section includes these options to manage your full-text index:

-   **Rebuild Index…:** Re-create the full-text index for all items from scratch. This option may be helpful if you use OCR text recognition on a large number of attachment files or you find that searching using "Everything" or "Attachment Content" does not return the correct results.
    -   Before using this option, verify that the item has searchable text and that the text is properly stored in the PDF/EPUB/HTML (e.g., try to copy text out of the document and ensure that it is high quality).
    -   You can re-index individual PDF/EPUB/HTML files in your library by right-clicking on the relevant attachment item in your library and choosing "Reindex Item".
    -   If you are still having issues after re-indexing an item, please ask a question on the [Zotero Forums](/forum).
-   **Clear Index…:** Delete the full-text index. Use this option if you intend to disable full-text indexing and wish to reduce the size of your Zotero database (note that the full-text index typically occupies a relatively small amount of storage space on your computer).
-   **Maximum characters to index per file:** The number of characters to index in PDF/EPUB/HTML and plain-text attachments (default: 500,000, approximately 100,000 words or 180–200 pages of content). Set this value to 0 to disable full-text indexing.

### Index Statistics

This section provides details about the size of the full-text index in your Zotero database. Reported statistics are:

-   **Indexed:** The total number of files that are completely indexed.
-   **Partial:** The number of files that are partially indexed.
-   **Unindexed:** The number of files that are not yet indexed.
-   **Words:** The total number of words included in your Zotero library's full-text index.


