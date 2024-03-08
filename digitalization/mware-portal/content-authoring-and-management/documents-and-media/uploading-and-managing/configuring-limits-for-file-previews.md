# Configuring Limits for File Previews

Users can preview certain documents (e.g. PDF or text files) in the Documents and Media application; however, previews can take a large amount of storage, if you have many or large documents. In some cases, you might want to limit the number of pages a user can preview. You can save a significant amount of storage by applying a limit to the number of previewed pages.

NOTE

When you change the file preview limit, it affects only new files.

To configure the file preview and limit the number of previewed pages,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and select the _Control Panel_ tab.
2.  Select _System Settings_ or _Instance Settings_.

    You can apply global rules at the system level and apply more restrictive configurations at an instance level.

    For example, you can limit the maximum number of previewed pages to 10 at the system level, but you can set a value of 10 or fewer pages at an instance level.
3. Click _Documents and Media_ under Content and Data.

![Configure the file preview limits through system or instance settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/configuring-limits-for-file-previews/images/01.png)

1. On the left menu, select _File Preview Limits_ (1).
2.  Set the maximum file size for generating previews. No preview is generated for files larger than the specified value.

    The size value is in bytes, and the default maximum size is 20 megabytes.

    Use `-1` to enable previews to all files.

    Use `0` to disable previews to all files.
3.  Set the number of pages shown in your preview.

    Use `0` to show all pages.

When file preview limits are set, a message appears during document preview indicating that the preview may not show all pages.

![While previewing a file, users will see a message indicating that the preview may not show all pages.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/configuring-limits-for-file-previews/images/02.png)
