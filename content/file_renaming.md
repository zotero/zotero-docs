# File Renaming

Zotero automatically renames PDFs and other files saved to your library based on the bibliographic details (title, author, etc.) of the parent item, freeing you from having to sort through piles of randomly named files or manually rename each new file to your preferred format.

Zotero will always rename files saved from the web (via the Zotero Connector, Add Item by Identifier, or Find Available PDF).

By default, it will also rename the first [stored](attaching_files#stored_files_and_linked_files) PDFs and EPUBs you add to items, as well as files for which it successfully [retrieves metadata](retrieve_pdf_metadata). You can disable this by unchecking "Automatically rename attachment files using parent metadata" ("Automatically rename files" in Zotero 8) in the General pane of the Zotero settings. If an item already has an attachment, additional files will not be automatically renamed, to avoid changing the filenames of supplementary materials.

**Starting in Zotero 8,** Zotero will keep attachment filenames in sync as you make changes to parent item metadata (e.g., changing the title). In previous versions, you would need to right-click on the attachment and select Rename File from Parent Metadata to update a filename after editing metadata.

Linked files are not automatically renamed by default, but you can enable the "Rename linked files" setting in to apply renaming to those as well.

You can use "Rename files of these types" to adjust which file types are automatically renamed.

## Attachment Title vs. Filename

The attachment filename being renamed is separate from the attachment title shown in the items list. See [Attachment Title vs. Filename](kb/attachment_title_vs_filename) for more information.

## Customizing the Filename Format

By default, Zotero names files after the parent item's creator (1–2 authors or editors), year, and title:

`Lee et al. - 2023 - The First Room-Temperature Ambient-Pressure Superconductor.pdf`

While Zotero has always renamed files automatically, Zotero 7 introduces a new, powerful syntax for customizing filenames. The default format can be customized from the General pane of the Zotero settings.

This is the default template string:

`{{ firstCreator suffix=" - " }}{{ year suffix=" - " }}{{ title truncate="100" }}`

The following variables and parameters are supported:

### Variables

| Variable          | Description                                                                                                                       |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `authors`         | Parent item's principal creators (depending on the item type these are authors or artists but not editors or other contributors). |
| `authorsCount`    | The number of the parent item's principal creators. *(Zotero 7.0.16)*                                                             |
| `editors`         | Parent item's editors.                                                                                                            |
| `editorsCount`    | The number of parent item's editors. *(Zotero 7.0.16)*                                                                            |
| `creators`        | All parent item's creators.                                                                                                       |
| `creatorsCount`   | The total number of all parent item's creators. *(Zotero 7.0.16)*                                                                 |
| `firstCreator`    | Parent item's creator (1–2 authors or editors), same as the value of the "Creator" column.                                        |
| `itemType`        | Parent item type. Complete list of recognized item types can be found [here](https://api.zotero.org/itemTypes)                    |
| `attachmentTitle` | The title of the attachment that is being renamed or created                                                                      |
| `year`            | Year, extracted from parent item's date field.                                                                                    |
| `accessDate`      | Access date in UTC, or in a specified time zone if the `timeZone` parameter is present. *(Zotero 8)*                              |
| Any item field    | Complete list of fields can be found at the bottom of this page.                                                                  |

If a variable value starts or ends with a space, which is likely to happen when used in conjunction with the
`truncate` parameter, these spaces are removed from the filename.

### Parameters

| Parameter             | Variables                        | Default Value                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|-----------------------|----------------------------------|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `start`               | All                              |                               | Truncates the variable value from the beginning. For example, `{{ title start="5" }}` will be replaced with the value of the parent item's title, omitting the first 5 characters. It can be combined with `truncate`.                                                                                                                                                                                                                                                                                                                                                                          |
| `truncate`            | All                              |                               | Truncates variable value at fixed number of characters, e.g, `{{ title truncate="20" }}` will be replaced with the first 20 characters of parent item's title. Truncation happens after every other parameter has been applied, except for `prefix`, `suffix` and `case`.                                                                                                                                                                                                                                                                                                                       |
| `prefix`              | All                              |                               | Prepends variable with given character(s), e.g., `{{ title prefix="title" }}` will be replaced by the word “title” followed by parent item's title. If variable is empty (e.g., item's parent has empty title), entire statement, including prefix, is ignored.                                                                                                                                                                                                                                                                                                                                 |
| `suffix`              | All                              |                               | Appends given character(s) at the end of a variable with, e.g., `{{ title suffix="!" }}` will be replaced by parent item's title followed by an exclamation mark. If variable is empty (e.g., item's parent has empty title), entire statement, including suffix, is ignored.                                                                                                                                                                                                                                                                                                                   |
| `case`                | All                              |                               | Converts case of a variable, following values are accepted: `upper`, `lower`, `sentence`, `title`, `hyphen`, `snake`, `camel` and, added in Zotero 7.0.16, `pascal`. E.g., `{{ title case="snake" }}` will result in `title_written_like_this` in the file name.                                                                                                                                                                                                                                                                                                                                |
| `replaceFrom`         | All                              |                               | Use a regular expression to replace a matching string in the variable with a value specified by the `replaceTo` parameter. For example, `{{ title replaceFrom="problem" replaceTo="solution" }}` is substituted with the parent item's title where the first occurrence of the word "problem" is replaced with the word "solution". It can be further configured with `regexOpts` parameter.                                                                                                                                                                                                    |
| `replaceTo`           | All                              |                               | Defines a replacement value to use when matching using regular expressions (see `replaceFrom`). It is possible to specify capture groups defined in `replaceFrom`. For example, to prefix all occurrences of the words "dog" and "cat" with the word "super-" in the item's title, the following template can be used: `{{ title replaceFrom="(dog|cat)" replaceTo="super-$1" regexOpts="gi" }}`.                                                                                                                                                                                               |
| `regexOpts`           | All                              | 'i'                           | Defines [flags](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions#advanced_searching_with_flags) to use when matching using regular expressions (see `replaceFrom`). For example, `{{ title replaceFrom="\s+" regexOpts="g" }}` is substituted with the parent item's title with all white space removed (without `regexOpts`, only the first white space would be removed).                                                                                                                                                                                    |
| `match`               | All                              |                               | Use a regular expression to test for a matching string in the variable. This parameter is useful in conditions and it cannot be used with any other parameters, except for `regexOpts`. For example, the following template will only return the parent item's URL if the URL's domain name is zotero.org: `{{ if {{ url match="^https?://zotero.org.*?$" }} }}{{ url }}{{ endif }}`.                                                                                                                                                                                                         |
| `max`                 | `authors`, `editors`, `creators` |                               | Limits number of creators to use, e.g., `{{ editors max="1" }}` will be replaced with first editor of parent's item.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `name`                | `authors`, `editors`, `creators` | `family`                      | Customizes how creator name appears in the filename, with the following accepted options: `family-given` will use full name of the creator, beginning with family (last) name of the creator, `given-family` also uses full name but inverts the order, and options `given` and `family` will only use part of the parent item's creator's name.                                                                                                                                                                                                                                                 |
| `name-part-separator` | `authors`, `editors`, `creators` | ` `(single space character) | Defines what characters to use to separate given and family name, especially useful when combined with `initialize`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `join`                | `authors`, `editors`, `creators` | `, `                        | Defines what characters to use to separate consecutive creators.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `initialize`          | `authors`, `editors`, `creators` |                               | Enables use of initials for part or whole of creators name, with the following accepted options: `full` will use initials for the entire name, `given` and `family` will only use initials for that part of the name. Order of name parts is controlled by `name` parameter and only parts included by `name` parameter can be converted to initials. E.g. `{{ authors name="given-family" initialize="given" }}` will be replaced by a comma-separated list of authors, where each author's given name is replaced with an initial, followed by a dot and a space (e.g. `J. Smith, D. Jones`). |
| `initialize-with`     | `authors`, `editors`, `creators` | `.`                           | Controls what character is appended to the initial, if name part was initialized.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `localize`            | `itemType`                       |                               | Whether to use localized value of the item type variable, e.g., `{{ itemType localize="true" }}` will be replaced by parent item's type spelled in the language Zotero is using.                                                                                                                                                                                                                                                                                                                                                                                                                |
| `timeZone`            | `accessDate`                     |                               | Converts the date to the specified time zone, e.g. `{{ accessDate timeZone="America/New_York" }}` will be replaced by the parent item’s Access Date, converted to local time in New York. [List of time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones). *(Zotero 8)*                                                                                                                                                                                                                                                                                                      |

### Examples

A year of publication, followed by a hyphen-separated list of authors, followed by a title truncated at 30 characters:

Template:

    {{ year suffix="-" }}
    {{ authors name="family-given" initialize="given" initialize="-" join="-" suffix="-" case="hyphen" }}
    {{ title truncate="30" case="hyphen" }}

Filename: `2023-lee-sukbae-kim-ji-hoon-kwon-young-wan-the-first-room-temperature-amb.pdf`

Anything not included inside a `{{` bracket is copied to the filename literally:

Template:

    {{ itemType localize="true" }} from {{ year }} by {{ authors max="1" name="given-family" initialize="given" }}

Filename: `Preprint from 2023 by S. Lee.pdf`

Templates also support conditionals. Certain part of the template can be included or excluded using a combination of `if`, `elseif`, and `else`. The condition must end with `endif`. The template below will use the DOI for journal articles and preprints, the ISBN for books, and the title for any other item type:

    {{ if itemType == "book" }}
    {{ISBN}}
    {{ elseif itemType == "preprint" }}
    {{ DOI }}
    {{ elseif itemType == "journalArticle" }}
    {{ DOI }}
    {{ else }}
    {{ title }}
    {{ endif }}

As of Zotero 7.0.16, it's possible to compare numeric values like `authorsCount` using relational operators such as `<`, `<=`, `>`, and `>=`. For example, the following template checks the number of authors: if there are two or more, it uses the first author's name followed by et al.; if there are one or two authors, it includes all their names in the filename.

    {{ if {{ authorsCount > 2 }} }}
    {{ authors max="1" suffix=" et al" }}
    {{ else }}
    {{ authors join=" & " }}
    {{ endif }}

It's possible to use regular expressions to match values and change the behavior of the template. For example, the following template preserves common attachment names (such as "Full Text"), but for attachments with non-matching titles, it uses the standard Zotero filename template:

    {{ if {{ attachmentTitle match="^(full.*|submitted.*|accepted.*)$" }} }}
    {{ attachmentTitle }}
    {{ else }}
    {{ firstCreator suffix=" - " }}{{ year suffix=" - " }}{{ title truncate="100" }}
    {{ endif }}

As of Zotero 8, it's possible to include the access date and time of an item, converted to a local time in a specified time zone. Since ":" is not allowed in file names, we should replace it to avoid it being removed. Example below uses "-" as a replacement character.

    {{ accessDate timeZone="Europe/Berlin" replaceFrom=":" replaceTo="-" regexOpts="g" }}-{{ title truncate="100" }}

### Complete List of Fields

-   `abstractNote`
-   `applicationNumber`
-   `archive`
-   `archiveID`
-   `archiveLocation`
-   `artworkMedium`
-   `artworkSize`
-   `assignee`
-   `audioFileType`
-   `audioRecordingFormat`
-   `billNumber`
-   `blogTitle`
-   `bookTitle`
-   `callNumber`
-   `caseName`
-   `citationKey`
-   `code`
-   `codeNumber`
-   `codePages`
-   `codeVolume`
-   `committee`
-   `company`
-   `conferenceName`
-   `country`
-   `court`
-   `date`
-   `dateDecided`
-   `dateEnacted`
-   `dictionaryTitle`
-   `distributor`
-   `docketNumber`
-   `documentNumber`
-   `DOI`
-   `edition`
-   `encyclopediaTitle`
-   `episodeNumber`
-   `extra`
-   `filingDate`
-   `firstPage`
-   `format`
-   `forumTitle`
-   `genre`
-   `history`
-   `identifier`
-   `institution`
-   `interviewMedium`
-   `ISBN`
-   `ISSN`
-   `issue`
-   `issueDate`
-   `issuingAuthority`
-   `journalAbbreviation`
-   `label`
-   `language`
-   `legalStatus`
-   `legislativeBody`
-   `letterType`
-   `libraryCatalog`
-   `manuscriptType`
-   `mapType`
-   `meetingName`
-   `nameOfAct`
-   `network`
-   `numPages`
-   `number`
-   `numberOfVolumes`
-   `organization`
-   `pages`
-   `patentNumber`
-   `place`
-   `postType`
-   `presentationType`
-   `priorityNumbers`
-   `proceedingsTitle`
-   `programTitle`
-   `programmingLanguage`
-   `publicLawNumber`
-   `publicationTitle`
-   `publisher`
-   `references`
-   `reportNumber`
-   `reportType`
-   `reporter`
-   `reporterVolume`
-   `repository`
-   `repositoryLocation`
-   `rights`
-   `runningTime`
-   `scale`
-   `section`
-   `series`
-   `seriesNumber`
-   `seriesText`
-   `seriesTitle`
-   `session`
-   `shortTitle`
-   `status`
-   `studio`
-   `subject`
-   `system`
-   `thesisType`
-   `title`
-   `type`
-   `university`
-   `url`
-   `versionNumber`
-   `videoRecordingFormat`
-   `volume`
-   `websiteTitle`
-   `websiteType`
