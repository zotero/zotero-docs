# The Zotero Data Directory

## Locating Your Zotero Data

The easiest and most reliable way to find your Zotero data is by clicking the "Show Data Directory" button in the [Advanced](preferences/advanced) tab of the [Zotero settings](preferences). This will reveal the folder on your computer that contains your Zotero database and attachment files.

### Default Locations

Unless you have selected a custom data directory in the [Advanced](preferences/advanced) tab of the Zotero settings, your Zotero data is stored within the following OS-dependent directories:

<table>
  <tbody>
    <tr><td><strong>macOS</strong></td><td><code>/Users/&lt;username&gt;/Zotero</code></td></tr>
    <tr><td><strong>Windows 7 and higher</strong></td><td><code>C:\Users\&lt;User Name&gt;\Zotero</code></td></tr>
    <tr><td><strong>Windows XP/2000</strong></td><td><code>C:\Documents and Settings\&lt;username&gt;\Zotero</code></td></tr>
    <tr><td><strong>Linux</strong></td><td><code>~/Zotero</code></td></tr>
  </tbody>
</table>

The "Show Data Directory" button will always reveal the data directory currently in use and is the recommended method for finding your data directory. If you're unable to access the Zotero settings, a search for the file name 'zotero.sqlite' can also help you locate the Zotero data directory.

<details>
<summary><strong>Older Versions</strong></summary>

<h5>Zotero 4 for Firefox (2017 and earlier)</h5>

<table>
  <tbody>
    <tr><td><strong>macOS</strong></td><td><code>/Users/&lt;username&gt;/Library/Application Support/Firefox/Profiles/&lt;randomstring&gt;/zotero</code><br><small>Note: The /Users/&lt;username&gt;/Library folder is hidden by default. To access it, click on your desktop, hold down the Option key, and click the Finder's Go menu, and then select Library from the menu.</small></td></tr>
    <tr><td><strong>Windows 7 and higher</strong></td><td><code>C:\Users\&lt;User Name&gt;\AppData\Roaming\Mozilla\Firefox\Profiles\&lt;randomstring&gt;\zotero</code></td></tr>
    <tr><td><strong>Windows XP/2000</strong></td><td><code>C:\Documents and Settings\&lt;username&gt;\Application Data\Mozilla\Firefox\Profiles\&lt;randomstring&gt;\zotero</code></td></tr>
    <tr><td><strong>Linux (most distributions)</strong></td><td><code>~/.mozilla/firefox/Profiles/&lt;randomstring&gt;/zotero</code></td></tr>
  </tbody>
</table>

<h5>Zotero 4 Standalone (2017 and earlier)</h5>

<table>
  <tbody>
    <tr><td><strong>macOS</strong></td><td><code>/Users/&lt;username&gt;/Library/Application Support/Zotero/Profiles/&lt;randomstring&gt;/zotero</code><br><small>Note: The /Users/&lt;username&gt;/Library folder is hidden by default. To access it, click on your desktop, hold down the Option key, and click the Finder's Go menu, and then select Library from the menu.</small></td></tr>
    <tr><td><strong>Windows 7 and higher</strong></td><td><code>C:\Users\&lt;User Name&gt;\AppData\Roaming\Zotero\Zotero\Profiles\&lt;randomstring&gt;\zotero</code></td></tr>
    <tr><td><strong>Windows XP/2000</strong></td><td><code>C:\Documents and Settings\&lt;username&gt;\Application Data\Zotero\Profiles\&lt;randomstring&gt;\zotero</code></td></tr>
    <tr><td><strong>Linux (most distributions)</strong></td><td><code>~/.zotero/Profiles/&lt;randomstring&gt;/zotero</code></td></tr>
  </tbody>
</table>

</details>

## Data Directory Contents

The most important file in the data directory is the `zotero.sqlite` file, which is the database containing the majority of your data: item metadata, notes, tags, etc. When Zotero starts up, it reads the `zotero.sqlite` file in the active data directory.

The directory also contains a `storage` folder with 8-character subfolders (e.g., `N7SMB24A`) containing all of your file attachments, such as PDFs, web snapshots, audio files, or any other files you have imported. (Files that are [linked](attaching_files) are not copied into this subfolder.)

Your data directory will likely contain several other files and folders:

  - `zotero.sqlite-wal` and `zotero.sqlite-shm` — temporary database files while Zotero is open. `zotero.sqlite-wal` is normally zeroed when Zotero closes. If Zotero doesn't shut down cleanly, `zotero.sqlite-wal` may contain recent changes that Zotero will apply at the next startup, so you shouldn't delete it, and you should keep it with zotero.sqlite when copying the database elsewhere.
  - `zotero.sqlite.bak` and `zotero.sqlite.1.bak` — automatic backups of `zotero.sqlite`, made at most once a day by default. On most Mac systems, backups are cloned files (APFS copy-on-write) that don't take up additional disk space except where the main database has changed.
  - `fulltext.sqlite` — indexes of full-text content (attachments, notes, etc.) for faster searching. This file isn't backed up, because it can be recreated automatically if necessary.
  - Folders such as `locate`, `logs`, `pipes`, `styles`, and `translators` that are created automatically at Zotero startup

**Warning**: Before you copy, delete, or move any of these files, be sure that Zotero is closed. Failure to do so before moving these files can damage your data.

## Backing Up Your Zotero Data

We strongly recommend that you regularly back up your Zotero data directory. While [syncing](sync) is a great way to make sure you can restore your libraries if something happens to your computer, it's not a complete substitute for a proper backup: the Zotero servers only store the most recent version of your libraries, and it takes just a single (possibly automatic) sync to change the server copy (though some inadvertent changes can be restored from [Zotero's automatic backups](#restoring_from_the_last_automatic_backup)).

Rather than backing up just your Zotero database, we recommend using a backup utility that automatically backs up your entire hard drive to an external device on a regular basis and keeps incremental backups so that you can restore to a given version. Most modern operating systems offer such functionality (e.g., Time Machine on Macs).

If you really want to back up your Zotero data specifically, [locate your Zotero data](zotero_data#locating_your_zotero_data), close Zotero, and copy your data directory (the *entire folder*, including `zotero.sqlite` and `storage` and the other subfolders) to a backup location, preferably on another storage device. As with all important data, it's a good idea to back up your Zotero data frequently, which is why we recommend an automated full-system backup instead.

Note that if you're using "download files as needed" for file syncing, your attachment files may not all exist locally and may not be included in a backup. Zotero Storage provides reliable storage of uploaded files, so you might choose to exclude the `storage` folder from your backup, but if you'd like a local backup of attachments as well, you would need to use "download files at sync time" on one computer and make a backup of the data directory from that computer.

**Warning**: You shouldn't use export (e.g., to Zotero RDF, BibTeX, or RIS) as a backup method. Exporting and re-importing a library doesn't produce an exact copy — it will reset Date Added/Modified times and break links to existing citations in word processor documents, along with other potential changes.

## Restoring Your Zotero Data From a Backup

Between manual backups, automatic backups, and synced data, it's often possible to restore a lost Zotero library or restore data that was accidentally deleted.

Before following these steps, be sure that [Zotero is looking in the right place for your data](#where_did_my_items_go).

### Restoring Your Zotero Data Using Zotero Syncing

If you were using Zotero syncing and have an empty local library, you can likely restore your data simply by syncing with your online library. After verifying that your library is correct on zotero.org, simply reenter your username and password in the Sync tab of the Zotero settings and click the Sync button in the toolbar. (Zotero only syncs explicit deletions, so just syncing an empty library won't overwrite the server data **unless you deleted items manually**.)

If you have a local Zotero library that you want to overwrite, close Zotero and delete the old [Zotero data directory](#locating_your_zotero_data) before syncing. Syncing your database with a different Zotero account will also prompt you to remove the existing local database.

### Restoring Your Zotero Data From a Backup

If you were not using Zotero syncing (or were but don't want to perform a full sync) and have a backup of your Zotero data directory, you can restore your library by replacing your active data directory with your backed-up data directory.

Open the Advanced tab of the Zotero settings and make a note of the specified path under Data Directory Location. (By default, this will be "Zotero" within your home folder.) Click "Show Data Directory", which should reveal your active data directory containing zotero.sqlite and possibly a 'storage' subdirectory. Close Zotero, change to the parent folder of the active data directory (Cmd-up-arrow on macOS, Alt-up-arrow on Windows), and rename the folder to "Zotero-Old". Next, copy the data directory from your backup to the original location (e.g., "Zotero").

When you reopen Zotero, you should see your restored Zotero data.

Once you've successfully restored your data, you can delete the "Zotero-Old" folder, but it's a good idea to keep it for a while until you're sure your data is correct.

Note that, if you were using Zotero syncing, any changes you made to your library since the backup and subsequently synced to your online library will be applied to your restored database as soon as you sync. If you don't want that to happen, see the following section.

### Restoring Your Zotero Data From a Backup and Overwriting Synced Changes

If you or someone else made unwanted changes to your Zotero library and synced those changes to your online library, you may be able to restore data by using a local backup of your Zotero data directory.

1.  Temporarily disable auto-sync in the Sync tab of the Zotero settings.
2.  Follow the steps in the preceding section to restore from a backup of your Zotero data directory.
3.  Once you see your restored data, were you to sync again, the more recent data in the online library would replace the data you just restored, and you'll need to take steps to prevent that:
    -   If you're trying to restore a small number of deleted items or notes, you can simply duplicate the items — by right-clicking and choosing "Duplicate Item(s)" — so that the new copies remain even after syncing. You could also make a local change (e.g., adding items to a collection) to trigger a conflict, and then choose the local versions when you sync.
    -   If you're trying to restore deleted collections, you can create duplicate collections and drag items from the old collections to the new ones. When you sync, the old collections will be deleted but the new ones will remain.
    -   If many items were affected or collections were deleted, you can use [Replace Online Library](kb/sync_reset_options#replace_online_library) to force Zotero to upload the local version of the library, overwriting previously synced changes. Note that will delete any other changes you've made locally since the backup.

If you're happy with the results, you can re-enable auto-sync and continue working.

### Restoring From the Last Automatic Backup

If you make a critical mistake while using Zotero — for example, if you accidentally delete a large set of items — you may be able to revert to the last automatic backup. Note that automatic backups contain only data, not files.

1.  If you're using syncing, temporarily disable auto-sync in the Sync tab of the Zotero settings.
2.  [Locate your Zotero data](zotero_data#locating_your_zotero_data) and make a backup copy of any zotero.sqlite.bak files. The timestamps of the files may help you determine which file would contain the data you're trying to restore.
3.  Close Zotero. In your data directory, rename zotero.sqlite to zotero.sqlite.old, rename one of the original .bak files (based on the timestamp) to zotero.sqlite, and restart Zotero. You should now see the backed-up version of your library.
4.  If you were using syncing and the undesired changes were already synced, syncing again now would cause the more recent data in the online library to replace the data you just restored, and you'll need to take steps to prevent that:
    -   If you're trying to restore deleted collections, you can create duplicate collections and drag items from the old collections to the new ones. When you sync, the old collections will be deleted but the new ones will remain.
    -   If you're trying to restore a small number of deleted items or notes, you can simply duplicate the items — by right-clicking and choosing "Duplicate Item(s)" — so that the new copies remain even after syncing.
    -   If many items were affected or collections were deleted, you can use [Replace Online Library](kb/sync_reset_options#replace_online_library) to force Zotero to upload the local version of the library, overwriting previously synced changes.

If you're happy with the results, you can re-enable auto-sync and continue working. Keep zotero.sqlite.old and your .bak file backups until you're sure all your data is intact and in sync across all your computers.

### Restoring From the Last Upgrade Backup

When you upgrade to a major new version of Zotero, Zotero will automatically update your database to work with the new version. If you would like to revert to a previous version of Zotero at a later point, you will have to manually replace your database with the automatic backup Zotero made during the upgrade. In most cases this will be the highest-numbered "zotero.sqlite.[num].bak" file in your Zotero data directory.

It's a good idea to make a backup of your entire Zotero data directory before making any changes.

If you have synced your data with the Zotero servers, reverting to a previous version is as simple as reinstalling the previous version, closing Zotero, replacing "zotero.sqlite" in your Zotero data directory with "zotero.sqlite.[highest-number].bak", and restarting Zotero. (Note that if you try to open an upgraded database in an earlier version, Zotero will display an error. Just close Zotero and replace the .sqlite file.) Zotero will then sync from the online library any changes made since you last used the older database.

If you were not using syncing, you may wish to export to Zotero RDF any items added since the database upgrade and then reimport those into the earlier version. [Sorting](sorting) your library by Date Added may help you find such items.

To temporarily disable further updates, go to the Config Editor in the Advanced tab of the Zotero settings and set `app.update.auto` to false. Note that staying on an old version is not a long-term solution, as old versions are no longer supported and may stop syncing or receiving site-compatibility updates at any time. Be sure to post to the Zotero Forums and explain whatever is causing you to downgrade, and make a note to check back periodically to see whether it makes sense for you to re-enable automatic updates.

## Locating Missing Zotero Data

If you open Zotero to find your library blank or missing lots of data, there are a few main possibilities:

-   If you were using a very old version of Zotero — from 2017 or earlier — without installing any updates and just upgraded to a newer version of Zotero, see [Missing Data After Zotero 5 Upgrade](kb/data_missing_after_zotero_5_upgrade). Zotero 5 was released many years ago, so this no longer applies to most people.
-   If you're using a different computer from the one where you created the missing data, and your data is also missing in your online library, your data simply hasn't synced from the computer where you created it. See [Changes Not Syncing](kb/changes_not_syncing).
-   If you know you've had the data on this computer previously, something may have happened to your previous Zotero database, or Zotero may be looking in the wrong place for your data. Read on for instructions.

To determine what happened to your data on this computer, first locate your current Zotero data directory by going to the [Advanced → Files and Folders](preferences/advanced) section of the Zotero settings and using the "Show Data Directory" button. Take note of the names, sizes, and dates of the files beginning with "zotero.sqlite" in this folder, which are your Zotero database (zotero.sqlite) and automatic database backups (\*.bak). An empty Zotero database will be either approximately 1 MB (~1,000 KB) or 5 MB.

If you see only 1 MB or 5 MB zotero.sqlite files, look in the 'storage' folder, if one exists, for folders with dates corresponding with your previous usage of Zotero.

-   If you see folders in 'storage', this is likely the Zotero data directory you were using previously, but something happened to the zotero.sqlite database outside of Zotero — for example, you might have accidentally deleted zotero.sqlite using system tools while trying to clear disk space. In this case, you can restore your data through backups and/or Zotero syncing:
    1.  Look for larger zotero.sqlite.bak files in the data directory, or look for a larger zotero.sqlite file in any separate backups you have. (It's not possible to restore your data from the 'storage' files alone.) When Zotero starts up, it reads the zotero.sqlite file in the active data directory, so you can try other copies of zotero.sqlite by copying them to that location and filename. Do not try to import an .sqlite file into Zotero via File → "Import…" — it won't work.
    2.  Whether or not you have a backup, if you've been using Zotero syncing, you can sync to pull down all data in your [online library](/mylibrary). If you do have a backup, all data more recent than the backup will be downloaded. If you only have an empty database, all data will be downloaded. In either case, you won't overwrite data in the online library simply by syncing — syncing doesn't work that way.
    3.  If you can't find any other copies of zotero.sqlite and weren't using Zotero syncing, you'll unfortunately probably need to recreate your database from scratch. Close Zotero, move the Zotero data directory to your desktop as "Zotero Old", and restart Zotero to create a new library. You can search for all PDFs within your "Zotero Old" folder and drag them to Zotero, and Zotero will attempt to [retrieve metadata](retrieve_pdf_metadata) for as many of them as possible. You can also extract data from any Word or LibreOffice documents you used with the Zotero word processor plugin by using [Reference Extractor](https://rintze.zelle.me/ref-extractor/), though note that any data you re-import this way won't be linked to your existing documents.
-   If this isn't the location you were expecting to be using, or if you don't see a 'storage' folder or it's empty, you'll need to locate your previous data directory on this computer. Once you find it, either select that data directory from the Zotero settings or, with Zotero closed, rename the current directory (e.g., to "Zotero-Old") and move your desired Zotero directory to the specified location. If you're not sure where your most recent Zotero data is located, look for versions of zotero.sqlite or zotero.sqlite.bak larger than 5 MB with appropriate modification times stored elsewhere on your computer and look at the dates of the folders within the 'storage' folder.
    -   Unless you have a good reason to use a custom data directory location, we strongly recommend using the [default location](#default_locations) in your home directory.
    -   When specifying a custom data directory location, keep in mind that Zotero doesn't move or copy any data. You still need to copy your data into the specified location. Also, when pointing the data directory location to an existing folder, be sure to specify the folder containing zotero.sqlite and 'storage', not the 'storage' folder.

If you've gone through these steps and aren't sure what to do, post to the Zotero Forums with the following info:

-   The names, sizes, and dates of all files beginning with "zotero.sqlite" in your current data directory
-   Whether there's a 'storage' folder containing subfolders with dates corresponding to your previous usage of Zotero
-   Whether your current data directory is in the default location ("Zotero" in your home folder)
-   When you last used Zotero on this computer, and what happened on your computer since then
-   What you've tried so far
