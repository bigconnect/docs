# Exporting Assets List To a Spreadsheet

The Content Dashboard can export a spreadsheet (`.xls` file) of content related metadata for further analysis.

To export the asset list to a spreadsheet follow these steps:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Applications_ → _Content Dashboard_.
2. Click on _Export XLS_ to download the asset list.

![Click the Export XLS button to begin the export process.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/exporting-the-assets-list-to-a-spreadsheet/images/01.png)

While generating the file, _Export XLS_ is replaced with a status and a cancel button for the export process. If you click _Cancel Export_, the export process stops and shows a message: _XLS generation was canceled_.

When the export process is complete, the status shows the message: _XLS Generated_.

![Once the export process begins, it can be cancelled by clicking Cancel Export.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/exporting-the-assets-list-to-a-spreadsheet/images/02.png)

NOTE

Navigating away from the page while the `.xls` file is being generated stops the export process. The system prompts the user before leaving the page.

![A prompt appears to confirm navigating away from the page while an export is being processed.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/exporting-the-assets-list-to-a-spreadsheet/images/03.png)

### [XLS File Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/exporting-the-assets-list-to-a-spreadsheet#xls-file-content) <a href="#xls-file-content" id="xls-file-content"></a>

All metadata is exported from contents and documents, so the `.xls` file contains the following columns:

* Title
* Author
* Type
* Subtype
* Site or Asset Library
* Status
* Categories
* Tags
* Modified Date
* Review Date
* Description
* Extension
* File Name
* Size
* Display Date
* Creation Date
* Languages Translated Into

NOTE

The available metadata present in the `.xls` file affects content types. For example, an image will not display anything for _Languages Translated Into_, as an image cannot be translated.
