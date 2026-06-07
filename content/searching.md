# Searching

## Quick Search

Quick searches provide a fast way to find items in a library or collection.

### Running a Quick Search

![](/_media/center-column-toolbar-z3.0.png){ .align-right }

To begin searching, click inside the search box at the top-right of the center pane (or type Ctrl/Cmd-F) and start typing your search terms. As you type, only those items in the center column that match the search terms will remain.

### Quick Search Options

![](/_media/basic-search-options.png){ .align-right }

Quick search can be used in three different modes:

-   "Title, Year, Creator" - matches against these three fields, as well as publication titles
-   "All Fields & Tags" - matches against all fields, as well as tags and text in notes
-   "Everything" - matches against all fields, tags, text in notes, and indexed text in PDFs

### Speeding Up Quick Searches

By default, the quick search searches automatically as you type. In very large libraries, this can result in a slow search experience.

To speed things up, type a quotation mark `"` mark at the beginning of the search field. This causes the search to start only when you type `Enter`/`Return`.

## Advanced Search

Advanced searches offer more and finer control than quick searches, and allow you to make [#saved searches](#saved searches).

### Running an Advanced Search

To open the Advanced Search window, click on the magnifying glass icon (![quick_start/advanced_search_icon.gif](/_media/quick_start/advanced_search_icon.gif)) at the top of the center pane.

![advanced_search/advanced_search_window.png](/_media/advanced_search/advanced_search_window.png)

In this window, you can filter items by the content of specific fields or by other properties, like item type or the collection an item belongs to. Multiple filters can be set up by clicking the plus button.

![advanced_search/full_search.png](/_media/advanced_search/full_search.png)

Set the library to search in by using the "Search in libary:" option at the top of the window. It is not currently possible to search in multiple libraries at one time.

By default, items only show up in the search results if they satisfy all search criteria. To change the search so that all items matching at least one criterion are returned, change the "Match" option to "any".

You can filter items by the collections or saved searches they belong to by searching by "Collection". To include items in subcollections of matching collections in the search results, check "Search subcollections".

To hide non-matching parent items with child items that do match the search criteria, and to collapse matching parent items with matching child items, check "Show only top-level items".

To match search criteria against both parent items and their children, check "Include parent and child items of matching items". If this option is selected and "Match" is set to "all", parent/child items will still show up if just part of the criteria is met by the parent item and the other part by a child item.

### Wild Cards

The % percent sign character acts as a wild card in advanced searches, substituting for zero or more characters. For example, the search term "W% Shakespeare" will match "W Shakespeare", "W. Shakespeare" as well as "William Shakespeare".

To search for items where a field contains any content at all, search for the % percent sign character alone.

### Saved Searches

When you save an Advanced Search, it appears as a collection in your library (but with a Saved Search icon, ![advanced_search/saved_search_icon.png](/_media/advanced_search/saved_search_icon.png) ![](/_media/saved_search_mac.png), instead of the regular collection icon). Saved Searches are continuously updated. For example, if you set up a Saved Search for "Date Added" "is in the last" "7" "days", the saved search will always show the items that have been added in the last 7 days. Saved searches only store the search criteria, not the search results.

To save a search, click the "Save Search" button in the Advanced Search window and provide a name for the search. Saved searches can be edited or deleted by right-clicking the Saved Search and selecting "Edit Saved Search…" or "Delete Saved Search…", respectively.

You can also create a saved search in a library by right-clicking on the library name and choosing "New Saved Search…".

### Complex Search Criteria

It is possible to run complex Boolean searches by using multiple Saved Searches. For example, to run the search `(a OR b) AND (c OR d)`, first make a Saved Search called "Condition1" for `(a OR b)`, then make a Saved Search called "Condition2" for `(c OR d)`. Finally, run a third Advanced Search and search for `"Collection" "is" "Condition1" AND "Collection" "is" "Condition2"`.

## PDF Full-Text Indexing

Full-text PDF indexing allows embedded text within PDFs to be searched with Quick Search (via the "Everything" option) and Advanced Search (via "Attachment Content"). Indexing happens automatically in the background when Zotero is idle.

You can control how much text in a PDF is indexed in the [Search pane](preferences/search) of [Zotero preferences](preferences) (default: 500000 characters, 100 pages). You can remove indexed text with the "Clear Index…" button or re-create the index from scratch using the "Rebuild Index…" button. You can check the index status of any PDF attachment by selecting the attachment item in the Zotero library and looking at the "Indexed:" field in the right pane.

If an item isn't being indexed (e.g., if it is not showing up in an 'Everything' Quick Search), verify that the item has searchable text and that the text is properly stored in the PDF (e.g., try to copy text out of the document and ensure that it is high quality). If the PDF has valid text, rebuild the item's index by right-clicking on it and choosing "Reindex Item". If you are still having issues, please ask a question on the [Zotero forums](/forum).

**Note:** At this time, only PDF full text content (and plain text files) can be indexed by Zotero. Other document types (e.g., .docx, .odt, .epub) cannot be indexed by Zotero.
