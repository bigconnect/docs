# Documents and Media

The Documents and Media application stores and manages files in your Liferay instance. This includes files uploaded directly to Documents and Media, as well as those uploaded in applications that support file attachments.

All uploaded files are stored in your designated [file store](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage). By default, Documents and Media uses the [Simple File System Store](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/simple-file-system-store), but you can configure your instance to use an alternate file store (e.g., [DBStore](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/dbstore), [Google Cloud Store](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/google-cloud-storage), or [Amazon S3 Store](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/amazon-s3-store)).

![Use Documents and Media to store, view, and manage files for a Liferay instance.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/images/01.png)

Once uploaded, you can search, filter, and organize digital assets via the Liferay UI or use [WebDAV](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav) to access your documents via your system’s file manager. You can also integrate Documents and Media with [Microsoft Office 365](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/enabling-document-creation-and-editing-with-microsoft-office-365), [OpenOffice](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/enabling-openoffice-libreoffice-integration), [Google Drive](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-links-to-google-drive-documents), and other applications to provide additional functionality in the Liferay UI.

Enable Liferay [Workflow](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow#activating-workflow-for-specific-applications) for Documents and Media to help manage asset publishing and version updates. When enabled, documents must be reviewed before they are published.

You can configure Documents and Media integrations to [automatically generate previews](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/configuring-documents-and-media-previews) for files when uploaded or updated.

Files added to Documents and Media can be displayed in applications that support attachments (e.g., Web Content, Message Boards), as well as Page Fragments and widgets (e.g., [Media Gallery](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/publishing-documents#using-the-media-gallery-widget)).

If desired, you can [export](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content) Documents and Media assets as Liferay Archive (`LAR`) files. These files can be used as backups or to import your assets into another Site or Asset Library.

TIP

Documents and Media is available in all Sites and can be enabled in [Asset Libraries](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries). Files uploaded to a Site are scoped to that Site and cannot be readily accessed by other Sites in your DXP instance. Files uploaded to an Asset Library are available globally and can be linked to multiple Sites for ready access.
