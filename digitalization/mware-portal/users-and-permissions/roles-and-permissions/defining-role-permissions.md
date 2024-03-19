# Defining Role Permissions

Roles collect permissions, so when Users are given a Role, they inherit all Role’s permissions.

If you create a Role with permission to access something in the Control Panel, the _View Control Panel Menu_ permission is automatically granted.

To determine the permissions to carry out an action within Liferay DXP, iteration is necessary. Add one or two permissions, then test the Role to ensure the behavior you see is what you expected. As you create more Roles, you’ll better understand how to combine permissions to accomplish a given task.

TIP

Assigning a User to the Role and then [Impersonating the User](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/users/adding-and-managing-users#impersonating-users) is a good way to see what permissions the User has inherited from the Role.

To define permissions for any Role in Liferay DXP,

1. Go to the Control Panel and then click on _Users_ → _Roles_.
2. Click on the Regular, Site, Organization, or Asset Library Role, or [create a new one](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/creating-and-managing-roles).
3. From the Role Editor screen, click the _Define Permissions_ tab.
4. Assemble the permissions for the Role and click _Save_.

![Define the permissions of a Role so its Users can carry out their tasks.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions/images/04.png)

Of course, the bulk of the work is in choosing the permissions for the Role. Use the Search bar to find the permissions you need.

NOTE

The Roles application in the Control Panel is the main, but not the only place where permissions are configured. You can configure a Role’s permissions on a resource at a more granular level. For example, you can configure permissions on a particular widget from its Permissions menu. Permissions granted or removed in the Control Panel override permissions defined at the more granular level.

### [Example: Defining a User Group Manager](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions#example-defining-a-user-group-manager) <a href="#example-defining-a-user-group-manager" id="example-defining-a-user-group-manager"></a>

In order to create a new role that can manage User Groups in a Virtual instance, the permissions for a User Group Manager Role must be defined so that assigned [Role Users](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/assigning-users-to-roles) can act on all User Groups in a Virtual Instance:

* Add Users to any User Group.
* Remove Users from any User Group.

Once a Regular Role with the title _User Group Manager_ is created, go to its Define Permissions screen, then

1.  Drill down in the menu on the left to _Control Panel_ → _Users_ → _User Groups_.

    Alternatively, filter the permissions menu by entering _user groups_ in the search bar.

    ![Use the search bar to filter the list of permissions to the relevant ones.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/defining-role-permissions/images/01.png)
2. Under the _General Permissions_ heading, flag _Access in Control Panel_ and _View_. This lets user group managers access the User Groups Control Panel portlet and view existing User Groups.
3. Since you want to enable User Group managers to view User Groups and assign members to them, also check the _Assign Members_ and _View_ permissions under the _Resource Permissions_ → _User Group_ heading.
4. Click _Save_.

![Users assigned to the User Group Manager Role can Add available Users to any User Group, as well as remove them.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/defining-role-permissions/images/02.png)

Now the User Group Manager Role has all the permissions necessary for adding Users to User Groups. User Group managers can view User Groups, assign members, and access User Groups in the Control Panel. The permission to view Users in the Control Panel was necessary because you must view Users to assign them as members of a Role. Without this permission, User Group managers see an empty list if they try to add Users to a Role.

### [Permissions Overview](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions#permissions-overview) <a href="#permissions-overview" id="permissions-overview"></a>

There are three categories of permissions:

**Control Panel:** Portal Administrators can access the entire Control Panel. Control Panel permissions affect whether and how the Control Panel appears to different Role Users. For example, you can fine-tune whether a Role grants view access to various applications in the Control Panel, or get more granular and only grant access to an application’s Configuration actions.

**Site and Asset Library Administration:** Site Administrators can access the site administration tools belonging to the Site Administration category. The permissions in the Site and Asset Library Administration → Applications categories govern the content that can be created by applications such as the Wiki and Message Boards. If you pick one of the applications from this list, you see options for defining permissions on its content. For example, if you pick Message Boards, you see permissions for creating categories and threads or deleting and moving topics.

Site application permissions affect the application as a whole, throughout the scope of the Role (e.g., all Sites in the Virtual Instance for Regular Roles). For example, granting Message Boards application permissions to add the Message Boards widget to a page for an Organization Role means that Organization Users with this Role can take the action on any page in any Site owned by the [Organization](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/understanding-organizations).

**User:** By default, Users inherit the User Role, and can manage their User accounts via the permissions belonging to the User category. These permissions can be used to define a new Role for Users, to provide fewer permissions on the Users’ accounts than the default User Role. You can then use the [Default Associations](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/assigning-users-to-roles#default-user-associations) configuration to replace the original User Role with your more restrictive Role (perhaps named _Restricted User_ or _Custom User_ to differentiate it).

Don’t limit yourself to just one category: when defining a custom Role you can mix and match permissions from all of these categories.

TIP

You can change the scope of a permission: click the _Change_ link in the _Scope_ column of the permissions table, then choose a new scope. Click _Save_ to see a list of all permissions currently granted to the Role along with their scope. From the Summary view, you can add more permissions or go back to the Role Application default view by clicking on the _Back_ (<) icon.

![You can fine-tune Role permissions by changing the scope of the permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/defining-role-permissions/images/03.png)

You may find that a certain permission grants more or less access than what you expected—always test your permissions configurations!

### [Troubleshooting: Site Roles May Require Global View Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions#troubleshooting-site-roles-may-require-global-view-permissions) <a href="#troubleshooting-site-roles-may-require-global-view-permissions" id="troubleshooting-site-roles-may-require-global-view-permissions"></a>

A Site Role cannot, by definition, define permissions to act on entities outside the Site scope. This can be problematic when defining administrative type permissions in a custom Role. For example, if you try to duplicate the default Site Administrator Role in a Custom Site Administrator Role, you’ll find that adding all the _Site and Asset Library Permissions_ → _People_ → _Membership_ permissions isn’t enough to assign new members to the Site. Users that aren’t already Site Members are not visible to the Custom Site Administrator. To grant the proper permissions, you need a Regular Role on top of the Site Role. To give the Regular Role view permission on the User resource, go to the _Define Permissions_ section of the Roles application, then _Control Panel_ → _Users_ → _Users and Organizations_). Users with the Site Role and the Regular Role can now see the Users in the system and add them to the Site.

NOTE

The built-in Site Administrator Role automatically grants permission to view Users.

### [Delegating Social Activities Configuration](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions#delegating-social-activities-configuration) <a href="#delegating-social-activities-configuration" id="delegating-social-activities-configuration"></a>

There’s a permission that allows Site administrators to delegate to other Users responsibility for configuring social activities. To add this permission to a Role, click _Actions_ next to the desired Role and select _Define Permissions_. Find the _Site Administration_ → _Configuration_ → _Social Activity_ permissions category. Flag all of the permissions and then click _Save_:

* Access in Site Administration
* Configuration
* Permissions
* Preferences
* View

Once these permissions are granted, Role assignees can manage the site’s Social Activities.

Roles group various permissions in whatever combinations you want. This gives you as much flexibility as possible to build the Site you have designed.
