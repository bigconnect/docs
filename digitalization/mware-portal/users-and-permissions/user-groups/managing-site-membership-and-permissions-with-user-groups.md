# Managing Site Membership and Permissions with User Groups

You can use User Groups to manage Site membership, Roles, and permissions.

### [Managing Site Membership with User Groups](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/managing-site-membership-and-permissions-with-user-groups#managing-site-membership-with-user-groups) <a href="#managing-site-membership-with-user-groups" id="managing-site-membership-with-user-groups"></a>

One of the primary use cases for User Groups is to manage Site membership. When you assign a User Group to a Site, that group’s Users instantly become members of that Site.

![The User Groups tab in Memberships shows the User Groups currently assigned to the Site.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/user-groups/managing-site-membership-and-permissions-with-user-groups/images/02.png)

Follow these steps to assign a User Group to a Site:

1. Open the Menu (![Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)), select the Site you want to work in, then open its Site Administration menu.
2.  In the Site Administration menu, select _People_ → _Memberships_. This opens the Site Memberships screen.

    ![Select Memberships from the People menu.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/user-groups/managing-site-membership-and-permissions-with-user-groups/images/01.png)
3. In Memberships, select the _User Groups_ tab. This tab displays any User Groups currently assigned to the Site.
4. Click the _Add_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)), select any User Groups you want to assign to the Site, then click _Done_. The User Groups you selected now appear in the User Groups tab.

### [Managing Site Permissions with User Groups](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/managing-site-membership-and-permissions-with-user-groups#managing-site-permissions-with-user-groups) <a href="#managing-site-permissions-with-user-groups" id="managing-site-permissions-with-user-groups"></a>

[Creating and Managing User Groups](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/creating-and-managing-user-groups) describes how to assign a Regular Role to a User Group for Users who must have permissions across the portal. User Groups are often created so the group’s Users can act only in one Site. Just as with Regular Roles, it starts by assigning the needed permissions for that action to a _Site Role_, and then assigning that Role to the User Group.

To grant Site scoped permissions to Users in a User Group,

1. [Create the User Group](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/creating-and-managing-user-groups#creating-a-user-group).
2. Assign the User Group to a Site ([see above](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/user-groups/managing-site-membership-and-permissions-with-user-groups#managing-site-membership-with-user-groups)).
3. Create the Site Role and define its permissions (see [Roles and Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions)).
4. Assign the Role to the User Group.

To assign an existing Site Role to an existing User Group that’s already a member of the Site,

1. Open the Menu (![Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)), select the Site to work in, then open its Site Administration menu.
2. In the Site Administration menu, select _People_ → _Memberships_.
3. In Memberships, select the _User Groups_ tab to see the User Groups currently assigned to the Site.
4.  Click the _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the User Group you want to assign to a Role, and select _Assign Roles_.

    ![Select Assign Roles for the User Group.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/user-groups/managing-site-membership-and-permissions-with-user-groups/images/03.png)
5. In the Assign Roles dialog, select the Role from the list and click _Done_.
