---
tags:
  - kb
  - styles
---

### How do I use rich text formatting, like italics and sub/superscript, in titles?

You can apply rich text formatting by manually adding the following HTML-like tags to fields in your Zotero library:

-   `<i>` and `</i>` for *italics*
-   `<b>` and `</b>` for **bold**
-   `<sub>` and </sub> for <sub>subscript</sub>
-   `<sup>` and `</sup>` for <sup>superscript</sup>
-   `<span style="font-variant:small-caps;">` and `</span>` for <span style="font-variant: small-caps;">smallcaps</span>
-   `<span class="nocase">` and `</span>` to suppress capitalization rules (e.g., for foreign phrases within English titles)

Zotero will automatically replace these tags by the specified formatting in bibliographic output. E.g. "<i>Pseudomonas aureofaciens</i> nov. spec. and its pigments" will become "*Pseudomonas aureofaciens* nov. spec. and its pigments".

Note that if rich text formatting has to be applied indiscriminately to entire fields (e.g. a style guide may dictate that titles should be in italics), you can modify the relevant Citation Style Language (CSL) style (see the [CSL documentation](dev/citation_styles) and the [CSL field formatting options](http://citationstyles.org/downloads/specification.html#formatting)).

A future version of Zotero will allow visual rich-text editing without manually adding HTML tags.


