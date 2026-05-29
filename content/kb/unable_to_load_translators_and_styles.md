# “Zotero was unable to load translators and styles.”

If you receive the above error at startup, Zotero wasn't able to load or update site translators or citation styles. Functionality that depends on these files, such as word processor integration, will likely not work until the problem is resolved.

Try resetting translators and styles and checking your database integrity from the Advanced → Files and Folders pane of the Zotero preferences. If that doesn't help, delete the `styles` and `translators` directories in your [Zotero Data Directory](zotero_data) and restart Zotero. If you continue to have trouble, check file permissions in your Zotero data directory.

On Windows, the problem might appear if Zotero fails to delete temporary files. These are located under:
C:\Users\[Your Username]\AppData\Local\Temp\Zotero
Try deleting this directory manually and restarting Zotero.

If you're still having trouble, [report the error](reporting_problems) and post a Report ID to the Zotero Forums.
