# Using File Expiration and Review Dates

With the Documents and Media application, you can set expiration and review dates for files. This can help you manage any digital assets that are only valid for a limited period of time (e.g., due to licensing). When [enabled](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates#enabling-expiration-and-review-dates), the application checks for any files that have reached their expiration or review date. This check occurs at [configurable intervals](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates#configuring-the-check-interval), according to the Documents and Media system setting.

If a file has reached its expiration or review date, a [notification email](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates#configuring-notification-emails) is automatically sent to file owners and reviewers, as well as any users subscribed to the folder containing the file.

Additionally, when a file expires, it is deactivated and cannot be used or edited in Liferay DXP. To reactivate the file, you must change its expiration date, or disable file expiration. While expired, the asset cannot be accessed in other applications (e.g, Web Content, Blogs) or via URL.

By contrast, when a file requires review, it remains available for use. Reaching the file’s review date only triggers user notifications and does not affect the file in any way.

### [Enabling Expiration and Review Dates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates#enabling-expiration-and-review-dates) <a href="#enabling-expiration-and-review-dates" id="enabling-expiration-and-review-dates"></a>

By default, files are set to never expire or require review. However, you can enable and set expiration and review dates when uploading or editing files.

When uploading or editing a single file, these settings appear as follows.

![Enable or disable expiration and review for an individual file.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates/images/01.png)

When uploading multiple files, expiration and review settings appear as follows.

![Enable or disable expiration and review for multiple files.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates/images/02.png)

To activate expiration or review, simply uncheck the _Never Expire_ or _Never Review_ checkbox. The default review and expiration dates are set to one year from the time of activation. If desired, you can set an alternative date and time.

Liferay’s system only checks for files that have reached their expiration or review date according to the set [check interval](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates#configuring-the-check-interval).

When a date has been reached, relevant users are automatically [notified](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates#configuring-notification-emails). If the file is expired, it is no longer available for use on the platform.

### [Configuring the Check Interval](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates#configuring-the-check-interval) <a href="#configuring-the-check-interval" id="configuring-the-check-interval"></a>

By default, the Documents and Media application is configured to check file expiration and review dates every 15 minutes. However, you can configure this interval via the Control Panel.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click on the _Control Panel_ tab, and go to _System Settings_ → _Document and Media_ → _Service_.
2.  Use the _Check Interval_ field to determine the number of minutes between file checks.

    ![Enter the number of minutes between checks.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates/images/03.png)
3. Click on _Save_ when finished.

Once saved, the system will automatically check for files that have reached their expiration or review date according to the set interval.

WARNING

If you require this feature to be disabled, [blacklist](https://learn.liferay.com/web/guest/w/dxp/system-administration/installing-and-managing-apps/managing-apps/blacklisting-osgi-components) the component. Do not enter zero or a negative number for the interval.

### [Configuring Notification Emails](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates#configuring-notification-emails) <a href="#configuring-notification-emails" id="configuring-notification-emails"></a>

When a file expires or requires review, a notification email is sent to the file’s owners, reviewers, or any users subscribed to the folder containing the file. Documents and Media provides a default notification message, but you can edit the message if desired.

1. Navigate to the _Documents and Media_ application in a Site or Asset Library.
2. Click on the Actions button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar, and select _Configuration_.
3.  In the modal window, click on the _Documents Needs Review Email_ or _Document Expired Email_ tabs.

    ![Edit the notification emails in the Configuration modal window.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/using-file-expiration-and-review-dates/images/04.png)
4. Draft the desired notification message.
5. Click on _Save_ when finished.

\
