# Asset Library Content

_Asset libraries_ provide a convenient way to share content across Liferay sites while streamlining content collaboration and reducing the risk of data duplication. To add, view, and manage an asset library’s content, first navigate to the Library’s menu page.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), select the _Applications_ tab, and click _Asset Libraries_ under Content.
2. Click the _name_ of the desired asset library.

Now you can access the library’s active applications to create and manage content. You can also enable staging, implement custom workflows, categorize content using tags and categories, create collections, and more.

![Click the desired asset library's name to access its menu page.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/asset-library-content/images/01.png)

### [Creating Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/asset-library-content#creating-content) <a href="#creating-content" id="creating-content"></a>

To create content in an asset library, first go to the library’s menu page, and click the desired application. From here, creating content is essentially the same as creating it in a site. The differences are mainly its scope and default theme settings.

However, some content requires additional configuration when set within an asset library’s context. Read [Categories and Vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/asset-library-content#categories-and-vocabulary) to learn more.

See [Content Authoring and Management](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management) articles for more information about creating each type of content.

NOTE

Only active applications appear on a library’s menu page. To activate or deactivate an application, see [Configuring Asset Libraries](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#configuring-asset-libraries) for more information.

### [Using Content in Sites](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/asset-library-content#using-content-in-sites) <a href="#using-content-in-sites" id="using-content-in-sites"></a>

Once you’ve added content to an asset library, you can access it from [connected sites](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#sites) and display it using display page templates, fragments, widgets, and more.

For Liferay 7.4 U21+ and GA21+, when using the item selector in a site, you can browse content (e.g. web content, documents and media, and blogs) in connected asset libraries. To do this, open the item selector window and use the breadcrumb to navigate to _Sites and Libraries_.

![Use the breadcrumb to navigate to sites and libraries](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/asset-library-content/images/02.png)

From here, navigate to the _Asset Library_ tab and select a connected asset library. Once selected, you can browse its content and select the desired asset.

![Navigate to the asset library tab and select a connected library.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/asset-library-content/images/03.png)

Alternatively, you can filter the item selector window to include content from _Everywhere_. This displays both site content and asset library content.

![Filter content in the item selector to include content from everywhere.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/asset-library-content/images/04.png)

When publishing content using the Asset Publisher widget, you must set its scope to include content from a specific asset library and select the content you want to use.

![Set the Asset Publisher's scope to include content from a specific asset library.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/asset-library-content/images/05.png)

See [Displaying Content](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content) documentation for more information about using content in your sites.

NOTE

Unlike sites, display page templates cannot be defined in an asset library. Instead, asset library content is displayed using each site’s default display page template for the respective content type.

### [Staging Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/asset-library-content#staging-content) <a href="#staging-content" id="staging-content"></a>

Asset libraries support both [Local Live](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-local-live-staging) and [Remote Live](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-remote-live-staging) staging. This provides a working environment for adding, removing, and editing assets in the Web Content or Documents and Media applications before making those changes live. Staged content options are limited to the applications enabled for the asset library. Once enabled, you can then connect the library to staging-enabled sites.

See [Using Staging in Asset Libraries](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/using-staging-in-asset-libraries) for more information.

### [Categorizing Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/asset-library-content#categorizing-content) <a href="#categorizing-content" id="categorizing-content"></a>

The Tags and Categories applications are enabled for all asset libraries. To access them, navigate to a library’s menu page and click _Tags_ or _Categories_ under Categorization. Tags created in an asset library are scoped to that library and can only be viewed and applied in that context. However, vocabularies and categories defined in an Asset library are immediately available in all connected sites.

NOTE

If created in an asset library, vocabularies and categories cannot be edited or removed in connected sites. They can only be modified in the asset library in which they were created.

See [Tags and Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories) documentation for more information about using these applications.

### [Categories and Vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/asset-library-content#categories-and-vocabulary) <a href="#categories-and-vocabulary" id="categories-and-vocabulary"></a>

Liferay DXP 2024.Q1+/Portal 7.4 GA112+

When creating a vocabulary within an asset library, some additional configurations for [categories and vocabularies](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content) are available:

1. Open the _Global Menu_ (![Applications Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Applications_ tab, and select _Asset Libraries_ under Content.
2. Click the name of the asset library where you want to add your vocabulary.
3. Under categorization, click _Categories_.
4. Click _Add_ (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-widget.png)) next to Vocabularies.
5.  Configure whether and where that vocabulary is required. You can find the configuration under Associated Asset Types with three options:

    ![Determine whether the vocabulary is required and set where it's required.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/asset-library-content/images/06.png)

    | Option                                                 | Description                                                                                      |
    | ------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
    | Not Required                                           | The vocabulary field is not required.                                                            |
    | Only Required in This Asset Library                    | The vocabulary field is only required in content created in that asset library.                  |
    | Required in This Asset Library and Its Connected Sites | The vocabulary field is required in content created in the asset library and in connected sites. |

    NOTE

    When the vocabulary is marked as required, an asterisk marks the vocabulary field, and it must have content or it cannot be saved. Trying to save content without the required vocabulary causes an error message to appear.

    ![Determine whether the vocabulary is required and set where it's required.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/asset-library-content/images/07.png)

Note that when using the Required in This Asset Library and Its Connected Sites option, if a [site gets disconnected](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#sites) from the asset library, the vocabulary stops being shown on the content created at a site level. Without the required vocabulary, you can publish content without filling in the vocabulary field.

If a site is connected to an asset library with a required vocabulary, the vocabulary becomes required for the content on that site, and filling in the vocabulary field becomes mandatory. Content cannot be saved without entering the required vocabulary field first.

### [Exporting and Importing Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/asset-library-content#exporting-and-importing-content) <a href="#exporting-and-importing-content" id="exporting-and-importing-content"></a>

You can export and import asset library content as LAR files. To initiate a new export/import process for a library, go to its dashboard and click _Export_ or _Import_ at the bottom of the page. Then click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to initiate a new process.

Exporting and importing library content follows the same process as site assets. You can also create and use custom export templates by clicking _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the application bar and selecting _Export Templates_. See [Exporting/Importing Site Pages and Content](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content) for more information.

### [Removing Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/asset-library-content#removing-content) <a href="#removing-content" id="removing-content"></a>

Asset libraries support the [Recycle Bin](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/recycle-bin/configuring-the-recycle-bin) application. When enabled, any content removed from the library is temporarily stored in its recycle bin. By default, recycled content is retained for 43200 minutes (i.e., 30 days), but you can adjust this period in each asset library’s settings. Recycled content that has been in the recycle bin for more than the allotted time is deleted automatically from your instance.

Alternatively, you can then manually delete recycled content or restore it to your asset library via the asset’s _Actions_ button ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ).

![Manually delete recycled content or restore it to your asset library.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/asset-library-content/images/08.png)

See [Recycle Bin Overview](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/recycle-bin/recycle-bin-overview) for more information.

\
