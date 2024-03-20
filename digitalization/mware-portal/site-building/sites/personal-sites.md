# Personal Sites

By default, the portal generates a personal site for every user. Each site includes two [page sets](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages#page-sets): My Profile (public) and My Dashboard (private). Each user is the sole member of their personal site and can use it to display and manage their content.

If desired, you can disable or customize personal sites using portal properties.

IMPORTANT

New portal installations include a release feature flag that disables the creation of private pages, including dashboard pages. To manually create custom dashboard pages for a personal site, private pages must be enabled for your system. See Enabling Private Pages for more information.

TIP

With user group sites, you can dynamically add pages to the personal sites of all group members. See User Group Sites for more information.

### Profile Pages and Dashboard Pages <a href="#profile-pages-and-dashboard-pages" id="profile-pages-and-dashboard-pages"></a>

In profile pages, users can share content (e.g., blogs, activities) publicly. By default, each profile page displays the user’s basic information and provides a download link to the user’s vCard (virtual business card).

![The profile page displays public info for other users to view.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/personal-sites/images/01.png)

In dashboard pages, users can access private files, create personalized RSS feeds, manage site memberships, and more. By default, each dashboard page includes the My Sites widget.

![The dashboard page displays private information that's just for you to view.](<../../../../.gitbook/assets/02 (2).png>)

### Managing Personal Site Pages <a href="#managing-personal-site-pages" id="managing-personal-site-pages"></a>

To access your personal site pages, follow these steps:

1.  Open the _User Profile Menu_ icon and select either _My Profile_ or _My Dashboard_.

    ![You access your personal site pages from the User Profile Menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/personal-sites/images/03.png)
2. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Site Builder_, and click _Pages_.
3. Manage them as you would any other site page.

You can also add and define pages for a user’s personal site via a user group. See User Group Sites for more information.

### Personal Site Permissions and Roles <a href="#personal-site-permissions-and-roles" id="personal-site-permissions-and-roles"></a>

Permissions and roles for personal sites follow the behavior shown below:

* Administrators can customize the modifiable portions of personal sites through portal’s permissions system by removing permissions from roles (disallow all users from modifying something by removing the relevant permission from the user role).
* Users can administer their personal sites and modify the pages and applications when they are members of the power user role.

### Customizing Personal Sites with Portal Properties <a href="#customizing-personal-sites-with-portal-properties" id="customizing-personal-sites-with-portal-properties"></a>

There are several portal properties you can add to `portal-ext.properties` to customize the default pages of personal sites. You can customize the names of the pages, the applications that appear on the pages, the themes and layout templates of pages, and more. Please refer to the Default User Public Layouts and Default User Private Layouts sections of the `portal.properties` file for details. Some of these properties are listed in the table below:

| Functionality                                                 | Property                                        |
| ------------------------------------------------------------- | ----------------------------------------------- |
| Disable personal sites                                        | `layout.user.public.layouts.enabled=false`      |
| Disable private pages on personal sites                       | `layout.user.private.layouts.enabled=false`     |
| Disable automatic creation of personal sites                  | `layout.user.public.layouts.auto.create=false`  |
| Disable automatic creation of private pages on personal sites | `layout.user.private.layouts.auto.create=false` |

The public and private page sets of personal sites are handled separately. You can leave one page set enabled while disabling the other. If you initially had user personal sites enabled for your instance but then disabled them, existing personal sites remain on your instance until the next time users sign in, at which point they’re removed.

\
