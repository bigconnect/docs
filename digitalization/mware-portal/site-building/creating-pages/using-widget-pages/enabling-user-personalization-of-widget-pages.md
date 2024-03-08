# Enabling User Personalization of Widget Pages

Administrators can designate some or all sections (rows and columns of the page layout) of a widget page as customizable for site members.

Page customizations made in those customizable areas only appear for the user who made the customizations.

### [Enabling Page Customizations](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-widget-pages/enabling-user-personalization-of-widget-pages#enabling-page-customizations) <a href="#enabling-page-customizations" id="enabling-page-customizations"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Site Builder_ → _Pages_.
2. Open _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the desired widget page and select _Configure_.
3. Select the _General_ tab at the top of the page and go to the _Customization Settings_ panel.
4.  Check the _Customizable_ checkbox.

    ![To enable page customizations, click on the Configure Page button next to the page, expand the Customization Settings area, and check Customizable option.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/using-widget-pages/personalizing-pages/images/01.png)
5. Toggle the _Not Customizable_ option to _Customizable_ for each section you want to make customizable.

### [Customizing Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-widget-pages/enabling-user-personalization-of-widget-pages#customizing-pages) <a href="#customizing-pages" id="customizing-pages"></a>

With customization activated for a page, site members can add apps to the customizable sections of the page from the Add menu in the top right. Site members can make two kinds of customizations to customizable regions:

* Add or remove apps
* Configure applications

NOTE

Site members can’t change a non-instanceable app’s configuration inside a customizable region since those apps are tied to the site where they’ve been added.

Site members have access to these contextual options from the page’s Options menu (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) in the Control Menu:

**View Page without my customizations**: Displays the default page and hides the Add menu.

**Reset My Customizations**: Restores the customized page back to the default page, discarding any customizations.

**View My Customized Page**: Displays the page with the user’s customizations and makes the Add menu available so users can add widgets to the page.

![Customizable areas are highlighted green when organizing apps on the page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/using-widget-pages/personalizing-pages/images/02.png)

Administrators of customizable pages have the same two views as site members: the default page view and the customized page view. Changes they make to the non-customizable sections of the default page affect all users just like other pages, while changes they make to the customized sections affect only themselves and do not overwrite users’ customizations.

### [Granting Roles Permission to Customize Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-widget-pages/enabling-user-personalization-of-widget-pages#granting-roles-permission-to-customize-pages) <a href="#granting-roles-permission-to-customize-pages" id="granting-roles-permission-to-customize-pages"></a>

By default, site members can make page customizations but non-site members and guests can’t. Administrators can update permissions for an existing role or [create a new role](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles) and assign it to other users to enable them to customize pages. Follow these steps to grant role permissions to customize pages:

1. Open the _Global Menu_ (![Applications Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), and navigate to _Control Panel_ → _Users_ → _Roles_.
2. Open _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the desired role and select _Edit_.
3. Click the _Define Permissions_ tab for the role and go to _Site and Asset Library Administration_ → _Site Builder_ → _Pages_ under Summary.
4.  Scroll down to the Page section under the Resource Permissions heading and check the _Customize_ permission.

    Users can customize the look and feel of apps and import or export app settings with the Customize permission.
5. Scroll down and click _Save_ to apply the changes.
