# Publications Permissions

Publications works with the portal’s permissions framework, so you can assign application and resource permissions to regular roles. You can also assign permissions scoped to individual publications when inviting collaborators. Only permitted users can access publications and perform actions on its resources.

NOTE

Publications permissions do not include page and content editing privileges. Publications users require explicit permissions for the applications and resources they’re expected to act upon (e.g., web content, blogs, pages).

By default, the portal provides the _Publications User_ role with basic permissions for creating and accessing publications.

![MWARE Portal provides the default Publications User role.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/publications-permissions/images/01.png)

If needed, you can create additional regular roles or configure existing roles for content creators, reviewers, and publishers. See Creating and Managing Roles and Defining Role Permissions for more information.

![Add additional regular roles or configure other role permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/publications-permissions/images/02.png)

NOTE

You can only assign Publications permissions to _regular roles_. See Understanding Roles and Permissions for more information.

### Application Permissions <a href="#application-permissions" id="application-permissions"></a>

Application permissions grant access to the Publications application itself and do not include resource permissions.

| Permission              | Description                                                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Access in Control Panel | Access the Publications application in the Global Menu ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ).                               |
| Add to Page             | N/A                                                                                                                                                                                                  |
| Configuration           | [Enable or disable](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/enabling-publications) Publications and the Sandbox Only mode.                             |
| Permissions             | View and modify permissions for the Publications application.                                                                                                                                        |
| Preferences             | N/A                                                                                                                                                                                                  |
| View                    | View the Publications application in the Global Menu ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ) and access the Publications Bar. |
| Work on Production      | Access and work in production while Publications is enabled.                                                                                                                                         |

At minimum, using Publications requires a regular role with the Access in Control Panel and View permissions.

IMPORTANT

If you’ve enabled Sandbox Only and don’t want users to work in production, ensure they do not have the Work on Production permission.

### Resource Permissions <a href="#resource-permissions" id="resource-permissions"></a>

Resource permissions grant access to view and act on resources in the Publications application. Some of these permissions relate to creating publications, while others are for performing actions on existing publications (e.g., edit, delete, publish).

#### Publications (Creating Publications) <a href="#publications-creating-publications" id="publications-creating-publications"></a>

| Permission          | Description                                                                                                                                                                                                                                                               |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add Publication (1) | Create a publication. Without this permission, users can only access and contribute to a publication when [invited](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/collaborating-on-publications#inviting-users-to-a-publication). |
| Permissions (2)     | View and modify permissions related to creating publications: Add Publication and Permissions.                                                                                                                                                                            |

Users with these permissions can perform these actions in the Publications application.

![Users can add a publication and manage permissions for adding publications.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/publications-permissions/images/03.png)

#### Publication (Acting on Existing Publications) <a href="#publication-acting-on-existing-publications" id="publication-acting-on-existing-publications"></a>

| Permission  | Description                                                                                                                           |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Delete      | Delete publications you can view.                                                                                                     |
| Permissions | View and modify permissions for publications you can view. This includes the ability to invite users to a publication.                |
| Publish     | Publish publications you can view.                                                                                                    |
| Update      | Update publications you can view; this includes the ability to work in a publication and edit the publication’s name and description. |
| View        | View all publications in the portal instance; this includes the ability to review each publication’s changes.                         |

Users with these permissions can perform actions on existing publications.

![Users can view, update, publish, delete, and manage permissions on existing publications.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/publications-permissions/images/04.png)

### Publications User Role <a href="#publications-user-role" id="publications-user-role"></a>

By default, the _Publications User_ role has these permissions:

* Portal: View Control Panel Menu
* Publications: Access in Control Panel
* Publications: View
* Publications > Publications: Add Publication

![MWARE Portal provides the default Publications User role with these permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/publications-permissions/images/05.png)

### Assigning Roles to Publication Collaborators <a href="#assigning-roles-to-publication-collaborators" id="assigning-roles-to-publication-collaborators"></a>

By default, publication creators are assigned the _owner_ role automatically and can perform all actions on their publication. When inviting users to their publication, owners can assign these publication roles:

| Publication Role | View | Update | Publish | Permissions | Delete |
| ---------------- | ---- | ------ | ------- | ----------- | ------ |
| Viewer           | ✔    | ✘      | ✘       | ✘           | ✘      |
| Editor           | ✔    | ✔      | ✘       | ✘           | ✘      |
| Publisher        | ✔    | ✔      | ✔       | ✘           | ✘      |
| Admin            | ✔    | ✔      | ✔       | ✔           | ✘      |

![When inviting users to a publication, you can assign a role scoped to that publication.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/publications-permissions/images/06.png)

Each publication role is scoped to the current publication and does not grant permissions in other publications.

Publication roles do not restrict regular role permissions. For example, if users with the delete permission are invited to a publication and assigned the _viewer_ role, they can delete the publication because of their regular role permissions.

\
