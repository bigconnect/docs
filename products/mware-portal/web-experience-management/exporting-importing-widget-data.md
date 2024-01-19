# Exporting/Importing Widget Data

You may need to export data from a specific widget instance, without regard to content on the rest of the Site. There are many widgets that let you export or import their data individually:

* Blogs
* Bookmarks
* Dynamic Data Lists
* Forms
* Knowledge Base
* Message Boards
* Web Content
* Wiki
* And more

Exporting widget data produces a `.lar` file that you can save and import into another widget of the same type. To import widget data, you must select a `.lar` file. Be careful not to confuse widget-specific `.lar` files with Site-specific `.lar` files. See the Importing/Exporting Pages and Content article for information on importing/exporting Site page data.

There are two ways to export/import widget content. You can navigate to the widget’s administrative area located in the Product Menu, or you can visit the widget on its page. Both export/import menus work the same, but the administrative area may hold content different from its widget counterpart (e.g., Web Content Admin in Product Menu and Web Content Display widget do not offer same content for export/import), so be wary of your selection.

To export or import data from the widget’s administrative area, follow the steps below.

1. Navigate to the widget’s designated area in the Product Menu. For example, if you plan to export Web Content data, navigate to _Content_ → _Web Content_.
2. Click the _Options_ button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030189932/icon-options.png)) from the top right of the page and select _Export/Import_.
3. Select the _Export_ or _Import_ tab to begin configuring the respective process.

![Figure 1: You can access a widgets administrative Export/Import feature by selecting its Options menu.](https://liferay-support.zendesk.com/hc/article\_attachments/360030189992/admin-app-export-import-feature.png)

To export or import data from a widget, follow the steps below:

1. Ensure the widget you’re exporting/importing from is available on a page. You can add widgets from the _Add_ (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030158931/icon-add-app.png)) → _Widgets_ menu.
2. Select the widget’s _Options_ button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030189912/icon-app-options.png)) and select _Export/Import_.
3. Select the _Export_ or _Import_ tab to begin configuring the respective process.

![Figure 2: You can access a widgets Export/Import feature by selecting its Options menu.](https://liferay-support.zendesk.com/hc/article\_attachments/360030189972/widget-export-import-feature.png)

Now that you know how to navigate to the _Export/Import_ menus, you can explore the export process.

### Exporting Widget Data <a href="#exporting-widget-data" id="exporting-widget-data"></a>

To export widget data, create a new export process by selecting the _New Export Process_ tab (default). You have several export options to configure.

First, you can choose to export your widget’s configuration settings. This exports your customized settings from your widget’s _Options_ → _Configuration_ menu. For some widgets, the configuration export might also include content. For example, a Web Content Display widget that shows a web content article also exports the article when exported, even though no content is selected. This applies when publishing a Web Content Display widget too; the configured article is published with the widget.

Next, you can select a _Date Range_ of content that you want to export. Content added to your widget within your specified date range is included in the `.lar` file. The following date range choices are available:

**All:** Publishes all content regardless of its creation or last modification date.

**Date Range:** Publishes content based on a specified date range. You can set a start and end date/time window. The content created or modified within that window of time is published.

**Last…:** Publishes content based on a set amount of time since the current time. For example, you can set the date range to the past 48 hours, starting from the current time.

By checking the _Content_ box, you can choose specific content you want to export. When you check the _Content_ box, more options appear, letting you choose specific kinds of metadata to include. For example, if you have a wiki page with referenced content that you don’t want, check the _Wiki Pages_ checkbox and uncheck the _Referenced Content_ checkbox. Another option is the selection of content types. Two familiar content types in your portal instance are _Comments_ and _Ratings_. If you want to include these entities in your `.lar` file, select _Change_ and select them from the checklist.

Next, you can choose to export individual deletions. This lets delete operations performed for content types be exported to the LAR file.

Finally, you can choose whether to include permissions for your exported content. The permissions assigned for the exported widget window are included if you enable the _Export Permissions_ selector.

After you’ve exported your widget’s data, switch to the _Current and Previous_ tab to view ongoing export processes and the history of past exports. You can also download the exported `.lar` file from this tab.

### Importing Widget Data <a href="#importing-widget-data" id="importing-widget-data"></a>

To import widget data, you can select the LAR using your file explorer or by dragging and dropping the file between the dotted lines.

![Figure 3: When importing widget data, you can choose a LAR file using the file explorer or drag and drop the file between the dotted lines.](https://liferay-support.zendesk.com/hc/article\_attachments/360030189952/import-menu.png)

Your LAR file is uploaded and displayed to you for review. Click _Continue_.

Now that you’ve uploaded and confirmed your LAR file, you’re given a similar screen to what you’d be offered during export. There are some additional options available: _Update Data_ and _Authorship of the Content_. Here’s options and descriptions for each section:

**Update Data**

**Mirror:** All data and content inside the imported LAR is newly created the first time while maintaining a reference to the source. Subsequent imports from the same source updates entries instead of creating new entries.

**Mirror with overwriting:** Same as the mirror strategy, but if a document or an image with the same name is found, it is overwritten.

**Copy as New:** All data and content inside the imported LAR is created as new entries within the current Site every time the LAR is imported.

**Authorship of the Content**

**Use the Original Author:** Keep authorship of imported content whenever possible. Use the current user as author if the original one is not found.

**Use the Current User as Author:** Assign the current user as the author of all imported content.

Once you’ve selected the appropriate options, select _Import_ and your widget’s data is imported and ready for use.
