# Users

Every person who accesses a site is considered a user. Unauthenticated users are considered _Guest_ users. The portal ships out-of-the-box with a default Admin user who has complete control of and access to the system. Roles and Permissions govern what users are able to see and do on a site.

Users break down into three general categories:

| User          | Default Access                                                                                                           | Notes                                                                                                |
| ------------- | ------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| Administrator | The portal Admin with full system access.                                                                                | We _strongly_ recommend that users change the password for this account promptly after installation. |
| Guest         | Unauthenticated user with view access to publicly accessible pages as well as limited ability to create and add content. | By default, Guest users can create an account on a site to view and interact with sites.             |
| User          | Authenticated user able to view restricted pages and create content.                                                     | Users get defined default permissions and can be elevated to organization or site administrators.    |

### [Managing Users](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/users/understanding-users#managing-users) <a href="#managing-users" id="managing-users"></a>

To add and manage users, open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Users and Organizations_.

![Add and manage users via the Users and Organizations application.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/users/understanding-users/images/01.png)

Here, you can manage each user’s metadata (name, department, etc.), permissions, pages, and activation status.

You can also filter, sort, search, and change the view layout for users using the controls available.

When filtering users, you can select _All_, _Inactive_, or _Active_ users. There’s also an option to _FILTER BY DOMAIN_ where you can choose between users that belong to an account or organization.

Version 2024.Q1+ There’s a new option under the _FILTER BY DOMAIN_ section to filter _Unassociated Users_. These are users that don’t belong to an account or organization.

#### Adding Users <a href="#adding-users" id="adding-users"></a>

Administrators can add users through the Users and Organizations section of the Control Panel or through the API. New users can be created by administrators, can register themselves if open registration is configured, or can be synced from an LDAP server. See Adding and Managing Users to learn more.

**Open Registration**

By default, guest users can create user accounts.

See Configuring Authentication to learn more about configuring registration and authentication for users.

**Configuring a User Directory**

Organizations that manage users with LDAP can sync their LDAP server with the portal to import users. See Connecting to a User Directory to learn more.

### Managing User Access <a href="#managing-user-access" id="managing-user-access"></a>

User access is controlled by roles and permissions. Permissions define what a user can and cannot do. Roles are groupings of permissions. Permissions are configured by defining permissions for a role and assigning users to pre-set roles. To learn more about managing access for users, see Roles and Permissions.

### Organizing Users <a href="#organizing-users" id="organizing-users"></a>

The portal has several tools to help you organize and administer users.

Organizations can group users in a distributed hierarchy. Practically, this means that large organizations can empower and delegate users to administer their organizations.

![Create Organizations to organize users.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/users/understanding-users/images/02.png)

User groups are another way to group users to simplify administration and assigning Roles. User groups are simple lists of users that can have roles assigned to them.

\
