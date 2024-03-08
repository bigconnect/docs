# Creating a Page Template

With _page templates_, you can create pages based on predefined layouts and content. There are several basic page templates available out-of-the-box.

![Basic page templates are available.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/creating-a-page-template/images/01.png)

You may also create your own [content page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages) templates and [widget page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages#widget-pages) templates.

NOTE

For Liferay DXP 7.4+, Page Template Collections are called Page Template Sets in the Liferay UI.

### [Creating a New Page Template](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template#creating-a-new-page-template) <a href="#creating-a-new-page-template" id="creating-a-new-page-template"></a>

The steps below show how to create and use widget and content page templates.

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Design_ → _Page Templates_.
2. Select the _Page Templates_ tab.
3.  Click _New_ and create a new page template set to organize your page templates.

    ![Use the Page Templates tab to create a page template set.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/creating-a-page-template/images/02.png)
4. Click _Add_ ( ![Add page template icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) and select _Content Page Template_ or _Widget Page Template_.
5. Enter a Name.
6. Click _Save_.

The new page template appears. Follow the instructions in [Building Content Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/adding-elements-to-content-pages) to configure your content page template. Likewise, see [Widget Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages#widget-pages) to create your widget page template.

You can edit the page template at any time by clicking _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Edit_ next to the page template in the set.

TIP

You can preview your newly created page template. In the page templates page, select _Actions_ (![Actions Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Preview_ for the template you want to preview.

### [Creating a Page Template from an Existing Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template#creating-a-page-template-from-an-existing-page) <a href="#creating-a-page-template-from-an-existing-page" id="creating-a-page-template-from-an-existing-page"></a>

You can also create a page template from a page while editing it. The new page template keeps all the configurations for fragments and widgets when it is created. This saves you time while you expand your site.

Follow these steps to make a page template from a page:

1.  While [editing the page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/adding-elements-to-content-pages), click _Options_ ( ![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png) ) at the top right corner of the page and select _Convert to Page Template_

    ![Click convert to page template in the options menu to turn the page you are editing into a new page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/creating-a-page-template/images/03.png)
2.  In the modal that appears on the screen, select an existing page set or select _Save In New Set_ to create a new page template set for the page template.

    ![Here, you can set the basic information for your new page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/creating-a-page-template/images/04.png)

    If you do not have a page template set, choose a _Name_ and, optionally, a _Description_ for a new page template set. Click _Save_ to save your page as a template in the newly created template set.
3. Click _Save_.

The chosen page template set now has a new page template based on the page you are currently editing, as a draft.

IMPORTANT

You must publish this page template draft to use it for new pages.

### [Sharing a New Page Template](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template#sharing-a-new-page-template) <a href="#sharing-a-new-page-template" id="sharing-a-new-page-template"></a>

By default, only the creator of the page template can use it.

To give other users access to a newly created page template follow these steps:

1.  Select _Permissions_ from the page template’s Actions menu ( ![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ).

    ![Allow other users to access newly created page templates while configuring permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/creating-a-page-template/images/05.png)
2. Check the _View_ permission for the roles that you want to have access to the page template. If you want any user who can create a page to be able to use the page template, check the _View_ permission for the user role.
3. Click _Save_.

Users with the roles specified now have access to the page template.

### [Propagating Changes for Widget Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template#propagating-changes-for-widget-pages) <a href="#propagating-changes-for-widget-pages" id="propagating-changes-for-widget-pages"></a>

When creating a page from a widget page template, changes are inherited from the page template by default, so future changes are _automatically_ propagated to the pages that use the page template. Site administrators can disable this behavior through the [settings for the individual page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#general).

### [Propagating Changes for Content Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template#propagating-changes-for-content-pages) <a href="#propagating-changes-for-content-pages" id="propagating-changes-for-content-pages"></a>

Changes for content pages **are not automatically propagated** when created using a content page template. However, you can propagate changes to fragments used on a content page. See [Propagating Fragment Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/propagating-fragment-changes) for more information.

### [Sharing Page Templates With Other Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template#sharing-page-templates-with-other-sites) <a href="#sharing-page-templates-with-other-sites" id="sharing-page-templates-with-other-sites"></a>

You can share your page templates with other sites using the same process as you would for pages. Follow the instructions in [Exporting/Importing Site Pages and Content](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content) to learn how.

### [Staging and Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template#staging-and-page-templates) <a href="#staging-and-page-templates" id="staging-and-page-templates"></a>

IMPORTANT

Staging functionality is in [maintenance mode](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/upgrading-liferay/reference/maintenance-mode-and-deprecations-in-7-3) as of 7.3 GA1+.

If staging has been enabled, changes to the widget page template are propagated automatically to the staged page. These changes must still be approved before the page is published to the live site. For this reason, the automatic propagation of widget page template changes to the staged page cannot be turned off and the _Inherit Changes_ selector does not appear.

\
