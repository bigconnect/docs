# Assigning Roles to User Segments

User segments are dynamically assigned user collections. If a segment can be well-defined with the available criteria, the administrative overhead of user management can be greatly reduced. On top of this, you can define personalized experiences for user segments created at the site scope, ensuring that users see the most relevant content in your site.

| Where do I create user segments?                                  | What are they for?                                              | Where do I assign roles to a segment?         |
| ----------------------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------- |
| Site Segments (Site Menu → People → Segments)                     | Create personalized experiences for a site’s users.             | Site Menu → People → Segments (Actions)       |
| Instance Segments (Control Panel → Users → Roles (Regular Roles)) | Efficiently and dynamically manage users and their permissions. | Control Panel → Users → Roles (Regular Roles) |

### Assigning a Regular Role to a User Segment <a href="#assigning-a-regular-role-to-a-user-segment" id="assigning-a-regular-role-to-a-user-segment"></a>

Version 7.2 SP2+

Analytics Administrator, Portal Content Reviewer, Power User, Publications User and any other regular roles you create can be assigned to user segments created at the global scope. To assign regular roles to a user segment,

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Control Panel_ → _Users_ → _Roles_.
2.  Click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the regular role and select _Edit_.

    ![Edit a role to assign the user segment to it.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-roles-to-user-segments/images/01.png)
3.  Select _Assignees_ → _Segments_ tab, and click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to select a user segment.

    ![Go to the Segments tab under Assignees to assign the role.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-roles-to-user-segments/images/02.png)
4.  Check the box next to an existing user segment and click _Add_ to assign the role to it. If the user segment doesn’t exist, click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to create a new user segment.

    ![Check the box next to the user segment you want to assign the role to.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-roles-to-user-segments/images/03.png)

The selected user segment(s) appear(s) in the list of assignees for the role.

![The user segment appears under the list of assignees for the role.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-roles-to-user-segments/images/04.png)

### Assigning a Site Role to a User Segment <a href="#assigning-a-site-role-to-a-user-segment" id="assigning-a-site-role-to-a-user-segment"></a>

Version 7.2 SP3+

Site roles can be assigned to segments, with two exceptions: Site Administrator and Site Owner.

1. Open the Product Menu (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _People_ → _Segments_.
2. Open the segment’s _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and click _Assign Site Roles_.
3.  Assign site roles to the segment, then click _Add_.

    ![Site roles can be assigned to segments.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/roles-and-permissions/assigning-roles-to-user-segments/images/05.png)
