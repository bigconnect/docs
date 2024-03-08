# Adding Members to Sites

Users and sites are central concepts in Liferay DXP. Sites are where all your content and pages are stored, and users access and create that content. While user management is covered in depth in [User Management](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions), there are some configuration options specific to site management and assigning users to sites that are relevant here:

* [Managing Site Membership](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/adding-members-to-sites#managing-site-membership)
* Configuring Users to Automatically Join a Site
* [Creating Teams](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/creating-teams-for-sites) or groups of site members for various site functions

The steps below show how to add users manually to sites and how to provide options for self-management.

### [Managing Site Membership](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/adding-members-to-sites#managing-site-membership) <a href="#managing-site-membership" id="managing-site-membership"></a>

Administrators can manage site members from that site’s Memberships app.

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), click the compass icon(![Compass Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-compass.png)), and select the site you want to manage.
2. Select _People_ → _Memberships_ in the Product Menu. If you don’t see this option, Manual Membership is disabled. To enable Manual Membership, select _Site Settings_ → _Site Configuration_. In the _Details_ tab, toggle _Allow Manual Membership Management_.

From here you can manage site memberships, organizations, and user group associations. You can learn more about those in [Users and Organizations](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions). On this page, you can see a list of all current site members and you can add or remove user memberships from the site.

![The current members of the site are displayed on the site Memberships page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-membership/adding-members-to-sites/images/01.png)

#### [Adding Members to a Site](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/adding-members-to-sites#adding-members-to-a-site) <a href="#adding-members-to-a-site" id="adding-members-to-a-site"></a>

Follow these steps to make an existing user a member of the site:

1. Click _Add_ (![Add User](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) in the top right of the screen.
2. Use _Filter and Order_ or the _Search_ function to locate the user you want to add to the site.
3. Select the user(s) you want to add and click _Done_.

On the _Assign Users to This Site_ screen, all users eligible to be added to the site appear. Deactivated users do not appear. Site members also appear, but with a greyed-out checkbox.

![You can view the list of users that are members of the site. Note that the current members are visible but cannot be added or removed here.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-membership/adding-members-to-sites/images/02.png)

#### [Removing User Membership from a Site](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/adding-members-to-sites#removing-user-membership-from-a-site) <a href="#removing-user-membership-from-a-site" id="removing-user-membership-from-a-site"></a>

There are two ways to remove a user from a site. To remove an individual member, follow these steps:

1. Click the _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) icon for the user that you want to remove.
2. Select _Remove Membership_.
3. In the pop-up that appears, confirm the removal.

![You can remove individual members from a site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-membership/adding-members-to-sites/images/03.png)

To bulk remove users from a site, follow these steps:

1. Click the checkbox for each user that you want to remove.
2. In the menu at the top of the page, click the `X` icon to remove the users from the site.
3. In the pop-up that appears, confirm the removal.

Removed users lose membership in any site roles or teams they had.

#### [Assigning Site Roles](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/adding-members-to-sites#assigning-site-roles) <a href="#assigning-site-roles" id="assigning-site-roles"></a>

Roles grant permissions in Liferay Portal. Roles can be assigned for the entire instance or just for one specific site or organization. Site roles assign permissions for a specific site. Learn more about roles in [Roles and Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions).

Follow these steps to assign site roles to users:

1. Select a user or users and click _Assign Roles_ (either through the Actions menu or the menu at the top). This takes you to the _Assign Roles_ screen.
2. Select the roles you want to assign to the selected user(s).
3.  Click _Done_.

    ![You can assign site roles to users.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-membership/adding-members-to-sites/images/04.png)

### [Joining Sites with the My Sites Application](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/adding-members-to-sites#joining-sites-with-the-my-sites-application) <a href="#joining-sites-with-the-my-sites-application" id="joining-sites-with-the-my-sites-application"></a>

The My Sites application lists the sites a user belongs to. This application is added to the User Profile Menu [Dashboard pages](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/personal-sites) by default. You can view the available open and restricted sites by adding the My Sites application to a page and accessing the _All Sites_ tab. You can request access to any of the sites you’re not a member of by selecting the site’s _Options_ button (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and clicking _Join_.

![The My Sites Application displays the sites you're a member of and the ones you can join.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-membership/adding-members-to-sites/images/05.png)

\
