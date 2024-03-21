# Uploading Files

With Documents and Media, you can upload any type of file to your portal instance’s file store or connected repositories. Once uploaded, Users with the requisite permissions can view, edit, download, or share uploaded files.

NOTE

Users can configure the portal to tag supported assets automatically when uploaded. See Configuring Asset Auto Tagging to learn more.

IMPORTANT

You can enable automatic antivirus scanning to scan files on upload. For details, please see Enabling Antivirus Scanning for Uploaded Files.

In order to upload files, first navigate to the Documents and Media application in a Site or Asset Library.

To access Documents and Media in a Site, open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Content & Data_ → _Documents and Media_.

![Click on Documents and Media under Content & Data in the Site Menu.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/uploading-files/images/01.png)

To access Documents and Media in an Asset Library, navigate to any Library where it’s enabled and click on _Documents and Media_.

![Click on Documents and Media in the Asset Library.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/uploading-files/images/02.png)

Once you’ve opened the application, the quickest way to upload files is to drag and drop them into the desired folder. This immediately begins uploading your files without prompting you to configure your upload before they’re published. Once a file finishes uploading, you can edit its details by clicking its _Actions_ button and selecting _Edit_.

![Drag and drop any number of files into the desired folder.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/uploading-files/images/03.png)

However, if you would like to define file details before uploading them, follow these steps:

1.  Click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select either _File Upload_, or _Multiple Files Upload_.

    ![Select either File Upload, or Multiple Files Upload.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/uploading-files/images/04.png)
2.  Drag and drop files into the designated drop-zone or use the file selector to browse for your files.

    If you’ve selected _File Upload_, the upload screen appears as follows.

    ![Select a single file to upload.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/uploading-files/images/05.png)

    If you’ve selected _Multiple Files Upload_, the upload screen appears as follows.

    ![Drag and drop or select multiple files to upload.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/uploading-files/images/06.png)
3. Configure file details before upload. See Configuring File Upload Reference for more details.
4. When finished, click _Publish_ to begin the upload process.

### Configuring File Upload Reference <a href="#configuring-file-upload-reference" id="configuring-file-upload-reference"></a>

| Field           | For Single File Upload | For Multiple File Upload | Description                                                                                                                                                                                                                                                            |
| --------------- | ---------------------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| File/Files      | ✔                      | ✔                        | Select the file/files for upload. By default, individual file uploads cannot exceed 100MB.                                                                                                                                                                             |
| Title           | ✔                      |                          | Set a display title used for the file after upload. By default this field is populated with the selected file’s original name, but you can change it during or after upload.                                                                                           |
| File Name       | ✔                      |                          | Set a new name for the uploaded file. By default this field is populated with the selected file’s original name, but you can change it during or after upload.                                                                                                         |
| Description     | ✔                      | ✔                        | Add a file description for your uploaded file/files.                                                                                                                                                                                                                   |
| Display Page    | ✔                      | ✔                        | Select a Display Page Template for the uploaded file/files.                                                                                                                                                                                                            |
| Categorization  | ✔                      | ✔                        | Apply tags and public or internal Categories to the uploaded file/files.                                                                                                                                                                                               |
| Expiration Date | ✔                      | ✔                        | Determine if and when the file/files expire or should be reviewed. By default, files are set to never expire or require review. If enabled, you the date when the file expires or should be reviewed. See Using File Expiration and Review Dates for more information. |
| Related Assets  | ✔                      |                          | Select related assets or the file from the Site or connected Asset Library.                                                                                                                                                                                            |
| Permissions     | ✔                      | ✔                        | Set permissions for the uploaded file/files. These settings determine which user roles can view the file and perform other actions related to it (e.g., update, delete).                                                                                               |
