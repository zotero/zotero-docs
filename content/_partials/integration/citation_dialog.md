The Citation Dialog has two modes: List and Library. You can switch between the two modes using the toggle in the bottom right corner, preserving any added items or entered search terms. By default, it will open in the last mode you used, but you can choose a different default mode in the Cite tab of the Settings.


List mode lets you quickly search for citations across your Zotero libraries by title, creator, and year. Matching items will be listed below the search field. Items you are currently working with will appear
at the top of the list in their respective sections:

- items you have manually selected in Zotero under "Selected Items"
- currently open Zotero reader tabs under "Open Documents"
- items already cited in this document under "Cited Items"

All other items are grouped by their library (My Library and any groups you are part of). Libraries are sorted by the number of cited items in the current document.

![citation-dialog-list.png](/_media/word_integration/citation-dialog-list.png)

Library mode includes a full library browser, letting you find items in specific libraries or collections.
"Selected Items", "Open Documents" and "Cited Items" appear in the horizontal scrollable area above the library browser.
Only items in the selected library or collection that match your search are shown.

![citation-dialog-library.png](/_media/word_integration/citation-dialog-library.png)

Select an item by clicking on it, by pressing Enter/Return when it is highlighted, or by clicking on the "+" icon next to it in Library mode. The item will appear in the Citation Dialog in a shaded bubble. Press Enter/Return again, or click the "Accept" icon, to insert the citation and close the Citation Dialog.
To remove an unwanted item from the citation, place the cursor before it and press Delete/Backspace.

You can click on the bubble of a cited item, or select it with arrow keys and press Space,
to open the citation details popup. There, you can [customize your citation](#customizing_citations), or click "Open in My Library (or the Group Library's name)" to view the item in Zotero. Items that are orphaned (not connected to any items in your Zotero database) will not have an "Open in My Library" button. Orphaned items can exist if they were inserted by a collaborator from their My Library or a group you don't have access to or if they were deleted from your Zotero library.

### Citations with Multiple Cited Items

![citation-dialog-multi-item-example.png](/_media/word_integration/citation-dialog-multi-item-example.png){ .align-right width=300 }

To create a citation containing multiple cites (e.g., "[2,4-6]" for numeric styles or "(Smith 1776, Schumpeter 1962)" for author-date styles), add them one after the other in the Citation Dialog. After selecting the first item, don't press Enter/Return, but type the author, title, or year of the next item.

![citation-dialog-sorted-example.png](/_media/word_integration/citation-dialog-sorted-example.png){ .align-right width=300 }

Some citation styles require that items within a single in-text citation are ordered either alphabetically (e.g., "(Doe 2000, Grey 1994, Smith 2008)") or chronologically ("(Grey 1994, Doe 2000, Smith 2008)"). Zotero will follow these sort rules automatically.

-   To disable automatic sorting of the cites in the citation, drag the citations to rearrange them in the Citation Dialog. You can also click the settings button in the bottom right corner of the Citation Dialog and uncheck the "Keep Sources Sorted" option. *This option only appears for citation styles that specify a sort order for citations.* To restore automatic sorting, re-check the "Keep Sources Sorted" option.

### Customizing Citations

Citations can be customized in various ways.

If a citation is simply incorrect or missing data, start by making sure that the item metadata in Zotero is correct and complete, and then click Refresh in the plugin to update your document with any changes.

Other customizations can be made via the Citation Dialog. Click an existing citation in your document and click Add/Edit Citation to open the Citation Dialog, and then click the citation bubble to open the citation details popup, where you can make the following changes.

#### Page and Other Locators

![citation-dialog-locator-example.png](/_media/word_integration/citation-dialog-locator-example.png){ .align-right width=350 }

In some cases you want to cite a certain part of an item, e.g. a certain page, page range or volume. This additional cite-specific information (e.g. "pp. 4-7" in the cite "Doe et al. 2001, pp. 4-7") is called the "locator".

The citation details popup has a dropdown list of the different locator types ("Page" is the default), and a text box in which you can enter the locator value (e.g. "4-7"). To cite a locator other than the ones listed (e.g., "Table"), use the Suffix field.

![citation-dialog-locator-example.png](/_media/word_integration/citation-dialog-locator-example-2.png){ .align-right width=350 }

You can also add page numbers from the keyboard as you insert citations. Any number you type (e.g. 10-15) right after adding a new item will be added to it as a page locator. Press Cmd/Ctrl+Z to undo this automatic locator placement.

You can add any other locator by typing it in full or short form (e.g. "line 20-25" or "l 20-25") and pressing Enter.
Finally, if a locator is typed together with the search term, it will also be added to the next item you select.

#### Prefix and Suffix

The "Prefix" and "Suffix" text boxes allow you to specify text to respectively precede and follow the automatically generated cite. For example, instead of "Tribe 1999", you might want "cf. Tribe 1999, see also…".

![citation-dialog-suffix-example.png](/_media/word_integration/citation-dialog-suffix-example.png){ .align-right width=350 }

Any text in the prefix and suffix fields can be formatted with the HTML tags `<i>` (for italics), `<b>` (bold), `<sub>` (subscript), and `<sup>` (superscript). For example, typing "`<i>`cf`</i>`. the classic example" will be displayed as "*cf*. the classic example".

Prefixes and suffixes can be applied to each item in a citation to create complex citations. For example: "(see Smith 1776 for the classic example; Marx 1867 presents an alternate view)". Modifying citations by entering text into the Prefix and Suffix fields is always preferable to directly typing in the citation fields in Word. Manual modifications will prevent Zotero from automatically updating the citation.

#### Narrative Citations with Omit Author ("According to Smith (1776)")

With author-date styles, authors are often moved into the text and omitted from the following parentheses-enclosed citation, e.g.: "According to Smith (1776), the division of labor is crucial…". To omit the authors from the cite, check the "Omit Author" box, which will result in a cite like "(1776)" instead of "(Smith, 1776)", and write the author's name ("Smith") as part of the regular text in your document.

#### Other Changes

If your citation still isn't displaying the way you want, you can edit the citation directly in your document, but note that doing so will prevent Zotero from being able to automatically update the citation to reflect other changes in the document (e.g., for 'ibid.' or given name disambiguation). After you make a manual edit, Zotero will ask you to confirm that you want to keep the edit and prevent the citation from being updated automatically going forward. It may be preferable to instead make notes in the text of changes you want to make, wait until you're ready to submit the document, and make the changes in a copy of the document after using Unlink Citations.

If you believe there's an error in a citation style, post to the Zotero Forums so that we can investigate and, if necessary, correct the style. If a style is updated, your document will automatically update to reflect any changes the next time you refresh the document.

### Switching to the "Classic View"

The Classic Citation Dialog has been deprecated. See: [What happened to classic view](https://www.zotero.org/support/kb/classic_citation_dialog/).