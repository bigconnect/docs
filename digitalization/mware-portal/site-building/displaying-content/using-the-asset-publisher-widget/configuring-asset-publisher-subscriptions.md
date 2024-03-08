# Configuring Asset Publisher Subscriptions

The Asset Publisher supports two kinds of subscriptions: email subscriptions and RSS feed subscriptions.

### [Email Subscriptions](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions#email-subscriptions) <a href="#email-subscriptions" id="email-subscriptions"></a>

Users can subscribe to the Asset Publisher to receive email notifications when new assets are published. You must enable this notification first. Follow these steps:

1. Hover over the Asset Publisher and click the Options icon (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) in the widget’s menu and select _Configuration_.
2. Toggle the _Enable Email Subscription_ selector to Yes.
3.  Fill in the form and click _Save_ to apply the changes.

    ![An email subscription notifies users when new assets are published.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions/images/01.png)
4. Enable the _Subscribe_ feature under the Display Settings tab if it’s not enabled. Users can click the _Subscribe_ button to receive email notifications of newly published assets.

![Enabling Email Subscription adds a Subscribe link to the Asset Publisher.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions/images/02.png)

#### [Configuring the Asset Check Interval](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions#configuring-the-asset-check-interval) <a href="#configuring-the-asset-check-interval" id="configuring-the-asset-check-interval"></a>

Liferay Portal periodically checks for new assets and sends emails to subscribed users informing them about the new assets. By default, assets are checked every twenty-four hours. You can change the check interval through System Settings. Follow these steps:

1. Open the Product Menu and go to _Control Panel_ → _Configuration_ → _System Settings_.
2. Select _Assets_ under the _Content and Data_ heading.
3. Go to _System Scope_ → _Asset Publisher_.
4. Change the _Check Cron Expression_ or _Check Interval_ setting to the interval (in hours) that you want to check for new assets and notify subscribed users, and click _Save_ to apply the changes.

![The Check Interval settings specifies how often assets are checked for updates.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions/images/03.png)

WARNING

If you require this feature to be disabled, [blacklist](https://learn.liferay.com/web/guest/w/dxp/system-administration/installing-and-managing-apps/managing-apps/blacklisting-osgi-components) the component. Do not enter zero or a negative number for the interval.

### [RSS Feed Subscriptions](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions#rss-feed-subscriptions) <a href="#rss-feed-subscriptions" id="rss-feed-subscriptions"></a>

NOTE

RSS feeds are deprecated for Liferay Portal 7.2+ and are disabled by default. To leverage RSS feeds, you must enable this feature.

To enable RSS feed subscriptions for the Asset Publisher, follow these steps:

1. Open the Product Menu and go to _Control Panel_ → _Configuration_ → _System Settings_.
2. Select _Web Content_ under the _Content and Data_ heading.
3.  Under the _System Scope_ → _Administration_ tab, check the _Show Feeds_ box. For more information on deprecated apps, see [this article](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/upgrading-liferay/reference/maintenance-mode-and-deprecations-in-7-4).

    ![Enable RSS feeds through System Settings.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions/images/04.png)
4. Navigate back to the Asset Publisher widget, hover over it and click the Options icon (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) in the widget’s menu and select _Configuration_.
5. Toggle the _Enable RSS Subscription_ selector to Yes.
6.  Fill in the form and click _Save_ to apply the changes.

    ![An RSS subscription sends RSS feeds to subscribers' RSS readers.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions/images/05.png)
7. Enable the _Subscribe_ feature under the Display Settings tab if it’s not enabled. Users can click the _RSS_ link to subscribe to the RSS feed.

![Enabling RSS Subscription adds a RSS link to the Asset Publisher.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-subscriptions/images/06.png)
