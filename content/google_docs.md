# Using Zotero with Google Docs

Zotero's powerful Google Docs support helps you easily add citations and bibliographies to the documents you create in Google Docs.

You can quickly search for items in your Zotero library, add page numbers and other details, and insert citations. When you're done, a single click inserts a formatted bibliography based on the citations in your document. Zotero supports complex style requirements such as *Ibid.* and name disambiguation, and it keeps your citations and bibliography updated as you make changes to items in your library. If you need to switch citation styles, you can easily reformat your entire document in any of the over 10,000 citation styles that Zotero supports.

Google Docs support is provided by the [Zotero Connector](/download/connectors) for Chrome, Firefox, Edge, and Safari and requires the Zotero program to function.

Using another word processor? Zotero also integrates with [Word](word_processor_plugin_usage) and [LibreOffice](libreoffice_writer_plugin_usage).

## Citation Interface

The Zotero Connector adds a Zotero menu to the Google Docs interface:

![google-docs-menu.png](/_media/google-docs-menu.png){ width=400 }

It also adds a toolbar button for one-click citing:

![google-docs-toolbar.png](/_media/google-docs-toolbar.png){ width=300 }

In the Zotero menu, you'll find the following options:

<table>
<thead>
<tr class="header">
<th>Add/Edit Citation</th>
<th>Add a new citation or edit an existing citation in your document at the cursor location.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Add Annotation</td>
<td>Insert annotations created in Zotero Reader at the cursor location.</td>
</tr>
<tr class="even">
<td>Add Note</td>
<td>Insert a note created in Zotero at the cursor location.</td>
</tr>
<tr class="odd">
<td>Add/Edit Bibliography</td>
<td>Insert a bibliography at the cursor location or edit an existing bibliography.</td>
</tr>
<tr class="even">
<td>Preferences</td>
<td>Open the Document Preferences window, e.g. to change the citation style.</td>
</tr>
<tr class="odd">
<td>Refresh</td>
<td>Refresh all citations and the bibliography, updating any item metadata that has changed in your Zotero library.</td>
</tr>
<tr class="even">
<td>Unlink Citations</td>
<td>Unlink Zotero citations in the document by removing the field codes. This prevents any further automatic updates of the citations and bibliographies.<br />
Note that removing field codes is <strong>irreversible</strong> and should usually only be done in a final copy of your document.</td>
</tr>
</tbody>
</table>

<span id="authentication"/><!-- For old links from connector -->

## Authorization

Interacting with the Zotero functionality for the first time in a document will prompt you to authorization the plugin to access your Google account. Be sure to:

1\. Select the Google account you used to create the document or that has been given editing access by the document's creator. This is unrelated to any Zotero account you may have, which isn't required to use Zotero or Google Docs integration.

2\. Grant Zotero the permission to "See, edit, create and delete all your Google Docs documents". Zotero requires this permission to be able to insert and modify citations into your document. The plugin doesn't do anything else with your document content and doesn't access documents other than the ones on which it's triggered. The integration works entirely locally on your computer, so even when you trigger the plugin on a given document, nothing is sent to Zotero servers.

Once you've authorized the plugin to access your document, you can begin inserting citations from your Zotero libraries.

## Citing

You can begin citing by clicking the ![](/_media/zotero-z-16px-offline.png){ width=16 } ("Add/Edit Zotero Citation") button in the Google Docs toolbar or by selecting "Add/Edit Citation" from the Zotero menu, both of which will bring up the citation dialog.

<!-- include: integration/citation_dialog.md -->

## Bibliography

Clicking the “Add/Edit Bibliography” menu option inserts a bibliography at the cursor location.

You can edit which items appear in the bibliography by clicking the “Add/Edit Bibliography” button again, which will open the bibliography editor. See [Editing the Bibliography](#editing_the_bibliography) below for more info. Manual edits made to the bibliography in the document will be overwritten the next time Zotero refreshes the document.

## Editing the Bibliography

<!-- include: integration/edit_bibliography/_start.md -->
To do this, select the “Add/Edit Bibliography” option again to open the Edit Bibliography window:
<!-- include: integration/edit_bibliography/_middle.md -->

If you need to edit items in your bibliography, it is best to do this as a final step before submitting the document. First, save a backup copy of the document. Then, in the copy, use the "Unlink Citations" menu option to disconnect your document from Zotero and convert all citations and the bibliography to regular text. Finally, make your adjustments to the bibliography text.
<!-- include: integration/edit_bibliography/_end.md -->

## Add Note 

You can insert notes from Zotero into the document via “Add Note” option. Citations in the note, including those generated from PDF annotations, will remain active, so they’ll automatically be added to your bibliography.

![](/_media/word_integration/insert-note-example.png){ .align-right width=700 }

<div style="clear: both;"></div>

## Add Annotation

You can insert annotations from your attachments into the document via “Add Annotation” option. Annotations will include any added comments and create active Zotero citations that automatically generate bibliography entries.

![](/_media/word_integration/add-annotations-example.png){ .align-right width=700 }

<div style="clear: both;"></div>

## Collaboration

Google Docs is designed to let you collaborate on documents, and Zotero’s integration is no different. You and your coauthors can all insert and edit citations in a shared document, and you don't even need to be in a Zotero group. If you're planning a large collaborative project, though, we recommend using a group library, which not only makes it easy to collect and manage materials but will also allow all collaborators to change cited item metadata (authors, title, date of publication, etc.). If someone cites an item from their personal library, only they will be able to update the metadata for that item.

We recommend that anyone making changes to the document have the Zotero Connector installed. (The Zotero app itself is necessary only if inserting or editing citations.) If someone cuts and pastes an active citation without the Zotero Connector, the citation will be unlinked from Zotero and disappear from the bibliography, and the next person refreshing the document with the Zotero Connector will receive a warning about unlinked citations. While people without the Connector can theoretically edit non-citation parts of the document, we don't recommend it due to the risk of accidental citation unlinking.

When working collaboratively on a document, you and your coauthors should avoid inserting or editing citations at the same time. The Zotero Connector has mechanisms in place to prevent document and citation corruption from concurrent citation editing, but due to technical limitations they do not provide perfect safety.

## Document Preferences

![Zotero Document Preferences in Google Docs](/_media/gdocs_document_preferences.png){ .align-right width=370 }

The "Document Preferences" window lets you set the following document-specific preferences:

1.  The [citation style](styles).
2.  The language to use to format citations and bibliography.
3.  For styles that abbreviate journal titles (e.g., "Nature"), whether to use the **MEDLINE** abbreviations list to abbreviate titles.
    -   If this option is selected (the default), the contents of the "Journal Abbr" field in Zotero will be ignored.
4.  Whether Zotero should automatically update citations for disambiguations, ibid and numbering, or whether updating should be delayed until a manual refresh. Note that if you enable this mode, Zotero will insert your citations with a gray background to indicate that the citation text is not final. The citation will be finalized and the gray background removed once you click "Refresh" in the Zotero menu.

<div style="clear: both;"></div>

## Saving for Publication

When you're ready to submit your document, use File → "Make a copy…" and, in the new document, use Zotero → "Unlink Citations" to convert the citations and bibliography to plain text. You can then download that second document (e.g., as a PDF), while keeping active citations in the original document in case you need to make further changes. Zotero will prompt you to create a copy if you try to download your original document.


## Keyboard Shortcuts

You can use keyboard shortcuts for improved accessibility and faster citing.

Press Ctrl-Command-C (Mac) or Ctrl-Alt-C (Windows/Linux) to insert a citation. You can configure this from the Advanced pane of the Zotero Connector preferences.

<!-- include: integration/citation_dialog_keyboard_commands.md -->

## Limitations

While we've tried to create the same experience available in Word and LibreOffice, there are some limitations to be aware of when working in Google Docs:

-   As noted [above](#collaboration), anyone making changes to the document should have the Zotero Connector installed. (The Zotero app itself is necessary only if inserting or updating citations.) Citations that are cut and pasted without the Connector installed will be unlinked.
-   Dragging citations within the document will cause the citations to become unlinked. Cutting and pasting is fine as long as the Zotero Connector is installed.
-   If someone views the document without having the Zotero Connector installed, or if you download the document instead of first making a copy and unlinking citations, active citations in the document will show up as links leading to URLs such as <https://www.zotero.org/google-docs/?abc123>.
-   Citation inserts and edits slow down significantly as the number of citations increases. With 100+ citations, a single citation update can take up to 10 seconds, so for longer documents you'll want to disable automatic citation updates in the Zotero document preferences.
-   Google Docs provides limited facilities for text formatting. Styles that use small caps fonts will not use a true small caps formatting style in Google Docs and will instead fall back to the "Alegreya Sans SC" font. Citations that have been inserted with automatic citation updates disabled will be inserted with a gray background instead of dashed underlining like in Word and LibreOffice.

## Troubleshooting

### Menu doesn't appear

If nothing appears when you click the Zotero menu, or you see a thin gray line, try restarting Zotero and your browser.

If that doesn't help, disable all other browser extensions, reload Google Docs, and try again. In particular, the Google Docs Offline extension has been reported as interfering with Zotero's Google Docs integration.

In some browsers, you may need to give the Zotero Connector permission to run. While Google Docs support only requires access to `docs.google.com` and `www.zotero.org`, if you're going to be using Zotero, you'll want to use the Zotero Connector to save to Zotero, and for that to work it needs to be able to run on all sites. (See [why this is safe](privacy#zotero_connector).) In Chrome or Edge, right-click on the Save to Zotero button in your browser toolbar, select "This Can Read and Change Site Data", and choose "On All Sites". In Safari, go to the Websites tab of the Safari settings, click on Zotero Connector in the left column, and make sure any sites that show up and "For other websites" at the bottom are all set to "Allow".

If it's still not working, try in a new browser profile (e.g., a new Chrome profile) or in a different browser.

### Citation dialog doesn't appear after clicking Add/Edit Citation

If you can open the Zotero menu but the citation dialog doesn't appear after you click Add/Edit Citation, make sure that a dialog isn't appearing behind your other browser or Zotero windows.

If you're sure that's not the problem, generate a [Debug ID](debug_output) for reloading Google Docs and clicking Add/Edit Citation and post it to the Zotero Forums along with a description of the problem.

### Unlinked Citations

See [Why isn’t Zotero detecting my existing Google Docs citations?](kb/google_docs_citations_unlinked)

### “The Google account you selected does not have permission to edit this document”

You likely selected the wrong Google account. See [Authentication](#authentication).

### Other problems

If you encounter other problems citing in Google Docs, let us know in the [Zotero Forums](https://forums.zotero.org). Provide a [Debug ID](debug_output) from the Zotero Connector for reloading Google Docs and trying to perform the relevant action.

You should always troubleshoot in a new, empty document or with a copy of the original document, using File → "Make a copy…". If something isn't working in a particular document, the document version history may allow you to revert to an earlier version. Some of the [Debugging Broken Documents](word_processor_plugin_troubleshooting#debugging_broken_documents) steps may also be useful in Google Docs.
