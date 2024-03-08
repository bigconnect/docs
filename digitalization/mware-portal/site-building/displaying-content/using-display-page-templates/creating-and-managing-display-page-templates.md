# Creating and Managing Display Page Templates

Display page templates define structures for displaying content at a dedicated friendly URL. These templates currently support the following content types:

* [Blog Entries](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/getting-started-with-blogs)
* [Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content) Liferay DXP/Portal 7.4+
* [Custom Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries) Liferay DXP/Portal 7.4+
* [Documents](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/publishing-documents)
* [Web Content Articles](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/adding-a-basic-web-content-article)

See [Using Display Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates) to learn more.

### [Creating a Display Page Template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#creating-a-display-page-template) <a href="#creating-a-display-page-template" id="creating-a-display-page-template"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and click _Design_ → _Page Templates_.
2. Click the _Display Page Templates_ tab and click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3.  Enter a _Name_ for your template and select a _Content Type_ and _Subtype_, if applicable. Then click _Save_.

    ![Select the content type and subtype for your new template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/01.png)
4. Design the template using the same interface and page elements as [content pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/content-page-editor-ui-reference).
5.  Once you’ve added page elements, you can [map](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference#mapping-settings) them to content structure fields (e.g., title, description) by double-clicking on an editable field. Mapped fields are outlined in purple.

    TIP

    You can use mapped fields when configuring SEO and Open Graph settings for a display page template. See [Configuring SEO and Open Graph](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph) for more information.

    ![Map page elements to structure fields.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/02.png)
6. Before publishing, you can preview your editable fields mappings with existing content using the [preview function](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#previewing-the-display-page-template-content-mappings) (available 7.4+).
7. When finished, click _Publish_ to save your work.

You can now use the template to [display your content](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages).

#### [Setting a Friendly URL for the Display Page Template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#setting-a-friendly-url-for-the-display-page-template) <a href="#setting-a-friendly-url-for-the-display-page-template" id="setting-a-friendly-url-for-the-display-page-template"></a>

Liferay 2023.Q4+/GA102+

You can set a friendly URL for your display page template. The friendly URL is used when the display page template is not marked as default or when it is manually selected for use.

For example, when you link a button in a content page to a mapped URL, you can select an object connected to a display page template as the item and select the display page template as the field. In this case, the friendly URL is used for the selected display page template.

![Set a friendly URL for your display page template. The friendly URL is used when you select a display page template specifically from a list of links.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/03.png)

To set a friendly URL for your display page template,

1. While editing your display page template, click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) at the top right corner of the page.
2.  Select _Configure_.

    You can access the display page template configuration through the Page Templates app. Open the Page Templates app. In the Display Page Templates tab, click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the display page template you want to configure and choose _Configure_.
3. Under the General section, define your friendly URL.
4. Click _Save_

#### [Mapping Links to Different Display Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#mapping-links-to-different-display-page-templates) <a href="#mapping-links-to-different-display-page-templates" id="mapping-links-to-different-display-page-templates"></a>

IMPORTANT

This feature is currently behind a release feature flag. Read [Release Feature Flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags) for more information.

If a content type (e.g., custom object, web content article, or blogs entry) more than one display page template, you can map links to any available template for that content type.

For example, you can create different display page templates for an object, each corresponding to a step in a mapped form (e.g., submission, editing, and approval). Then, you can choose which display page template to use for each stage, enabling the creation of multi-step forms.

![Map links to different display page templates](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/04.png)

This gives you more freedom when dealing with objects and [forms mapped from objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-fragments-to-build-forms) with multiple steps.

#### [Previewing the Display Page Template Content Mappings](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#previewing-the-display-page-template-content-mappings) <a href="#previewing-the-display-page-template-content-mappings" id="previewing-the-display-page-template-content-mappings"></a>

Liferay DXP/Portal 7.4+

While editing, Liferay provides two ways to preview display page templates with content items.

From the Application Menu, you can click the _Preview With_ drop-down menu and choose a content item. If no item appears, click _Select Other Item_ and browse for the desired content. This dynamically populates the template with content data according to your field mapping.

![Choose the content you want to use to preview your display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/05.png)

Alternatively, from the display page template editor, click the _Options_ menu(![Options menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) button at the top right corner and choose _Preview in a New Tab_.

### [Viewing Display Page Template Usage](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#viewing-display-page-template-usage) <a href="#viewing-display-page-template-usage" id="viewing-display-page-template-usage"></a>

Liferay DXP/Portal 7.4+

You can manage the different display page templates using the _Actions_ menu (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)). From here, the _View Usages_ option provides a list of content that uses a specific display page template other than the default template. This option is not available for default templates.

![Managing your display page template using the Actions menu](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/06.png)

Before you delete a display page template that is in use, you have two ways to unassign the display page template from the content:

* Assign to Default: Your content is unassigned from the current display page template and assigned to the default display page template for the content type and subtype (if applicable.)
* Unassign: Your content is not assigned to any display page template.

Follow these steps to view your display page template usage and unassign your content:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Page Templates_.
2. Click the _Display Page Templates_ tab.
3. Click the display page template’s _Actions_ menu (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _View Usages_.
4. Select one or more elements from the list of content using the display page template.
5. Click the _Actions_ menu (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the top-right corner and select _Assign to Default_ or _Unassign_.
6. Click _OK_.

If you assign your content to a new display page template, make sure the content appears as expected. To preview and display your content, see [Publishing Content with Display Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages).

### [Duplicating a Display Page Template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#duplicating-a-display-page-template) <a href="#duplicating-a-display-page-template" id="duplicating-a-display-page-template"></a>

Liferay 7.4 2023.Q4+/GA102+

You can duplicate a published display page template. The copy is linked to the original content type, and its mappings are the same as the original page.

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) (1) and navigate to _Design_ → _Page Templates_ (2).
2.  Select the _Display Page Templates_ tab (3), click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the display page template you want to alter, and choose _Make a Copy_ (4).

    If the display page template was never published, the Make a Copy option is disabled.
3.  In the sub-menu, choose _Display Page_ or _Display Page With Permissions_.

    The copy is named `Original Name + Copy + Duplicate Number` and is created as a draft so the user can review it before publishing.

![Use the actions menu to make a copy of your published display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/08.png)

IMPORTANT

You can only make a copy of display page templates if they were published at some point. If a published template becomes a draft again, you can copy the last published version of the display page template.

### [Changing the Content Type of Display Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#changing-the-content-type-of-display-page-templates) <a href="#changing-the-content-type-of-display-page-templates" id="changing-the-content-type-of-display-page-templates"></a>

Liferay 7.4 2023.Q4+/GA102+

WARNING

Changing the content type/subtype of a display page template may cause some of its elements to lose their previous mapping.

The mapped elements in a display page template are highlighted in purple in the Browser menu on the left. The unmapped elements are not highlighted in purple.

To change the content type linked to a display page template,

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) (1) and navigate to _Design_ → _Page Templates_ (2).
2.  Select the _Display Page Templates_ tab (3), click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the display page template you want to alter, and choose _Change Content Type_ (4).

    ![Change the content type linked to the display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/09.png)
3.  Choose the new content type for your display page template, the new content type and its subtype, or just the new subtype and click _Save_.

    If the content type has a subtype, you must select a subtype before saving the new content type.

#### [Changing the Content Type of Assigned Display Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates#changing-the-content-type-of-assigned-display-page-templates) <a href="#changing-the-content-type-of-assigned-display-page-templates" id="changing-the-content-type-of-assigned-display-page-templates"></a>

You cannot change the content type of a display page template if it’s assigned to an asset (e.g. web content article).

If you try changing the content type, a _Display Page In Use_ message appears. You can click _View Display Page Usages_ to see the assets using the display page template. You can then select assets, click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)), and select _Assign to Default_ or _Unassign_.

Assign to Default assigns the asset to the default display page template, while Unassign removes the display page template and assigns the asset to None instead.

![While viewing the display page usage, you can assign an asset to the default display page template or unassign the display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/10.png)

The picture below shows the display page settings for a web content article with the display page template assigned to it (1), after using the Assign to Default option (2) and after using the Unassign option (3).

![The display page settings for a web content article with a display page template assigned to it, after using the Assign to Default option, and after using the Unassign option.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates/images/11.png)

\
