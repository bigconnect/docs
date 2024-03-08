# Publishing Content With Display Pages

You can use display page templates to create reusable dynamic designs for displaying content items at their friendly URLs. After [designing the template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates), you can make it the default template for displaying its content type or manually configure only some of your content items to use it. Once set, users can view the content item’s display page by visiting its friendly URL. The content is rendered dynamically according to your template design and mappings. See [Using Display Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates) for more information.

NOTE

Even though you can create multiple display page templates for the same content type, you can select only one default template. To use an alternative template as the default, you must assign the template to content items individually.

### [Setting a Default Display Page Template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages#setting-a-default-display-page-template) <a href="#setting-a-default-display-page-template" id="setting-a-default-display-page-template"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and click _Design_ → _Page Templates_.
2. Click the _Display Page Templates_ tab.
3.  Click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired template and select _Mark as Default_

    TIP

    Each template’s content type is listed under its name (1). The blue check (2) indicates which templates are marked as default.

    ![Select a display page template as the default one for the content type using the Actions menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages/images/01.png)

### [Setting a Display Page Template for Individual Content Items](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages#setting-a-display-page-template-for-individual-content-items) <a href="#setting-a-display-page-template-for-individual-content-items" id="setting-a-display-page-template-for-individual-content-items"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to the desired application (e.g., web content, blogs, categories).
2.  Begin editing or creating a content item.

    ![Edit or create a content item.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages/images/02.png)
3.  In the Properties tab, expand the _Display Page Template_ section, click the drop-down menu, and select _Specific_.

    NOTE

    If you don’t choose a specific display page template, your content is published using the [default one](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages#setting-a-default-display-page-template) for the content type.

    ![Select a specific display page template to configure it.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages/images/03.png)
4. Click _Preview_, to preview what the display page looks like.
5.  (Optional) In the Properties tab, expand the _Friendly URL_ panel and modify the friendly URL. This determines the URL for displaying your content item using the display page template.

    NOTE

    For Liferay 7.4 U34+/GA34+, the Display Page Template framework takes into account an asset’s friendly URL history. This means all URLs saved in the file’s URL history redirect automatically to the file’s current URL. The friendly URL history is supported for both [documents](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/configuring-document-urls) and [blogs](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/displaying-blogs).

    ![You can configure the friendly URL used for your displayed content.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages/images/04.png)
6. Click _Publish_.

Once set, Liferay uses the template to display the content item at its unique friendly URL. If desired, you can preview your content’s display page at any time by clicking the content’s _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and selecting _Preview_ or _View Content_

_Preview_ displays the content in the display page template context, while _View Content_ redirects you to the content item’s friendly URL.

![Preview your content in the context of the display page template or view the Display Page using the content's Action menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages/images/05.png)
