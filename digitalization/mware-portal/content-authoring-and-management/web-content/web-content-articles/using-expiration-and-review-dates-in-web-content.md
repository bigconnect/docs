# Using Expiration and Review Dates in Web Content

You can set expiration and review dates for web content articles to manage time-sensitive content. When enabled, file owners, reviewers, or folder subscribers are [notified via email](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/using-expiration-and-review-dates-in-web-content#configuring-notification-emails) of articles that have reached their expiration or review date at configurable intervals, based on the Web Content system setting.

When an article expires, it is deactivated and cannot be used or edited. To reactivate the article, you must change its expiration date or disable article expiration. While expired, the article cannot be accessed.

By contrast, when an article requires review, it remains available for use. Reaching the article’s review date only triggers user notifications and does not affect the article’s availability.

### Enabling Expiration and Review Dates <a href="#enabling-expiration-and-review-dates" id="enabling-expiration-and-review-dates"></a>

By default, web content articles never expire or require review. You can change this, however, when creating or editing articles.

![Enable or disable expiration and review for an individual article.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-articles/using-expiration-and-review-dates-in-web-content/images/01.png)

When editing multiple articles, expiration is done through the toolbar or action menu.

![You can select multiple articles and then expire them through the toolbar.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-articles/using-expiration-and-review-dates-in-web-content/images/02.png)

To activate expiration or review, uncheck the _Never Expire_ or _Never Review_ checkbox. The default review and expiration dates are set to one year from the time of activation. Change the dates if needed.

The portal checks for articles that have reached their expiration or review dates at a specific check interval.

### Configuring the Check Interval <a href="#configuring-the-check-interval" id="configuring-the-check-interval"></a>

By default, Web Content checks article expiration and review dates every 15 minutes. You can customize this.

1. Click _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → _Control Panel_ → _System Settings_ → _Web Content_ → _Web Content_.
2.  Use the Check Interval field to set the minutes between article checks.

    ![Enter the number of minutes between checks.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-articles/using-expiration-and-review-dates-in-web-content/images/03.png)
3. Click _Save_ when finished.

Now the system checks for expiration or review dates according to the interval you set.

WARNING

If you require this feature to be disabled, blacklist the component. Do not enter zero or a negative number for the interval.

### Configuring Notification Emails <a href="#configuring-notification-emails" id="configuring-notification-emails"></a>

When an article expires or requires review, the portal sends a notification email to the article’s owners, reviewers, or any users subscribed to the folder containing the article. There’s a default notification message, but you can customize it.

1. Navigate to the Web Content application in a site or asset library.
2. Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar and select _Configuration_.
3.  In the modal window, choose the _Web Content Review Email_ or _Web Content Expired Email_ tabs.

    ![Edit the notification emails in the Configuration modal window.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-articles/using-expiration-and-review-dates-in-web-content/images/04.png)
4. Draft the desired notification message.
5. Click _Save_ when finished.

\
