# Managing Translation Permissions

The required permissions for translating content pages and web content depend on the translation method:

* Manual translation requires either Update permissions for the resource type, or Translate permissions for one or more languages.
* Exporting and importing translations requires Update permissions for the resource type.

### [Assigning Update Permissions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/managing-translation-permissions#assigning-update-permissions) <a href="#assigning-update-permissions" id="assigning-update-permissions"></a>

Users with Update permissions for pages and web content can translate them manually into any available language. They can also export them as XLIFF files for translation and then import the translations when finished. See [Defining Role Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions) to learn how to assign these permissions at the application level.

### [Assigning Translate Permissions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/managing-translation-permissions#assigning-translate-permissions) <a href="#assigning-translate-permissions" id="assigning-translate-permissions"></a>

Translate permissions are language-specific and grant access to the Translate action for content pages and web content. Users with Translate permissions can only use languages they’ve been assigned. Once granted, these permissions cannot be restricted by individual assets.

NOTE

The languages available for translation are determined by your instance’s localization settings. See [Initial Instance Localization](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/initial-instance-localization) and [Virtual Instance Localization](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/localization) for more information.

Follow these steps to assign Translate permissions for specific languages:

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _Roles_.
2.  Click the _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired role and select _Edit_.

    Alternatively, you can click the _Add_ button ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) to create a new role for translations.
3. Click the _Define Permissions_ tab and go to _Site and Asset Library Administration_ → _Content & Data_ → _Translation Processes_ in the permissions sidebar menu.
4.  Use the checkboxes to assign the desired language permissions to the selected role.

    ![Assign the desired language permissions to the selected Role.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/managing-translation-permissions/images/01.png)
5.  Click _Change_ (![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png)) under the Scope column to adjust the permission’s scope. By default, the change applies to all sites and asset libraries, but you can customize it to specific ones.

    Browse recently used sites in the Recent tab, all sites in My Sites, and asset libraries in Asset Libraries to select the scope.

    You can repeat this process to select additional sites or asset libraries, expanding the scope of permissions.
6. Click _Save_ at the bottom of the page.

Users with the configured role can now manually translate web content and content page experiences into the specified languages.

\
