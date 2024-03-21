# Assigning Users to Roles

The location of a Role’s assignment UI corresponds with the Role’s scope.

### Regular Roles <a href="#regular-roles" id="regular-roles"></a>

Globally scoped regular roles are available for configuration in the Control Panel.

| Permission Scope                         | Available Assignments                                                        |
| ---------------------------------------- | ---------------------------------------------------------------------------- |
| Throughout the Virtual Instance (global) | <p>User Groups<br>Organizations<br>Sites<br>Segments<br>Individual Users</p> |

You can assign Users to a Regular Role in the Assignees tab of the Add/Edit Role form.

1. Go to Control Panel → Users → Roles, and click on a Regular Role.
2. In the Assignees tab of the Add/Edit Role form, click the tab for the assignment type you need. Regular Roles are assigned to Users, Sites, Organizations, User Groups, or User Segments.
3. Click the Add button ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png).
4.  Select the assignees and click _Add_.

    If assigning a group, all Users assigned to that group inherit the Role.

![You can assign a Regular Role to an entire Organization of Users.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-users-to-roles/images/05.png)

### Site Roles <a href="#site-roles" id="site-roles"></a>

You can assign Users to a Site Role in the Memberships section of a Site’s People sub-menu.

| Permission Scope | Available Assignments                                                      |
| ---------------- | -------------------------------------------------------------------------- |
| A single Site    | <p>User Groups<br>Organizations<br>Segments<br>Individual Site Members</p> |

1. Go to Site Menu → People → Memberships.
2. Click the tab for the membership type you’re assigning the Role. Site Roles are assigned to Users (i.e., individual Site members), Organizations, or User Groups.
3. Click the Actions button ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) → Assign Roles.
4.  Choose the Roles to assign and click _Done_.

    If assigning a group, all Users assigned to that group inherit the Role.

![Assign a Site Role within the Site Menu's People → Memberships section.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-users-to-roles/images/01.png)

NOTE

As of version 7.2 SP3+, you can assign Site Roles to User Segments. See Assigning Roles to User Segments for more details.

### Organization Roles <a href="#organization-roles" id="organization-roles"></a>

You can assign Users to a Organization Role in the Organizations tab of the Users and Organizations Control Panel section.

| Permission Scope      | Available Assignments |
| --------------------- | --------------------- |
| A single Organization | Individual Users      |

1. Go to Control Panel → Users → Users and Organizations, and click on the _Organizations_ tab.
2. Click the Actions button ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) for the Organization and choose _Assign Organization Roles_.
3. Choose the Role to assign and you see the current assignees listed.
4. To add new assignees, click the _Available_ tab to see the Organization members that can be assigned to the Role.
5. Choose the assignees, then click _Update Associations_.

![Assign an Organization Role within the Organizations section of the Control Panel.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-users-to-roles/images/02.png)

### Asset Library Roles <a href="#asset-library-roles" id="asset-library-roles"></a>

Making Role assignments for Asset Libraries is nearly identical to making assignments for Sites: assign Users to an Asset Library Role in the Memberships section of the Asset Library’s People sub-menu.

| Permission Scope       | Available Assignments                                   |
| ---------------------- | ------------------------------------------------------- |
| A single Asset Library | <p>Organizations<br>User Groups<br>Individual Users</p> |

1. Go to the Asset Library Menu → People → Memberships.
2. Click the tab for the membership type you’re assigning the Role. Like Sites, Asset Libraries are assigned to Users (i.e., individual Site members), Organizations, or User Groups.
3. Click the Actions button ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) → Assign Roles.
4.  Choose the Roles to assign and click _Done_.

    If assigning a group, all Users assigned to that group inherit the Role.

![Assign a Asset Library Role within the Asset Library Menu's People → Memberships section.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-users-to-roles/images/03.png)

### Default User Associations <a href="#default-user-associations" id="default-user-associations"></a>

By default, new Users receive only the Users Role, but you can manage the default Role associations to add new Roles or remove the User assignment.

| Permission Scope                         | Available Assignments |
| ---------------------------------------- | --------------------- |
| Throughout the Virtual Instance (global) | All new Users         |

1. Navigate to Control Panel → Configuration → Instance Settings.
2. Select the Users category under the PLATFORM section.
3. Click _Default User Associations_ from the left menu.
4. Use the Regular Roles configuration field to enter a comma-separated list of Regular Roles that all new Users should inherit.

Read more in the Default User Associations documentation.

![Use Instance Settings to set the default Regular Roles for all new Users.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-users-to-roles/images/06.png)

Assignment is important, but a Role isn’t worth the database row it occupies without permissions. Read about defining permissions for a Role next.
