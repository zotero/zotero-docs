---
tags:
  - kb
  - sync
---

## Why aren't changes I make syncing between multiple devices and/or zotero.org?

*This page covers problems with data syncing. For problems with file syncing, see [Files Not Syncing](kb/files_not_syncing).*

##### 1) Looking in the wrong place

In Zotero libraries, all existing items are shown in either the library root or the trash. Make sure you're not looking in a collection that contains only some of your items. If you don't see the left pane in the Zotero desktop app, click the bar at the left edge of the window to show it, or go to View → Layout and make sure Collections Pane is checked. If you don't see your collections listed in the left-hand pane, make sure the library is expanded by clicking the arrow or plus sign next to the library. If you see a different number of items in a collection, make sure you're not just using a different setting for View → Show Items from Subcollections.

##### 2) The right device hasn't fully synced

If you're sure you're looking in the right place, check the [web library](/mylibrary) on this site to see whether the data in question has [synced](sync). If you don't see the data in the web library, the problem is on the device where the data originated, and any other device is irrelevant. If the data appears in the web library, the problem is solely on the device where you don't see the data. If the problematic device is a computer, check the sync icon in the Zotero app. If it's still spinning, the sync process hasn't yet completed. Hover over the sync icon to see the current status.

##### 3) You've received a sync error

If you see a red error icon to the left of the sync icon (desktop app) or an error at the bottom of the screen (iOS/Android), an error has occurred during the sync. Click the icon or tap the message to view the error. It may give you enough information to fix the problem yourself, or you can post to the [Zotero Forums](/forum) for further assistance. Be sure to include the message you're receiving and a [Report ID](reporting_problems) in your forum post.

##### 4) The library isn't set to sync

In the [Sync pane](preferences/sync) pane of the settings in the Zotero desktop app, click "Choose Libraries…" and make sure that the library you're trying to sync has a checkmark next to it. If not, click in the Sync column to enable syncing for that library. All libraries are set to sync by default.

##### 5) You're syncing with the wrong account.

Check the Sync pane of the app settings to make sure you're syncing with the same account you're using to log in to zotero.org and on each of your devices. If the problem is with a group, make sure the account is a member of that group on [zotero.org](/groups).

##### 6) You're using an outdated version of Zotero

If items appear in your [Zotero web library](https://www.zotero.org/mylibrary) but not in the Zotero desktop app, make sure you’re running a version of Zotero that supports all the data fields used in your library. Newer versions of Zotero may introduce fields that older versions cannot recognize. When an older version encounters an unsupported field, it may be unable to download the affected item, and you may see a sync warning such as “Some data in your Zotero library could not be downloaded. It may have been saved with a newer version of Zotero.” Other items may continue to sync, leaving your desktop library incomplete.

Check your Zotero version under Help → About Zotero (or Zotero → About Zotero on macOS). If it’s outdated, install the [latest version of Zotero](https://www.zotero.org/download/), then restart the app and sync again. Also make sure your plugins are up to date and compatible with your version of Zotero. Updating Zotero should allow it to recognize newer data fields and download the affected items.

##### 7) Further troubleshooting

If you're still having trouble, post to the Zotero Forums (in your existing thread, if you have one) with a [Debug ID](debug_output#debug_output_logging) for the first sync after making a change that doesn't sync (e.g., making a change to an item in the online library that doesn't appear in Zotero, or vice versa), along with a description of what's not transferring, including the zotero.org URL after selecting the item in the online library if it appears there. Start debug output logging before making the change and keep it going until the sync has ended.


