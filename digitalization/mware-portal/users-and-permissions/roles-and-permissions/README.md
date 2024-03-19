# Roles and Permissions

To get things done in Liferay DXP, [Users](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/users/understanding-users) must have the proper permissions. Roles join Users with permissions. Most administrative work for Roles and permissions is done in _Control Panel_ → _Users_ → _Roles_.

![Manage Roles from the Control Panel.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions/images/03.png)

To skip straight to creating Roles and assigning Users, see

* [Role Creation and Management](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles)
* [Defining Role Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions)
* [Assigning Users to Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/assigning-users-to-roles)

NOTE

[Site Teams](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/creating-teams-for-sites) have a dedicated permissions management User Interface.

### [Roles and Scope](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions#roles-and-scope) <a href="#roles-and-scope" id="roles-and-scope"></a>

To give Users the permissions they need to perform their intended function within Liferay DXP, you must first associate the set of permissions with a Role.

Using a _Message Boards Admin_ Role and a User _Jane Slaughter_ as an example, here are the steps you might follow:

1. Determine the function needed: _Manage Message Boards in Liferay DXP._
2. Determine the scope: _Throughout the Virtual Instance (globally)._
3. Create the properly scoped Role, assigning the permissions that power the function: _Message Boards Admin (Regular Role)._
4. Create the User collection: _a User Group called Message Boards Administrators._
5. Assign the Role to the User Collection: _Message Boards Administrators → Message Boards Admin Role._
6. Assign Users to the User Collection: _Jane Slaughter → Message Boards Administrators._

The conceptual diagram below shows how the User inherits the necessary permissions in Liferay DXP’s Roles and Permissions system. Only one fully-wired Role assignment is displayed here (for the User who becomes a Message Boards Administrator), but you can see how drawing new arrows would link the User with permissions via other User collections and Role assignments.

![Roles exist to link permissions efficiently with Users.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions/images/02.png)

Roles have scope, so they apply at different levels.

| Permission Scope                         | Role Type     | Where is it assigned to Users?                                                              | Available Assignments                                                        |
| ---------------------------------------- | ------------- | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Throughout the Virtual Instance (global) | Regular       | Control Panel → Users → Roles (Click on the Role) → Assignees                               | <p>User Groups<br>Organizations<br>Sites<br>Segments<br>Individual Users</p> |
| A single Organization                    | Organization  | Control Panel → Users → Users and Organizations → Organizations (Organization actions menu) | Individual Users                                                             |
| A single Site                            | Site          | Site Administration → People → Memberships                                                  | <p>User Groups<br>Organizations<br>Segments<br>Individual Site Members</p>   |
| A single Asset Library                   | Asset Library | Site/Library Administration of Asset Library → People → Memberships                         | <p>Organizations<br>User Groups<br>Individual Users</p>                      |

Individual [Users](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/users/understanding-users) can be manually assigned to Roles. This method is less efficient than using collections of Users. Users can also be [automatically](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances) assigned to Roles of all scopes through a Virtual Instance Setting called Default User Associations.

#### [Regular Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions#regular-roles) <a href="#regular-roles" id="regular-roles"></a>

Instance scoped Roles are called Regular Roles. These Roles grant permissions globally, or throughout the [Virtual Instance](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances).

Several User collections can be assigned to Regular Roles:

* [Organizations](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/understanding-organizations) hold Users of a shared hierarchical level.
* [User Groups](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/creating-and-managing-user-groups) hold Users that only share the need to perform the same function.
* [Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/adding-members-to-sites) hold Users (as Site Members) that might need to perform a certain action.
* [Segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments) hold Users of a Site that match certain conditions.

#### [Organization Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions#organization-roles) <a href="#organization-roles" id="organization-roles"></a>

Organization scoped Roles are called Organization Roles. Permissions for Organization Roles are defined at the global level and are applied to one specific [Organization](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/understanding-organizations). Users are added to Organizations individually and are assigned to Organization Roles individually.

#### [Site Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions#site-roles) <a href="#site-roles" id="site-roles"></a>

Site scoped Roles are called Site Roles. Permissions for Site Roles are defined at the global level and are applied to one specific [Site](https://learn.liferay.com/web/guest/w/dxp/site-building/getting-started-with-site-building). Individual Users, Organizations, and User Groups can be used both to control Site Membership and assign Site Roles.

### [Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions#permissions) <a href="#permissions" id="permissions"></a>

Permissions are created by developers of applications. They define actions Users can perform or how much access a User has to a particular asset.

#### [Application Scoped Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions#application-scoped-permissions) <a href="#application-scoped-permissions" id="application-scoped-permissions"></a>

Permissions can be granted on each widget instance that’s placed on a page in Liferay DXP, and each administrative application in the Site Menu → Content and Data section. See [Widget Permissions](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-widgets/configuring-widgets/setting-widget-permissions) for details.

#### [Asset Scoped Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions#asset-scoped-permissions) <a href="#asset-scoped-permissions" id="asset-scoped-permissions"></a>

Asset-level permissions (for instance, permission to edit an individual blog post, or view a folder in the Documents and Media library) are managed from the individual asset, not the Control Panel. See [Widget Permissions](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-widgets/configuring-widgets/setting-widget-permissions) for details.

\
