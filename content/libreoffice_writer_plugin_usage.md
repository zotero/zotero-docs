# Using the Zotero LibreOffice Plugin

These are instructions for using the Zotero LibreOffice Plugin. For plugins for Word or Google Docs, see [Word Processor Plugins](word_processor_integration).

## Zotero Plugin Toolbar

![libreoffice_integration_tab.png](/_media/word_integration/libreoffice_integration_tab.png){ .align-right width=200 }

[Installing the Zotero LibreOffice plugin](word_processor_plugin_installation) adds a Zotero toolbar to LibreOffice.

The Zotero toolbar contains these icons:

<table>
<tbody>
<tr class="header">
<td>Add/Edit Citation</td>
<td><img alt="zotero-toolbar-word-add-edit-citation.png" src="/_media/word_integration/zotero-toolbar-word-add-edit-citation.png"></td>
<td>Add a new citation or edit an existing citation in your document at the cursor location.</td>
</tr>
<tr class="odd">
<td>Add Annotation</td>
<td><img alt="zotero-toolbar-word-add-annotation.png" src="/_media/word_integration/zotero-toolbar-word-add-annotation.png" width="16"></td>
<td>Insert annotations created in Zotero Reader at the cursor location.</td>
</tr>
<tr class="even">
<td>Add Note</td>
<td><img alt="Add Note" src="/_media/word_integration/zotero-toolbar-word-add-note.png" width="16"></td>
<td>Insert a note created in Zotero at the cursor location.</td>
</tr>
<tr class="odd">
<td>Add/Edit Bibliography</td>
<td><img alt="zotero-toolbar-word-add-edit-bibliography.png" src="/_media/word_integration/zotero-toolbar-word-add-edit-bibliography.png"></td>
<td>Insert a bibliography at the cursor location or edit an existing bibliography.</td>
</tr>
<tr class="odd">
<td>Refresh</td>
<td><img alt="zotero-toolbar-word-refresh.png" src="/_media/word_integration/zotero-toolbar-word-refresh.png" width="16"></td>
<td>Refresh all citations and the bibliography, updating any item metadata that has changed in your Zotero library.</td>
</tr>
<tr class="even">
<td>Document Preferences</td>
<td><img alt="zotero-toolbar-word-doc-prefs.png" src="/_media/word_integration/zotero-toolbar-word-doc-prefs.png" width="16"></td>
<td>Open the Document Preferences window, e.g. to change the citation style.</td>
</tr>
<tr class="odd">
<td>Unlink Citations</td>
<td><img alt="zotero-toolbar-libreoffice-unlink-citations.png" src="/_media/word_integration/zotero-toolbar-libreoffice-unlink-citations.png" width="18"></td>
<td>Unlink Zotero citations in the document by removing the field codes. This prevents any further automatic updates of the citations and bibliographies.<br />
Note that removing field codes is <strong>irreversible</strong>, and should usually only be done in a final copy of your document.</td>
</tr>
</tbody>
</table>

## Citing

You can begin citing with Zotero by clicking the "Add/Edit Citation" (![zotero-toolbar-word-add-edit-citation.png](/_media/word_integration/zotero-toolbar-word-add-edit-citation.png){ width=16 }) button. Pressing the button brings up the Citation Dialog. It is used to select items from your Zotero library and create a citation.

<!-- include: integration/citation_dialog.md -->

## Bibliography

Clicking the “Add/Edit Bibliography” (![zotero-toolbar-word-add-edit-bibliography.png](/_media/word_integration/zotero-toolbar-word-add-edit-bibliography.png){ width=16 }) button inserts a bibliography at the cursor location.

As you use the plugin, Zotero will automatically update the bibliography based on the citations in the document.

In the rare case that you want to add items to the bibliography that you haven't cited in the document, you can click the “Add/Edit Bibliography” button again, which will open the [bibliography editor](#editing_the_bibliography). Manual edits made to the bibliography in LibreOffice will be overwritten the next time Zotero refreshes the document.

## Editing the Bibliography

<!-- include: integration/edit_bibliography/_start.md -->
To do this, click the “Add/Edit Bibliography” (![zotero-toolbar-word-add-edit-bibliography.png](/_media/word_integration/zotero-toolbar-word-add-edit-bibliography.png){ width=16 }) button again to open the Edit Bibliography window:
<!-- include: integration/edit_bibliography/_middle.md -->

If you need to edit items in your bibliography, it is best to do this as a final step before submitting the document. First, save a backup copy of the document. Then, click the "Unlink Citations" button (![zotero-toolbar-libreoffice-unlink-citations.png](/_media/word_integration/zotero-toolbar-libreoffice-unlink-citations.png){ width=18 }) to disconnect your document from Zotero and convert all citations and the bibliography to regular text. Finally, make your adjustments to the bibliography text.

<!-- include: integration/edit_bibliography/_end.md -->

## Add Note 

You can insert notes from Zotero into the document via “Add Note” (![zotero-toolbar-word-add-note.png.png](/_media/word_integration/zotero-toolbar-word-add-note.png){ width=18 }) button. Citations in the note, including those generated from PDF annotations, will remain active, so they’ll automatically be added to your bibliography.


![](/_media/word_integration/insert-note-example.png){ .align-right width=700 }

<div style="clear: both;"></div>

## Add Annotation

You can insert annotations from your attachments into the document via “Add Annotation” (![zotero-toolbar-word-add-annotation.png.png](/_media/word_integration/zotero-toolbar-word-add-annotation.png){ width=18 }) button. Annotations will include any added comments and create active Zotero citations that automatically generate bibliography entries.

![](/_media/word_integration/add-annotations-example.png){ .align-right width=700 }

<div style="clear: both;"></div>

## Document Preferences

![document-preferences-5-0.png](/_media/word_integration/document-preferences-5-0.png){ .align-right width=370 }

The "Document Preferences" window lets you set the following document-specific preferences:

1.  The [citation style](styles).
2.  The language to use to format citations and bibliography.
3.  For note-based styles (e.g., "Chicago Manual of Style (Note)"), whether citations are inserted in footnotes or endnotes.
    -   Note that Word, not Zotero, controls the style and format of footnotes and endnotes.
4.  Whether to store citations as **ReferenceMarks** or **Bookmarks**.
    -   Unless you need to collaborate with colleagues using Word, you should always choose ReferenceMarks.
5.  For styles that abbreviate journal titles (e.g., "Nature"), whether to use the **MEDLINE** abbreviations list to abbreviate titles.
    -   If this option is selected (the default), the contents of the "Journal Abbr" field in Zotero will be ignored.

<div style="clear: both;"></div>


## Keyboard Commands

The Zotero LibreOffice plugin can be used with just the keyboard for improved accessibility and faster use.
[Keyboard shortcuts](word_processor_plugin_shortcuts#libreoffice) can be set up for all the buttons in the Zotero tab.

<!-- include: integration/citation_dialog_keyboard_commands.md -->

## Troubleshooting

If you run into problems while trying to use the Zotero LibreOffice plugin, make sure to check out the [word processor plugin troubleshooting](word_processor_plugin_troubleshooting) page.
