# User Group Sites

With User Group Sites, you can create pages that are added automatically to each member’s [personal Site](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/personal-sites). These pages are organized into two [page sets](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages#page-sets), _My Profile_ (public) and _My Dashboard_ (private), just like personal Sites. If users belong to multiple User Groups, all pages from those groups are also added to their Sites.

![Create profile and dashboard pages that are added to each member's personal Site.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/user-groups/user-group-sites/images/01.png)

By default, inherited pages can only be edited from the User Group Site and cannot be edited from a personal Site. If needed, User Group administrators can define customizable page areas so users can add and configure widgets in a page.

User Group Sites can be designed [manually](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/user-group-sites#designing-user-group-sites-manually) or using a [Site Template](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/user-group-sites#using-site-templates).

IMPORTANT

For Liferay 7.4 U22+ and GA22+, new Liferay installations include a release feature flag that disables the creation of Private Pages, including My Dashboard pages. To manually create custom dashboard pages for a User Group, Private Pages must be enabled for your Liferay system. See [Enabling Private Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages#enabling-private-pages) for more information.

Alternatively, when Private Pages are disabled, you can add My Dashboard pages using a Site Template. Pages added to the template and applied to the My Dashboard page set are treated as Private Pages.

### [Designing User Group Sites Manually](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/user-group-sites#designing-user-group-sites-manually) <a href="#designing-user-group-sites-manually" id="designing-user-group-sites-manually"></a>

Follow these steps to design a User Group Site manually:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_, and click _User Groups_.
2. Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired group and select _Manage Pages_.
3.  [Add and design Site Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/adding-a-page-to-a-site).

    NOTE

    While private pages are disabled, all pages added in this way are added to the _My Profile_ page set. You must enable and create private pages to add pages to the _My Dashboard_ set.
4. (Optional) Click the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) to access additional Site applications and configuration options.

Once pages are added to a User Group Site, you can view them via the the _User Groups_ application. Just click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired group and select _Go to Profile Pages_ or _Go to Dashboard Pages_.

### [Using Site Templates](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/user-group-sites#using-site-templates) <a href="#using-site-templates" id="using-site-templates"></a>

Follow these steps to populate a User Group Site with profile and dashboard pages using [Site Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-templates):

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_, and click _User Groups_.
2. Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired group and select _Edit_.
3.  Use the _My Profile_ and _My Dashboard_ dropdown menus to select a Site Template for each page set.

    ![Select a Site Template for the My Profile and My Dashboard page sets.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/user-groups/user-group-sites/images/02.png)
4.  Determine whether to enable propagation of changes from the Site Templates.

    NOTE

    When enabled, changes in the template are automatically propagated to the User Group Site. If you disable this option but enable it again later, the template’s pages are copied to the Users’ Sites, overwriting any changes made.
5. Click _Save_.

Once saved, the template’s pages are added to each group member’s personal Site under their _My Profile_ or _My Dashboard_ page sets.

### [Legacy User Group Sites Behavior](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/user-group-sites#legacy-user-group-sites-behavior) <a href="#legacy-user-group-sites-behavior" id="legacy-user-group-sites-behavior"></a>

There is no impact on performance for inheritance of User Group Site pages, even with millions of users. Versions of Liferay Portal and Liferay DXP prior to 7.0, however, required User Group pages be copied to each User’s personal Site.

If you’re on Liferay DXP 7.2 and must keep that behavior, enable it by adding the following line to your `portal-ext.properties` file:

```log
user.groups.copy.layouts.to.user.personal.site=true
```

This property was removed in Liferay DXP 7.3.

When this property is set to `true`, the template pages are copied to a User’s personal Site once, and then may be modified by the User. This means that if changes are made to the template pages later, they only affect Users added to the User Group after the change is made. Users with administrative privileges over their personal Sites can modify the pages and their content if the _Allow Site Administrators to Modify the Pages Associated with This Site Template_ box has been checked for the template. When a User is removed from a User Group, the associated pages are removed from the User’s personal Site. If a User is removed from a group and is subsequently added back, the group’s template pages are copied to the User’s Site a second time. Note that if a User Group’s Site is based on a Site Template and an administrator modifies the User Group’s Site Template after users have already been added to the group, those changes only take effect if the _Enable propagation of changes from the Site Template_ box for the User Group was checked.
