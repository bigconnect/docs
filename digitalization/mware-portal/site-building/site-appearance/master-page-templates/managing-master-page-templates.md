# Managing Master Page Templates

To manage a master page template, follow these steps:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Design_ → _Page Templates_.
2. Select the _Masters_ tab.
3.  Click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for a master page template and select one of the options.

    ![Manage the master page template from the masters tab of the page templates application and opening the actions menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/defining-headers-and-footers/managing-master-page-templates/images/01.png)

**Edit:** Configure the master page template.

**Change Thumbnail:** Select a thumbnail image to display for the master page template’s card.

**Mark as Default:** This makes the template the default master page for widget pages. This option is available with approved master page templates.

**Rename:** Change the name of the template.

**Export:** Export and download the master page template definition. This option is available with approved master page templates.

**Make a Copy:** Duplicate the master page template. The copy uses the same name with “Copy” appended to the end.

**Permissions:** Define permissions for the template.

**Delete:** Remove the master page template. You can also delete multiple master page templates at once by checking the box for each template and selecting _Trash_ (![Trash icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-trash.png)) from the management toolbar.

**Discard Draft:** Discard the master page draft. This option is available with drafts.

NOTE

After changes are made and published, they are propagated to all pages that use the master page template.

### [Changing the Master Page Template of a Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/defining-headers-and-footers/managing-master-page-templates#changing-the-master-page-template-of-a-page) <a href="#changing-the-master-page-template-of-a-page" id="changing-the-master-page-template-of-a-page"></a>

You can change a page’s master page template in the [content page editor](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/content-page-editor-ui-reference). Follow these steps to change the template of a page:

1. Navigate to the page where you want to change the master page template.
2. Click _Edit_ (![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png))
3. In the editing sidebar, click the _Page Design Options_ menu (![Page Design menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-format.png)).
4.  Under the Masters section, select the master page template you want to use.

    ![Select the master page template you want to use through the page design options menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/defining-headers-and-footers/managing-master-page-templates/images/02.png)

### [Changing a Master Page Template’s Design](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/defining-headers-and-footers/managing-master-page-templates#changing-a-master-page-templates-design) <a href="#changing-a-master-page-templates-design" id="changing-a-master-page-templates-design"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Design_ → _Page Templates_.
2. Select the _Masters_ tab.
3. Click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the master page template you want to configure → _Edit_.
4.  Click the _Page Design Options_ menu (![Page Design Options menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-format.png)) → _More Page Design Options_(![More Page Design Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog3.png))

    ![Click More Page Design Options to access the master page templates general pptions](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/defining-headers-and-footers/managing-master-page-templates/images/03.png)
5.  In the Design tab, you can alter the style of your master page template.

    ![Select a new master page template from the available options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/defining-headers-and-footers/managing-master-page-templates/images/04.png)

**Theme:** Select whether to use theme from pages of a site or select a specific theme for the current master page template.

**Favicon:** Change the default favicon applied from the theme.

**Style Book:** Change the default style book applied from the theme.

**Theme CSS Client Extension:** Add [theme CSS client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/front-end-client-extensions#theme-css-client-extensions) to replace the default CSS fully from a theme without deploying an entire theme.

**CSS Client Extensions:** Add [CSS client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/front-end-client-extensions#css-client-extensions) to override CSS styling of a content page that uses the master page template.

**Custom CSS:** Custom CSS can also be applied and is loaded after the theme. Custom CSS is disabled when using the inherited theme.

**JavaScript Client Extension and Custom JavaScript:** [JavaScript client extentions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/front-end-client-extensions#javascript-client-extensions) can be added to have JavaScript run when the page using this master page template is visited. Custom JavaScript code can be added that is executed at the bottom of the page.

NOTE

A theme is applied to a content page through a master page template. When a master page template is applied to a content page, the design of the content page is defined by the master page template.

\
