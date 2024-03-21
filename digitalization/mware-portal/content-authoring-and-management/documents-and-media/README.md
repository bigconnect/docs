# Documents and Media

The Documents and Media application stores and manages files in your Liferay instance. This includes files uploaded directly to Documents and Media, as well as those uploaded in applications that support file attachments.

All uploaded files are stored in your designated file store. By default, Documents and Media uses the Simple File System Store, but you can configure your instance to use an alternate file store (e.g., DBStore, Google Cloud Store, or Amazon S3 Store).

![Use Documents and Media to store, view, and manage files for a Liferay instance.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/images/01.png)

Once uploaded, you can search, filter, and organize digital assets via the Liferay UI or use WebDAV to access your documents via your system’s file manager. You can also integrate Documents and Media with Microsoft Office 365, OpenOffice, Google Drive, and other applications to provide additional functionality in the Liferay UI.

Enable Liferay Workflow for Documents and Media to help manage asset publishing and version updates. When enabled, documents must be reviewed before they are published.

You can configure Documents and Media integrations to automatically generate previews for files when uploaded or updated.

Files added to Documents and Media can be displayed in applications that support attachments (e.g., Web Content, Message Boards), as well as Page Fragments and widgets (e.g., Media Gallery).

If desired, you can export Documents and Media assets as Archive (`LAR`) files. These files can be used as backups or to import your assets into another Site or Asset Library.

TIP

Documents and Media is available in all Sites and can be enabled in Asset Libraries. Files uploaded to a Site are scoped to that Site and cannot be readily accessed by other Sites in your DXP instance. Files uploaded to an Asset Library are available globally and can be linked to multiple Sites for ready access.
