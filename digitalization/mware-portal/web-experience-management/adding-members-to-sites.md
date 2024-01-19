# Adding Members to Sites

In [Adding Sites](adding-sites.md) you learned the difference between Site Membership Types and about public and private pages within a Site. Now you’ll learn how to add users manually to Sites and how to provide options for self management. For review, there are a few key reasons why Site Membership management is important:

1. Only Site Members can view a Site’s Private Pages.
2. Site Members have more permissions than guests for many widgets like Message Boards and Wikis that enable them to create content and collaborate on your Site.
3. Site Members can be associated with Roles that grant Site privileges.

### Administrating Site Membership <a href="#administrating-site-membership" id="administrating-site-membership"></a>

Administrators can manage Site members from that Site’s _Site Membership_ page.

1. Open _Site Administration_ and select the Site that you want to manage members for.
2. Click on _People_ → _Memberships_

From here you can manage Site Memberships, Organization, and User Group associations. Here you see a list of all of the current Users of the Site and you can add or remove user memberships from the Site.

![Figure 1: The current members of the Site as displayed on the Site Memberships page.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159131/orgs-add-organization-site.png)

#### ADDING MEMBERS TO A SITE <a href="#adding-members-to-a-site" id="adding-members-to-a-site"></a>

Follow these steps to make an existing user a member of the Site:

1. Click the _New_ (![Add User](https://liferay-support.zendesk.com/hc/article\_attachments/360030159111/icon-add.png)) button in the top right of the screen.
2. Use _Filter and Order_ or the _Search_ function to locate the User you want to add to the Site.
3. Select the User(s) you wish and click _Done_.

On the _Assign Users to This Site_ screen, all Users eligible to be added to the Site appear. Deactivated Users do not appear. Site members also appear, but with a greyed-out checkbox.

![Figure 2: The list of users available to add to the current Site. Note that the current members are visible but cannot be added or removed here.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159171/assign-users.png)

#### REMOVING USER MEMBERSHIP FROM A SITE <a href="#removing-user-membership-from-a-site" id="removing-user-membership-from-a-site"></a>

There are two ways to remove a user from a Site. You can remove an individual member like this:

1. Click the _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030190172/icon-actions.png)) icon for the user that you want to remove.
2. Select _Remove Membership_.
3. In the pop-up that appears, confirm the removal.

![Figure 3: Selecting to remove a user.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159151/remove-user.png)

To remove several users at once, you can do this:

1. Click the checkbox for each user that you want to remove.
2. In the menu at the top of the page, click the `X` icon to remove the users from the Site.
3. In the pop-up that appears, confirm the removal.

Removed Users lose access to the Site’s private pages and membership in any Site Roles or Teams they had.

#### ASSIGNING SITE ROLES <a href="#assigning-site-roles" id="assigning-site-roles"></a>

Roles grant permissions in the portal. Roles can be assigned for the entire instance or just for one specific Site or Organization. Site Roles assign permissions for a specific Site.

You can use the same interface options that you used to remove Users from the Site to assign them to Site Roles. If you select a User or Users and click _Assign Site Roles_ (either through the Actions menu or the menu at the top), you are taken to the _Assign Site Roles_ screen. From here:

1. Select the Roles that you want to assign to the selected users.
2. Click _Done_.

![Figure 4: Assigning Site Roles.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159191/assigning-site-roles.png)

Site Roles are created at a global level, but when they’re assigned they only provide privileges for the specific Site where they were assigned. Since Roles are created at a global level, they cannot be created by Site Administrators (since Site Administrators only have Administrator privileges for their Site). **Teams** allow Site Administrators to assign permissions to groups of Users within their Sites. Next, you’ll look at more configuration for managing members of your Site.
