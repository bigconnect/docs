# Exporting/Importing Site Pages and Content

Liferay sites include the export/import applications for extracting and transferring site data as Liferay Archive (LAR) files. You can access these tools at the site level.

NOTE

You can also access export and import functions for individual site applications and [page widgets](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-widgets/configuring-widgets/exporting-importing-widget-data).

### [Site LAR Properties and Requirements](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content#site-lar-properties-and-requirements) <a href="#site-lar-properties-and-requirements" id="site-lar-properties-and-requirements"></a>

LARs have these properties and requirements:

* LARs are version dependent. They can be imported into a site on another Liferay server, as long as the Liferay versions on each server are the same.
* Some naming conflicts are handled automatically. For example, if the LAR you’re importing and the site both have a page with the same friendly URL, a number is appended to the friendly URL and incremented until the conflict no longer occurs. The same is true for category names.
* You can use LARs to restore a site, but you must delete your site entirely and then create a new site with the same name to import the LAR into. This avoids potential data conflicts between content that exists both in the LAR and in the site.

IMPORTANT

Periodically exporting LARs is _not_ a backup solution; it should only be used to migrate data between two environments. Please refer to the [Backing up a Liferay Portal Installation](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/maintaining-a-liferay-installation/backing-up) to learn about backing up your Liferay installation.

### [Exporting Site Pages and Content](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content#exporting-site-pages-and-content) <a href="#exporting-site-pages-and-content" id="exporting-site-pages-and-content"></a>

Follow these steps to export site pages and content:

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Publishing_, and click _Export_.
2.  Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to create a new export process.

    This opens an export template.

    ![Name and configure the export template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/exporting-importing-site-pages-and-content/images/01.png)
3. Enter a title for the export process.
4.  Configure the export process by selecting the site pages and content you want to export.

    Under _Pages_, you can select which pages and page sets are included in the export. You can also determine whether the Theme Settings, Logo, Site Page Settings, and Site Template Settings are included.

    Under _Content_, you can select which applications are included in the export. You can also determine whether to export all data for the selected applications, or filter included application data by date or time.

    Under _Permissions_, determine whether to include page and content permissions in the export.
5. Click _Export_ to begin the export process.

Once finished, click the _Download_ button (![Download Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-download.png)) to download the generated LAR file. You can now import the LAR to another Liferay site.

![Download the LAR file.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/exporting-importing-site-pages-and-content/images/02.png)

NOTE

The generated LAR includes all data selected during export configuration. If this includes private or sensitive information, ensure the LAR file is stored in a secure location.

### [Importing Site Pages and Content](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content#importing-site-pages-and-content) <a href="#importing-site-pages-and-content" id="importing-site-pages-and-content"></a>

IMPORTANT

If the exported site uses a template, you must first export and import the site template to the Liferay installation before you can import the site. See [Exporting and Importing Site Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-templates/exporting-importing-site-templates) for more information.

Follow these steps to import site pages and content:

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Publishing_, and click _Import_.
2. Click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to create a new import process.
3.  Add the desired LAR file and click _Continue_.

    ![Add the LAR file and click Continue.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/exporting-importing-site-pages-and-content/images/03.png)
4. Review and configure the import process.
5.  Click _Import_ to begin importing.

    ![Configure and begin the import process.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/exporting-importing-site-pages-and-content/images/04.png)

### [Creating Export Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content#creating-export-templates) <a href="#creating-export-templates" id="creating-export-templates"></a>

Instead of manually creating an export process every time you export site pages content, you can create an export template to store and reuse your export settings. To create an export template, follow these steps:

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Publishing_, and click _Export_.
2.  Click _Options_ ![Options Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png) in the Application Bar and select _Export Templates_.

    ![Click the Actions button in the Application Bar and select Export Templates.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/exporting-importing-site-pages-and-content/images/05.png)
3. Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
4. Name and configure the export template.
5. Click _Save_.

You can now select your template when [creating a new export process](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content#exporting-site-pages-and-content).

### [Configuring Export/Import Processes](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content#configuring-exportimport-processes) <a href="#configuring-exportimport-processes" id="configuring-exportimport-processes"></a>

Liferay provides settings for configuring export/import processes as well as staging specific features. These settings affect staging and site template propagation.

| Setting                                   | Description                                                                                                                                                                                                                                               |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Include All Asset Links                   | If checked, all asset links are included from the site within the given date range. If unchecked, only asset links connected to the content are included.                                                                                                 |
| Replicate Individual Deletions by Default | If checked, delete operations performed for content types are always replicated in the target site by default.                                                                                                                                            |
| Publish Permissions by Default            | If checked, permissions assigned for exported pages and individual portlet windows are always included by default.                                                                                                                                        |
| Validate File Entries                     | If checked, publishing or importing content types with text causes Liferay to check the content for links to documents and ensure they point to valid documents.                                                                                          |
| Validate Journal Feed References          | If checked, publishing or importing content types with text causes Liferay to check the content for references to web content feeds and ensure they point to valid feeds.                                                                                 |
| Validate Layout References                | If checked, publishing or importing content types with text causes Liferay to check the content for links to one of the portal’s virtual hosts and ensure they point to a valid Liferay page. Uncheck this if your virtual host has pages not in Liferay. |
| Validate Missing References               | If checked, publishing and import processes cause Liferay to check for missing references. If unchecked, missing reference validation is skipped. As a result, references without existing targets can be published or imported to the target site.       |

\
