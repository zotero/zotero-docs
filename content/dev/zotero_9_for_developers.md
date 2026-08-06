# Zotero 9 for Developers

Zotero 9 introduced [various new features](https://www.zotero.org/blog/zotero-9/) but did not include any major developer-facing changes.

For previous changes, see [Zotero 8 for Developers](/support/dev/zotero_8_for_developers).

## Updating plugin compatibility

After confirming that your plugin is compatible, update `strict_max_version` in your manifest.json to `9.0.*`. If no changes are required, you can simply update `strict_max_version` in your plugin's update manifest without releasing a new version.
