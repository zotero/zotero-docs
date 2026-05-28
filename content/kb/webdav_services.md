---
tags:
  - kb
  - sync
---

# List of WebDAV services

This page contains a list of WebDAV services which provide a free plan and which users have reported success using with Zotero. Free plans may have some limitations. All service providers also offer larger and less limited paid plans. The list is not exhaustive. Service providers not listed here may still work with Zotero.

**This list is user-generated and does not entail an endorsement of any service by Zotero. Zotero works with correctly-specified WebDAV servers and can provide only minimal support for problems with 3rd party WebDAV providers.**

<table>
  <thead>
    <tr><th>Service</th><th>Free space</th><th>WebDAV URL</th><th>Notes and Limitations</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="http://www.4shared.com/features.jsp">4shared</a></td><td>15 GB</td><td>https://webdav.4shared.com/zotero</td><td>(unofficial) Maximum file size 2 GB. <a href="/forum/discussion/4559/2/which-webdav-service-for-zotero/#Item_23">Maximum 5,000 files per folder (≈2,500 Zotero attachments)</a>.</td></tr>
    <tr><td><a href="https://www.cloudme.com/en/pricing">CloudMe</a></td><td>3 GB</td><td>https://webdav.cloudme.com/{username}/xios/zotero</td><td>Maximum file size 150 MB. <a href="/forum/discussion/75169/webdav-error-for-a-delete-request-with-cloudme-id-1840279011">Users have reported sync errors with CloudMe WebDAV</a></td></tr>
    <tr><td><a href="https://www.driveonweb.de/fuer-privatanwender/produktbeschreibung">DriveOnWeb</a></td><td>3 GB</td><td>https://storage.driveonweb.de/probdav/zotero</td><td>Documentation is in German.</td></tr>
    <tr><td><a href="https://drive.google.com">Google Drive</a></td><td>3 GB</td><td>Requires setting up a personal <a href="https://github.com/mikea/gdrive-webdav">WebDAV bridge</a> or using a third-party service to provide WebDAV access to Google Drive, such as <a href="https://dav-pocket.appspot.com/webdav_access_to_google_docs">DAV-Pocket</a> (<a href="/forum/discussion/68527/dav-pocket-and-google-drive">may no longer be working</a>). WebDAV URLs vary by service. Some services will not work on all operating systems.</td><td><a href="/support/sync#alternative_syncing_solutions">Consider using linked files with Zotfile instead of WebDAV syncing with Google Drive</a></td></tr>
    <tr><td><a href="https://www.free-hidrive.com/product/hidrive-free.html">HiDrive</a></td><td>5 GB</td><td colspan="2">https://webdav.hidrive.strato.com/users/{username}/zotero</td></tr>
    <tr><td><a href="https://www.idrivesync.com/pricing">iDriveSync</a></td><td>10 GB</td><td>https://dav.idrivesync.com/zotero</td><td><a href="/forum/discussion/26858/webdav-syncing-with-idrivesync-is-working/">Some users have had difficulties with this service</a> and their documentation <a href="https://www.idrivesync.com/webdav">recommends against extensive use</a>.</td></tr>
    <tr><td><a href="https://koofr.eu/blog/posts/koofr-with-zotero-via-webdav">Koofr</a></td><td>10 GB</td><td>https://app.koofr.net/dav/Koofr/zotero</td><td>See setup instructions <a href="https://koofr.eu/blog/posts/koofr-with-zotero-via-webdav">here</a>.</td></tr>
    <tr><td><a href="https://www.drivehq.com/">DriveHQ</a></td><td>5 GB</td><td>https://webdav.drivehq.com/zotero</td><td></td></tr>
    <tr><td><a href="http://www.storegate.com/en/home-user-usd/plans/">Storegate</a></td><td>2 GB</td><td colspan="2">https://webdav1.storegate.com/{username}/home/{username}/zotero</td></tr>
    <tr><td><a href="https://disk.yandex.ru">Yandex Disk</a></td><td>10 GB</td><td>https://webdav.yandex.ru/zotero</td><td>Documentation is in Russian.</td></tr>
  </tbody>
</table>

Note: The "/zotero" part at the end of the WebDAV URL is added automatically by Zotero and should not be included when entering the WebDAV URL into Zotero preferences.


