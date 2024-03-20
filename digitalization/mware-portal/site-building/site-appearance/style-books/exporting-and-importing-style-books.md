# Exporting and Importing Style Books

You can export and import style books to share them between the portal installations as ZIP files or as Archive (LAR) files.

### Exporting a Style Book as a ZIP File <a href="#exporting-a-style-book-as-a-zip-file" id="exporting-a-style-book-as-a-zip-file"></a>

1. Open the _Site Menu_ ( ![Site Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Design_ → _Style Books_.
2. Find the style book you want to export and click _Actions_ ( ![Actions Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) → _Export_.
3.  A ZIP file is downloaded to your machine. It contains two files:

    `style-book.json` contains the name of the style book and the path to `frontend-token-values.json`.

    `frontend-token-values.json` contains a list of the token values of the style book. Each token references the CSS variable name and the value mapped to it.

You can now share and import your ZIP file.

### Importing Style Books from a ZIP File <a href="#importing-style-books-from-a-zip-file" id="importing-style-books-from-a-zip-file"></a>

1. Open the _Site Menu_ ( ![Site Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Design_ → _Style Books_.
2. On the Applications bar, click _Options_ ( ![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png) ) → _Import_.
3.  Choose the ZIP file you want to import.

    Check _Overwrite Existing Style Books_ to overwrite existing style books with the same name. With the option unchecked, importing a style book with the same name as an existing one renders an error message.
4. Click _Import_.

You can now use your imported style guide.

### Exporting Style Books as a LAR File <a href="#exporting-style-books-as-a-lar-file" id="exporting-style-books-as-a-lar-file"></a>

1. Open the _Site Menu_ ( ![Site Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Design_ → _Style Books_ .
2.  Select the style book you want to export. On the Applications bar, click _Options_ ( ![Options Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png) ) → _Export / Import_.

    ![To export a Style Book, go to options in the Style Books app](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/exporting-and-importing-style-books/images/01.png)
3. In the Export / Import modal, select _Export_.
4.  Start a _New Export Process_.

    NOTE

    You can select the _Current and Previous_ tab to view a list of previous processes and download exported LAR files.
5.  When starting a new export process, you can name the LAR file and configure the process.

    ![You can choose to Export or Import a style book and start a new process or check current and previous processes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/exporting-and-importing-style-books/images/02.png)
6. Click _Export_ to begin the export process.

You can now share and import your LAR file.

### Importing Style Books as a LAR File <a href="#importing-style-books-as-a-lar-file" id="importing-style-books-as-a-lar-file"></a>

1. Open the _Site Menu_ ( ![Site Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Design_ → _Style Books_.
2. Select the style book you want to import. On the Applications bar, click _Options_ ( ![Options Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png) ) → _Export / Import_.
3. In the Export / Import modal, select _Import_.
4.  Start a _New Import Process_.

    NOTE

    You can select the _Current and Previous_ tab to view a list of previous processes.
5. Upload the LAR file and click _Continue_.
6. Configure the process.
7. Click _Import_ to begin the import process.

You can now use your imported style book.

### Configuring Export and Import Processes as a LAR File <a href="#configuring-export-and-import-processes-as-a-lar-file" id="configuring-export-and-import-processes-as-a-lar-file"></a>

MWARE Portal provides settings for configuring export and import processes.

| Setting                                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Application                                       | When checked, your style book’s configuration settings are exported/imported to the LAR file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Content                                           | Select a date range of content that you want to export. Content added to your style book within your specified date range is included in the LAR file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Deletions                                         | When checked, the delete operations performed for content types are exported/imported to the LAR file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Delete Application Data Before Importing (Import) | When checked, all data created by the application is deleted before the import process. Be careful: other applications may reference this data. This process cannot be undone. If you are unsure, do an export first.                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Permissions                                       | When checked, permissions assigned for your exported/imported entries are included.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Update Data (Import)                              | <p>Choose one of these options to define what happens to the data once it is updated:<br><strong>Mirror</strong>: All data and content inside the imported LAR is newly created the first time while maintaining a reference to the source. Subsequent imports from the same source updates entries instead of creating new entries.<br><strong>Mirror with Overwriting</strong>: Same as mirroring, but if a document or an image with the same name is found, it is overwritten.<br><strong>Copy as New</strong>: All data and content inside the imported LAR is created nas new entries with the current site every time the LAR is imported.</p> |
| Authorship of the Content (Import)                | <p>Choose one of these options to define the authorship of the content:<br><strong>Use the Original Author</strong>: Keep authorship of imported content whenever possible (this option uses the current user as author if the original one is not found).<br><strong>Use the Current User as Author</strong>: Assign the current user as the author of all imported content.</p>                                                                                                                                                                                                                                                                     |

![Configure your export/import process](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/exporting-and-importing-style-books/images/03.png)
