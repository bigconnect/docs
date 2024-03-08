# Assigning Permissions to Web Content Structures and Templates

Web content structures and templates provide direct access to Liferay’s API. To avoid unauthorized or unintended access to Liferay DXP functionality, you should configure permissions for web content structures and templates.

As a best practice, define two different roles with access to web content structures and templates:

1. **Content Developer:** Users with this role have permission to create and edit web content structures or templates.
2. **Content Creator:** Users with this role have permission to view web content structures or templates, so they can use them to create content.

For information about how to create roles, see [Creating and Managing Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles).

Consider the information below when you assign permissions to web content structures or templates:

* Determine if your roles must have global permissions for all web content structures or templates across the Liferay DXP instance, or only for specific sites.
* The View permission only allows users to view web content structures or templates.
* The majority of users should not be able to edit web content structures or templates.

### [Security Considerations for Web Content Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/assigning-permissions-to-structures-and-templates#security-considerations-for-web-content-templates) <a href="#security-considerations-for-web-content-templates" id="security-considerations-for-web-content-templates"></a>

Web content templates use the FreeMarker Template Language (FTL) by default. Users with permission to create or edit templates using FreeMarker may execute arbitrary code on your DXP instance and access sensitive information, including information about other users.

IMPORTANT

Grant permission to create or edit web content templates in FreeMarker to trusted users only.

You can disable template creation completely. Once disabled, users can no longer access creation options for web content templates. This action does not affect existing templates.

1. Open the _Control Panel_ tab in the Global Menu.
2. Go to _Configuration_ → _System Settings_ → _Content and Data_ → _Dynamic Data Mapping_.
3. Click _Dynamic Data Mapping Web_.
4. Uncheck _Enable Template Creation_.
5. Click _Save_ or _Update_.

### [Assigning Permissions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/assigning-permissions-to-structures-and-templates#assigning-permissions) <a href="#assigning-permissions" id="assigning-permissions"></a>

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Content & Data_ → _Web Content_.
2. Select the _Structures_ tab to set permissions for structures, or the _Templates_ tab to set permissions for templates.
3. For the web content template where you want to assign permissions, click _Actions_ ( ![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) and select _Permissions_.
4. Select the permissions you need for your roles.
5. Click _Save_.

![Permissions dialog for web content structures and templates](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-structures/assigning-permissions-to-structures-and-templates/images/01.png)

See [Defining Role Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions) for more information on configuring permissions.

\
