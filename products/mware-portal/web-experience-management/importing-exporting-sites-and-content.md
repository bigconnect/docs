# Importing/Exporting Sites and Content

Export/Import lets you backup and restore your Site and app data as a LAR file. There are two primary places Export/Import is used: Sites and apps.&#x20;

### Backing Up and Restoring Pages and Their Content <a href="#backing-up-and-restoring-pages-and-their-content" id="backing-up-and-restoring-pages-and-their-content"></a>

In _Site Administration_ → _Publishing_, you can find the _Export_ and _Import_ option for pages. If you click on _Export_, you see an interface for exporting your public or private pages. The Export feature exports your Site’s data as a single LAR file. Similarly, _Import_ is a similar interface for importing public or private pages from a LAR file.

When importing data into a Site, you should use a newly created Site to avoid conflicts between the existing data and the data being imported. When exporting Site data, you can specify exactly what data should be included in the LAR:

* Site pages (you can select exactly which ones)
* Page settings
* Theme
* Theme settings
* Logo
* Application configurations
* Application content
* Archived setups
* User preferences

A LAR file can be imported into a Site on another portal server. You can take content from a Site in one environment (say, a development or QA environment) and move it all to a Site on another server with LARs. You can use LARs to import data onto production servers, but you should not make this a regular occurrence. If you want to regularly move pages from one server to another, you should use the portal’s staging environment.

You can export LARs to use them as a backup. If you ever have to restore your Site, you must only import the latest LAR file. However, please be careful! If there’s content that exists both in the LAR and in the Site that’s importing the data, there may be a conflict, and data could be corrupted. If you want to restore a portal Site using a LAR file, delete the Site entirely, create a new Site with the same name as the old one, and then import the LAR file into the new Site. This way, there’s no chance for there to be a data conflict.

Some naming collisions are handled automatically. For example, a collision occurs if the LAR you’re importing and the Site both have a page with the same friendly URL. The portal resolves the collision by adding a number to the end of the friendly URL and incrementing until there’s no collision. Similarly, if importing a LAR into a Site causes a category name collision, the imported categories are automatically renamed.

**Note:** LAR files are version dependent. You can’t import a LAR file that was exported from one version of portal into a portal server that’s running a different version of the portal. Also, note that periodically exporting LARs is _not_ a complete backup solution.

### Page Export Example <a href="#page-export-example" id="page-export-example"></a>

Here’s how the export process works:

1. Go the _Site Administration_ → _Publishing_.
2. Click _Export_.
3.  Click _Add_ (![Custom Export](https://liferay-support.zendesk.com/hc/article\_attachments/360030189532/icon-add.png)).

    A _New Custom Export_ page loads, so you can choose the pages and content you want to export from your Site.
4. Enter _Lunar Resort Version 1_ for the _Title_.
5. Under _Pages_, select public or private pages and the settings you want to export.
6.  Under the _Content_ category, select _All_.

    Note that if you select one of the _Choose_ radio selectors or _Change_ links, you’re given checkboxes for options to choose. The applications’ content can also be selected for export, including the Documents and Media Library, Message Boards, and Web Content assets. You can even export the theme you’re using. Finally, you can select whether the permissions for your exported pages and content are included.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030189572/export-page-templates.png" alt="Figure 1: You can configure your export options manually by selecting pages, content, and permissions." data-size="original">

    Figure 1: You can configure your export options manually by selecting pages, content, and permissions.
7. Click _Export_.

Once you click _Export_, the menu automatically switches to the _Processes_ tab, where you see the status of your exported LAR file. You can select the _Download_ icon (![Download](https://liferay-support.zendesk.com/hc/article\_attachments/360030158331/icon-download.png)) to download the export to your local machine. Once you have the file, you can copy it to a backup location for safekeeping or import it into another installation of the portal. If you must rebuild or wish to revert back to this version of your Site, you can import this file by clicking the _Import_ button from the Publishing menu, browsing to it, and selecting it. You can also drag a LAR file inside the dotted area, which also executes the import process.

### Export Templates <a href="#export-templates" id="export-templates"></a>

Instead of manually customizing an export process every time you export pages/content, you can use an Export Template. This provides you the convenience of storing export process settings so they can be reused. If you export pages frequently and usually select the same options to export, you can create an export template to export with your standard options.

To create an export template,

1. Select the _Options_ icon (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030189552/icon-options.png)) from the top right corner of the screen and select _Export Templates_.
2. Click the _Add_ button (![Add Export Template](https://liferay-support.zendesk.com/hc/article\_attachments/360030189532/icon-add.png)).
3. Assign the template a _Name_ and _Description_.
4. Fill out the configuration options for your export process.
5. Click _Save_.

Your template is now available to use from the _Export Templates_ menu. To use the template,

1. Click the _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030158371/icon-actions.png)) next to the template.
2.  Select _Export_.

    This automatically fills the fields and options for exporting pages and their content.
3. Give the export a name.
4. Click _Export_ and your LAR file is generated.
