# Creating and Managing Roles

Once you [understand](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions) roles and permissions, you can create the roles you need to empower your users to work within your Liferay DXP solution.

Create and manage roles in Control Panel → Users → Roles.

### [Creating Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles#creating-roles) <a href="#creating-roles" id="creating-roles"></a>

First, [determine the scope](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions) of the role you must create. Roles can be scoped globally (regular roles), by site, organization, asset library, or account.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Roles_.
2. Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3. Select a role type.
4. Enter a localizable title and description.
5. Enter a key. This is a unique identifier for referencing the role programmatically. By default, the title value is used.
6. Click _Save_.

![Select a role type and enter a title, description, and key.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/creating-and-managing-roles/images/01.png)

Now you can [define role permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions) and [assign the role to users](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/assigning-users-to-roles).

### [Updating Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles#updating-roles) <a href="#updating-roles" id="updating-roles"></a>

You can update a role’s title, description, and key at any time, but you cannot change its type/scope.

To do this, click the role’s _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Edit_. When finished, click _Save_.

### [Role Management Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles#role-management-permissions) <a href="#role-management-permissions" id="role-management-permissions"></a>

You can assign permissions for managing individual roles.

1. Click the role’s _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Permissions_.
2.  Specify the actions that existing regular roles can perform on the selected role. This assignment is for each individual role and is different from [defining permission for the Roles application](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions).

    ![Permissions can be configured for role creation and management.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/creating-and-managing-roles/images/02.png)

By default, only the Owner and Administrator regular roles can grant these permissions for a role. The Administrator role doesn’t appear in the permissions matrix because its permissions cannot be modified. The default user in a testing installation has this permission at the global scope and can carry out all actions, as well as assign users and define permissions for a role.

WARNING

Be careful about granting these permissions. This is administrative functionality that’s best left to a few trusted users.

### [Deleting Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles#deleting-roles) <a href="#deleting-roles" id="deleting-roles"></a>

1. Click the role’s _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Delete_.
2.  Confirm that you really want to delete the role. The role is deleted immediately, along with any [workflow task assignments](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/reviewing-assets) associated with it.

    ![Click OK to delete the role.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/creating-and-managing-roles/images/03.png)

### [Exporting and Importing Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles#exporting-and-importing-roles) <a href="#exporting-and-importing-roles" id="exporting-and-importing-roles"></a>

If needed, you can export and import roles. However, Liferay only exports company-wide and personal site permissions. Site and asset library-related permissions are not exported.

To begin an export or import process,

1.  Open the Roles application, click the _Actions_ button ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) in the Application Bar, and select _Export/Import_.

    ![Open the Roles application, click the Actions button in the Application Bar, and select Export/Import.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/creating-and-managing-roles/images/04.png)
2.  Configure export/import processes, download and upload LAR files, or review current and previous export/import processes.

    ![Configure and review export/import processes.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/creating-and-managing-roles/images/05.png)

### [Deleting Asset Containers Deletes their Assets](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles#deleting-asset-containers-deletes-their-assets) <a href="#deleting-asset-containers-deletes-their-assets" id="deleting-asset-containers-deletes-their-assets"></a>

A Web Content folder contains Web Content articles. The Web Content folder is an asset container, and the Web Content article is an asset. It’s possible to give a role permission to delete an asset container without giving the role permission to delete individual assets. In that case, beware: if a role assignee deletes an asset container with individual assets in it, the individual assets themselves are deleted as well.

| Asset Container            | Asset                     |
| -------------------------- | ------------------------- |
| Web Content Folder         | Web Content Article       |
| Knowledge Base Folder      | Knowledge Base Article    |
| Message Boards Category    | Message Boards Thread     |
| Wiki Node                  | Wiki Page                 |
| Documents and Media Folder | Document                  |
| Form                       | Form Record               |
| Dynamic Data List          | Dynamic Data List Records |
| App Builder Object         | App Builder App           |

Besides Web Content folders, examples of asset containers include Bookmarks folders, Message Boards categories, Wiki nodes, and Documents and Media folders.

\
