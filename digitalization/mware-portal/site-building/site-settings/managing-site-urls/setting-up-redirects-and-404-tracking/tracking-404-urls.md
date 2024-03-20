# Tracking 404 URLs

With the Redirection application, you can view and manage site requests leading to 404 errors. While enabled, the portal catalogs each error, so you can assess the cause of the errors and quickly resolve any issues. These URLs are preserved in your database according to your Redirection configurations.

![You can view and manage 404 URLs.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/tracking-404-urls/images/01.png)

IMPORTANT

For version 7.4+, 404 tracking is disabled by default. To use this feature, you must first enable 404 tracking. This enables both 404 tracking and the 404 URLs tab in the Redirection application.

### Viewing 404 URLs <a href="#viewing-404-urls" id="viewing-404-urls"></a>

You can filter cataloged requests by both _type_ and _date_. When filtering by type, you can choose to display _Active_, _Ignored_, or _All_ 404 URLs. When filtering by date, you can determine whether to view all URLs, or only those that have been requested within the last day, week, or month. You can also order cataloged requests by their _modified date_ or _number of requests_.

By default, this page displays all active URLs by number in descending order.

![You can filter and order cataloged 404 URLs.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/tracking-404-urls/images/02.png)

### Ignore/Unignore 404 URLs <a href="#ignoreunignore-404-urls" id="ignoreunignore-404-urls"></a>

Define a URL as active or ignored by clicking its _Actions_ button (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and selecting _Ignore_/_Unignore_. When ignored, the URL is hidden in the default Active view, though you can change its status at any time.

![You can determine whether a URL is active or ignored.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/tracking-404-urls/images/03.png)

### Creating Redirects for 404 URLs <a href="#creating-redirects-for-404-urls" id="creating-redirects-for-404-urls"></a>

Create an alias redirect for a 404 URL by clicking its _Actions_ button (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and selecting _Create Redirect_. This shows a form with the source URL pre-filled with the selected 404. Finish filling out the form and click _Create_ to save the new redirect.

![Create redirects for 404 URLs.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/tracking-404-urls/images/04.png)

### Configuring 404 Settings <a href="#configuring-404-settings" id="configuring-404-settings"></a>

You can configure how long 404 URLs are retained between requests, the maximum number of 404 URLs stored in your database, and the interval between 404 URL checks.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _System Settings_ → _Pages_ → _Redirection_ under Virtual Instance Scope.

By default, the 404 URL Maximum Age is set to 30 days, the Maximum Number of 404 URLs is set to 1000 entries, and the system checks for 404 URLs every 24 hours.

![Determine how long and how many 404 URLs are retained](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/tracking-404-urls/images/05.png)

### Enabling 404 Tracking <a href="#enabling-404-tracking" id="enabling-404-tracking"></a>

Available 7.4+

1.  Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _System Settings_ → _Pages_ → _Redirection_.

    ![Check Enabled and click Save.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/tracking-404-urls/images/06.png)
2. Check _Enabled_.
3. Click _Update_.

Once enabled, you can access the 404 URL management tab in the Redirection application.

IMPORTANT

Enabling or disabling the application does not affect data stored in the database. If disabled, you can reenable the feature to access your data at any time.

\
