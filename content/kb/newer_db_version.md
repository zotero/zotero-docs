---
tags:
  - kb
  - basics
---

#### Why am I getting a database version error?

After reinstalling or downgrading Zotero, you may see the following error message when you you try to open Zotero:

*This version of Zotero is older than the version last used with your database. Please upgrade to the latest version from zotero.org.*

This message results from trying to use a Zotero database from a later version of Zotero in an earlier version of the software. For example, if you installed Zotero 10 but then reinstalled Zotero 9, you would get this error. Most Zotero versions preserve backward database compatibility, but occasionally it's necessary for Zotero developers to make changes to the database that prevent it from working with previous versions.

The best solution is generally to reinstall the latest version of Zotero from [zotero.org](/). (If you were using the Zotero Beta, you may need to reinstall the [latest beta build](beta_builds).)

If you've fully synced your data and files with your online account, you can keep your current Zotero version by closing Zotero, deleting the zotero.sqlite, zotero.sqlite-wal, and the accompanying .bak files from the root of your [Zotero data directory](zotero_data) completely, and then starting Zotero and syncing to restore your previous data.
