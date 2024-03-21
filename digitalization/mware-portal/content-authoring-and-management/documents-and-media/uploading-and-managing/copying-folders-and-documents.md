# Copying Folders and Documents

With Documents and Media, you can copy folders, files, and shortcuts between sites and connected asset libraries. This can help accelerate projects by quickly duplicating structures across contexts.

Before copying your content, ensure the destination site or asset library has access to the copied files’ tags, categories, or custom document types. Otherwise a file’s tags and categories are removed, and MWARE Portal uses the Basic Document type for it.

NOTE

File metadata and related entities (e.g., comments, subscriptions) are not copied. To copy documents with their related entities, you must use the site’s export/import feature.

Copying content within Documents and Media requires specific permissions:

* View permissions in the origin site or asset library
* Add permissions in the destination

See Documents and Media Permissions Reference for more information.

### Copying Between Sites and Asset Libraries <a href="#copying-between-sites-and-asset-libraries" id="copying-between-sites-and-asset-libraries"></a>

1. Navigate to the desired folder in the Documents and Media application.
2.  To copy a single folder, file, or shortcut, click its _Actions_ (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Copy To_.

    ![Copy one folder or document.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/copying-folders-and-documents/images/01.png)

    To copy in bulk, select multiple items and click _Copy To_ in the document management toolbar.

    ![Copy multiple folders and documents.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/copying-folders-and-documents/images/02.png)
3.  Determine a destination for the copied items. This destination can be any site or connected asset library that you can access.

    ![Determine a destination for the copied items.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/copying-folders-and-documents/images/03.png)
4. Click _Copy_ to execute the operation.

### Setting Copy Size Limit <a href="#setting-copy-size-limit" id="setting-copy-size-limit"></a>

Depending on the size of the files, copying can be resource intensive. You can reduce the impact on system resources by setting a size limit for copy operations.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _System Settings_ or _Instance Settings_.
2. Go to _Documents and Media_ → _File Size Limits_.
3.  Enter a size limit for copy operations. The default value is `52428800` bytes.

    ![Enter a size limit for copy operations in bytes.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/copying-folders-and-documents/images/04.png)
4. Click _Save_.

Now copy operations cannot exceed the set limit.

\
