# Hidden Preferences

You can edit most Zotero preferences through the [Preferences](preferences) window in Zotero or the Preferences pane in the Zotero Connector in your browser. However, both Zotero and the Zotero Connector support additional hidden preferences. These settings may have received less testing and/or are intended for more advanced use.

## Zotero

To view the the full list of Zotero's preferences, including many hidden preferences, go to the Advanced pane of the Zotero preferences and click "Config Editor". Enter "zotero" into the Filter field at the top of the list that comes up. Preferences that can be safely changed by users are described below.

Most Zotero hidden preferences are preceded by "extensions.zotero."

### General Preferences

These general hidden preferences allow you to refine your Zotero configuration.

| Preference Name                | Default Value | Description                                                                                                                                                               |
|--------------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| backup.interval                | 1440          | Determines, at most, how often (in minutes) Zotero makes an automatic backup of the database. The default is every 24 hours (1440 minutes)                                |
| backup.numBackups              | 2             | Determines how many automatic database backups Zotero should keep. Excess backups are deleted oldest first. This does not include backups made during database upgrades.  |
| capitalizeTitles               | true          | By default, Zotero will recase titles of items you capture (e.g., to remove all caps). Switch this preference to false and you will preserve case information for titles. |
| debug.level                    | 5             | When debug.log is enabled, determines the lowest of the debug levels (1-5, with 5 being the lowest) that is displayed                                                     |
| debug.log                      | false         | Used for debugging Zotero. See [debug output](debug_output).                                                                                                             |
| debug.time                     | false         | When debug.log is enabled, shows the milliseconds from the previous debug call                                                                                            |
| fontSize                       | “1.0”         | This preference allows you to increase or decrease the size of text in the Zotero interface.                                                                              |
| httpServer.enabled             | true          | If set to true, Zotero will listen for requests from the Zotero Connector (e.g., to allow saving items to Zotero from the Connector).                                     |
| httpServer.port                | 23119         | If `httpServer.enabled` is enabled, this is the port on which Zotero will listen for connections from the Zotero Connector.                                               |
| sortAttachmentsChronologically | false         | If set to true, your attachments will be sorted by the order you added them instead of alphabetically.                                                                    |
| sortNotesChronologically       | false         | If set to true, your notes will be sorted by the order you added them instead of alphabetically.                                                                          |

### PDF Reader

| Preference Name                 | Default Value | Description                                                                      |
|---------------------------------|---------------|----------------------------------------------------------------------------------|
| sortNotesChronologically.reader | true          | Sort item notes in reverse chronological order. If `false`, sort alphabetically. |

### Note Editor

| Preference Name  | Default Value | Description                                                                                             |
|------------------|---------------|---------------------------------------------------------------------------------------------------------|
| note.css         |               | Custom CSS to apply to note content                                                                     |
| note.fontSize    | 14            | Note font size — settable from the View menu, but other values (including decimals) can be set manually |
| note.smartQuotes | true          | Automatically convert straight quotes to typographic quotes                                             |

### Translator Preferences

These hidden preferences allow you to control behavior for import/export translators for some specific bibliographic formats. **All translator hidden preferences are preceded by "extensions.zotero.translators."**

<table>
<thead>
<tr class="header">
<th>Preference Name</th>
<th>Default Value</th>
<th>Description</th>
<th>Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>RIS.import.ignoreUnknown</td>
<td>true</td>
<td>Do not store values that cannot be mapped to Zotero fields in notes.</td>
<td>RIS import translator</td>
</tr>
<tr class="even">
<td>RIS.import.keepID</td>
<td>false</td>
<td>Do not drop value from "ID -" tag. Can be used to find items in EndNote.</td>
<td>RIS import translator</td>
</tr>
<tr class="odd">
<td>BibTeX.export.dontProtectInitialCase</td>
<td>false</td>
<td>Do not surround words with braces if only the first letter is capitalized. Useful if you enter titles in Zotero in title case (not recommended).<br />
<code>false</code>: <code>{Tame {The} {BeaST}}</code><br />
<code>true</code>: <code>{Tame The {BeaST}}</code><br />
<em>Note that the first word is never surrounded if it does not contain internal upper-case letters</em></td>
<td>BibTeX export translator</td>
</tr>
<tr class="even">
<td>BibTeX.export.simpleCitekey</td>
<td>null</td>
<td>By default only for newly added entries the new simple format (disallowing any special character except dash and underscore) for the citekey are used. Setting this hidden key to true, such simple citekeys will be used always.</td>
<td>BibTeX export translator</td>
</tr>
</tbody>
</table>

### Full-Text Indexing

These preferences deal with Zotero's ability to create full-text indexes from imported files.

| Preference Name     | Default Value | Description                                                                                                                                                                                                                                                                            |
|---------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| search.useLeftBound | true          | This preference determines whether Zotero only finds word matches based on the left bound or whether it finds matches anywhere within words. Switching this to false may be beneficial for languages other than English, but it may significantly slow down Zotero's search functions. |

### Reports

These options allow you to customize your reports.

| Preference Name             | Default Value | Description                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| report.includeAllChildItems | true          | By default, selecting only parent items for a report causes those items' child notes and attachments to be included as well. If includeAllChildItems is set to false, only the items you have selected will be included. Selecting a combination of parent and child items will cause only the selected items to be displayed regardless of this setting. |
| report.combineChildItems    | true          | By default, Zotero groups child notes and attachments in reports together under their parent items. Switching this to false will cause notes to appear separately from their parent items. This can be helpful for people interested in using Zotero's note-taking features as an outlining tool.                                                         |

### Citation QuickCopy Settings

| Preference Name                                  | Default Value | Description                                                                                                                                                    |
|--------------------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| export.quickCopy.compatibility.indentBlockquotes | true          | Word and TextEdit don't indent blockquotes on their own and need this enabled. Results in an extra indent in LibreOffice, which handles blockquotes correctly. |
| export.quickCopy.compatibility.word              | false         | Add Word Normal style to paragraphs and enable double-spacing. LibreOffice inserts the conditional style code as a document comment.                           |
| quickCopy.quoteBlockquotes.plainText             | true          | Add quotes around blockquote paragraphs in plain-text output                                                                                                   |
| quickCopy.quoteBlockquotes.richText              | true          | Add quotes around blockquote paragraphs in rich-text output                                                                                                    |

### Word Processor Plugin

| Preference Name                         | Default Value | Description                                                                                                                                                                                      |
|-----------------------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| integration.keepAddCitationDialogRaised | false         | If you switch this to true, you can keep the Zotero word plugin interface for adding citations always at the front. and prevent it from going hidden behind the Word window you're working with. |

## Zotero Connector

To view hidden preferences for the Zotero Connector, open the preferences for the connector (by right-clicking on the save button and choosing Preferences/Options in Chrome and Firefox, or by long-pressing the save button in Safari). Then, click "Advanced", then "Config Editor".

### Translator Preferences

Zotero Connectors support some translator preferences that apply to all translators generally or to specific websites. To use these preferences, in the Zotero Connector Config Editor, click "Add Preference". Type or paste the preference's name and click "OK". Enter the appropriate preference value from the table below (e.g., **true** or **1**) and click "OK" again.

| Preference Name                 | Default Value | Description                                                                                                                                                                                                                                                                                    | Applies to                                                                                                                             |
|---------------------------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| translators.attachSupplementary | false         | Translators should attempt to attach supplementary data when importing items.                                                                                                                                                                                                                  | [All web translators implementing this behavior](https://forums.zotero.org/discussion/21880/supplementary-information/#Comment_153889) |
| translators.supplementaryAsLink | false         | Supplementary data attachments should be attached as links instead of being downloaded. This option has no effect if attachSupplementary is disabled. Setting this oprtion to "true" maintains the convenience of quick access to supplementary data, but speeds up saving items from the web. | [All web translators implementing this behavior](https://forums.zotero.org/discussion/21880/supplementary-information/#Comment_153889) |

**Note:** The supplementary data preferences will only work for sites whose translator supports this behavior. If you encounter sites with supplementary data that are not imported, please report it on the [Zotero forums](/forum).
