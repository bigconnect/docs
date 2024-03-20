# Configuring Locked Pages

Liferay locks pages during editing to avoid conflicts when multiple users try to edit the same page.

This feature works with content, master, utility, and collection pages, along with page and display page templates.

### Locking the Page <a href="#locking-the-page" id="locking-the-page"></a>

Whenever a user starts editing a page and adds new content to it, the page locks, preventing other users from accessing it.

If users try to access a locked page, a Page in Use message appears instead. This page shows a Go Back or a Go To \[Previous Page Name] depending on whether the previous page exists.

NOTE

Users can only discard drafts of unlocked pages.

![Trying to access locked pages redirects the user to a generic error page](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-locked-pages/images/01.png)

Not all events lock a page:

* You can still modify a page’s master page or its stylebook without locking the page.
* You can propagate Fragments, even if a page is locked.
* You can import a page’s page template, even if a page is locked.

TIP

If you are working with Publications, the page and its publications are locked and unlocked together.

### Unlocking a Page <a href="#unlocking-a-page" id="unlocking-a-page"></a>

Most of the time, you don’t have to worry about unlocking a page, as the system unlocks it automatically.

For example, when a user changes a page from edit mode to view mode, when the user logs out, or when the user closes the tab with the page, Liferay unlocks the page.

If a user leaves a page open in Edit mode for a long time without making changes, the system automatically unlocks the page. The amount of time can be configured:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and Navigate to _Control Panel_ → _Instance Settings_ under Configuration.
2. Under Content and Data, select _Pages_.
3. Under Virtual Instance Scope, select _Locked Pages_.
4.  Toggle _Allow Automatic Unlocking Process_.

    If you want to disable the feature, turn the feature off.
5. Set a _Time Without Autosave_ in minutes.

WARNING

The Time Without Autosave may not be accurate as the system checks for locked pages every 15 minutes.

![Set the automatic unlocking time in Instance Settings](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-locked-pages/images/02.png)

Site Administrators or Super Administrators can unlock pages manually.

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Configuration_ → _Locked Pages_.
2.  Click _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the locked page you want to unlock → _Unlock_.

    You can select multiple pages and bulk-unlock them by clicking on the Lock (![Lock icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-lock.png)) icon or the Actions button next to it → Unlock.

The page is unlocked. All the autosaved alterations are kept, and you can now edit the page.

TIP

In this list you can also see the type of the page, the user currently editing the page, and the time since the last autosave.

![If you are a Site Administrator or a Super Administrator, you can unlock any page manually](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-locked-pages/images/03.png)

\
