# User Group Sites

Each User has a personal Site that consists of public (Profile) and private (Dashboard) pages. A _User Group Site_ determines the base pages of the User Group members’ personal Sites. If the User Group Site pages are added to a User’s Profile pages, then the User Group Site is a public Site, accessible to anyone with the URL (`http://www.[sitename].com/web/[username]`). If the User Group Site pages are added to the user’s Dashboard pages, then the User Group Site is a private Site. A mixed approach can also be used, where both private and public pages are added for the User Group Site. If Users belong to multiple User Groups, all the pages from those User Group Sites are made part of their personal Sites.

When creating a user group, you can create the user group Site via the Site Templates available for selection in the _My Profile_ and _My Dashboard_ selector menus. You can also create a User Group Site later, either manually or via a Site Template.

### Creating User Group Sites From Site Templates <a href="#creating-user-group-sites-from-site-templates" id="creating-user-group-sites-from-site-templates"></a>

Follow these steps to create a User Group Site from a Site Template, for a User Group that already exists:

1. Open the Menu (![Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360030187952/icon-menu.png)) and select _Control Panel_ → _Users_ → _User Groups_. User groups appear in a table.
2. Click _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030157051/icon-actions.png)) → _Edit_ for the User Group you want to create a Site for. This opens a form that you can use to edit the User Group. Note that this is the same form that appears when creating a user group.
3. To use a Site Template to create a public profile for the Users on their _My Profile_ Site, select that Site Template from the _My Profile_ menu. To use a Site Template to create private pages for the Users on their _My Dashboard_ Site, select that Site Template from the _My Dashboard_ menu. Note that you can also do both.
4. Click _Save_.

Now, when one of the group’s Users navigates to their _My Profile_ or _My Dashboard_ Sites, the content of those Sites reflect the Site Template(s) you selected.

User Group Site pages function similarly to regular Site Template pages, with an important exception: User Group Site pages aren’t copied for each user. They’re shown dynamically along with any custom pages that Users may have on their personal Site. For this reason, Users can’t modify pages inherited from the User Group. If needed, the User Group administrator can define certain areas of a page as customizable, like with regular Sites. This lets Users add and configure widgets in the specified area of the page.

This flexibility lets you achieve almost any desired configuration for a User’s personal Site without having to modify it directly. When Users are assigned to a User Group, they’ll immediately have access to the User Group’s Site pages from their personal Sites.

**Note:** Site Templates have an option that propagates changes made to the Site Template. If you use a Site Template with this option enabled, the User Group Sites update automatically when that template changes. If you disable this option but enable it again later, the template’s pages are copied to the Users’ Sites, overwriting any changes they may have made. For more information on the automatically propagating Site Template changes, see Site Templates.

### Creating User Group Sites Manually <a href="#creating-user-group-sites-manually" id="creating-user-group-sites-manually"></a>

You can create a User Group’s Site manually, instead of basing it on a Site Template. Follow these steps:

1. Open the Menu (![Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360030187952/icon-menu.png)) and select _Control Panel_ → _Users_ → _Users Groups_. User groups appear here.
2. Click _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030157051/icon-actions.png)) → _Manage Pages_ for the user group you want to create a Site for. This opens the _Pages_ window. Note that this is the same window you use for creating pages.
3. Create the public and/or private pages that you want to use for the Users’ _My Profile_ and/or _My Dashboard_ Sites. Public pages you create here become pages on users’ _My Profile_ Site, while private pages become pages on users’ _My Dashboard_ Site.

When you return to User Groups in the Control Panel, you can access a User Group’s public and/or private pages via these links in the User Group’s _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030157051/icon-actions.png)):

* **Go to Profile Pages:** Opens the User Group’s public _My Profile_ page(s) in a new browser window.
* **Go to Dashboard Pages:** Opens the User Group’s private _My Dashboard_ page(s) in a new browser window.

In the new window, you can add more pages and portlets and configure Site settings.
