---
tags:
  - kb
  - basics
---

# What do I do if my Zotero database is corrupted?

Zotero stores your information in a database file, zotero.sqlite, in the [Zotero data directory](zotero_data). If the database becomes corrupted, Zotero may no longer be able to start up, or certain operations might fail.

Database corruption generally occurs when the data directory is placed in a cloud storage folder or on a network drive. If you've moved your data directory to one of those places, you should move it back to the default location. You should [never store the data directory in cloud storage](kb/data_directory_in_cloud_storage_folder). (The same applies to any database-backed program.)

## If You Were Using Syncing

If your data is all in your [web library](/mylibrary), you can simply sync to pull down the latest version of your library, either from an empty local database (after moving your corrupted zotero.sqlite out of the way and restarting Zotero) or from an earlier, uncorrupted backup of the database. In the latter case, Zotero will simply pull down changes since the last time you used that database, without needing to redownload your entire library. Once you're sure you've recovered your data, you can delete any copies of the corrupted zotero.sqlite file.

## If You Weren't Using Syncing

If your data directory was in cloud storage or you have a local backup, you can try to restore from an earlier version of zotero.sqlite, including one of the [automatic backups](zotero_data#restoring_from_the_last_automatic_backup) in your Zotero data directory, by closing Zotero, moving the current zotero.sqlite out of the way, and copying the backup file into place as zotero.sqlite. After restoring from a backup and starting Zotero, check the database integrity from the Advanced → Files and Folders pane of the Zotero preferences.

If you don't have a backup, or the backups are corrupted as well, you can try to fix the damage with the [Zotero Database Repair Tool](https://www.zotero.org/utils/dbfix/).


