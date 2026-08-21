# Note Templates

## Annotations

You can use note templates to customize how PDF annotations are [added to notes](pdf_reader#adding_annotations_to_notes). To view the available templates, go to the Advanced pane of the Zotero preferences, open the Config Editor, and search for `annotations.noteTemplates`. There are currently three available templates: for highlight annotations, for note annotations, and for the title of notes created from all of an item's annotation.

Templates support basic HTML, with variables within curly brackets. Here's the default template for highlights:

    <p>{{highlight}} {{citation}} {{comment}}</p>

You can see that, by default, highlight annotations are added as a single paragraph, with the highlighted text followed by the citation and any comment. Quotation marks are automatically added around the highlight.

If you prefer to have the highlight text in a blockquote, it's a simple change:

    <blockquote>{{highlight}}</blockquote><p>{{citation}} {{comment}}</p>

### Conditionals

Templates also support conditionals. Rather than combining the citation and comment in a single paragraph as in the previous example, you might want to create a separate paragraph for the comment, but only if a comment actually exists. You can test whether a variable is set with a simple `if`:

    <blockquote>{{highlight}}</blockquote><p>{{citation}}</p>{{:if comment}}<p>{{comment}}</p>{{:endif}}

Conditionals can also be used to test for specific values. Here, text highlighted in red becomes a header without quotation marks, text highlighted in blue becomes a blockquote, and all other highlights use a single paragraph:

    {{:if color == '#ff6666'}}
        <h2>{{highlight quotes='false'}}</h2>
    {{:elseif color == '#2ea8e5'}}
        {{:if comment}}<p>{{comment}}:</p>{{:endif}}<blockquote>{{highlight}}</blockquote><p>{{citation}}</p>
    {{:else}}
        <p>{{highlight}} {{citation}} {{comment}}{{:if tags}} #{{tags join=' #'}}{{:endif}}</p>
    {{:endif}}

### Variables

Here are the available variables and their supported parameters:

-   `highlight`
    -   `quotes`
        -   omitted: Include quotation marks around text unless the highlight is placed within a blockquote
        -   "true": Always include quotation marks
        -   "false": Never include quotation marks. The highlight must be placed within a blockquote to remain as an active annotation.
-   `citation`
-   `comment`
-   `color` — yellow: '#ffd400', red: '#ff6666', green: '#5fb236', blue: '#2ea8e5', purple: '#a28ae5', magenta: '#e56eee', orange: '#f19837', gray: '#aaaaaa'
-   `tags`
    -   `join` — string to use to join tags

(Note that `color` is primarily for use in conditionals. Annotation colors can be [toggled on and off](pdf_reader#displaying_annotation_colors) from the note editor menu. An upcoming version will add a preference for controlling whether colors are shown by default.)
