# Sending Alerts and Announcements

You can use the Alerts and Announcements widgets on Site pages to broadcast important information to users. The Alerts widget is designed for displaying high-priority information (e.g. planned downtime alerts, security alerts, etc.). Each alert is therefore labeled with a red _Important_ tag. The Announcements widget displays all other information you want to broadcast on your site. Each announcement therefore lacks the red tag. To separate important alerts from more mundane announcements, you can place the Alerts and Announcements widgets on different pages. However, you can use either widget to display any information you wish. Besides the red tag, they function the same. You can also scope your alerts and announcements to specific groups of users.

![Figure 1: The Alerts widget provides administrators with an easy way to communicate important information to appropriate groups of users.](https://liferay-support.zendesk.com/hc/article\_attachments/360040159172/alerts-widget.png)

These widgets have two tabs:

**Unread:** Non-expired alerts/announcements that you haven’t read.

**Read:** Alerts/announcements that have expired, or that you’ve read.

Click an alert/announcement’s _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360040164331/icon-actions.png)) to edit or delete it.

### Creating Alerts and Announcements <a href="#creating-alerts-and-announcements" id="creating-alerts-and-announcements"></a>

There are two places where you can create alerts and announcements:

1. The _Announcements and Alerts_ app. Access this app at _Control Panel_ → _Configuration_ → _Announcements and Alerts_. Announcements and alerts are in separate tabs in this app. To begin creating an announcement or alert, select the appropriate tab and then click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360040159152/icon-add.png)). This app gives administrators a central location to create announcements and alerts that are then displayed on Site pages by the Announcements and Alerts widgets.
2. The Announcements and Alerts widgets, after adding them to a Site page from the _Add_ (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360040159212/icon-add-app.png)) → _Widgets_ → _News_ menu. To begin creating an announcement or alert, click the widget’s _Add Alert_ or _Add Announcement_ button.

Regardless of where you create the alert or announcement, the form for creating it is the same. Follow these steps to complete the form:

1.  Use the _Title_ field to give the alert or announcement a title. Then create your content in the field _Write your content here…_. For a detailed explanation of the editor, see the [Blogs documentation](https://help.liferay.com/hc/zh-cn/articles/360029041131-Using-the-Blog-Entry-Editor).

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360040164351/alerts-new-alert.png" alt="Figure 2: Enter your alert or announcements title and content." data-size="original">

    Figure 2: Enter your alert or announcement's title and content.
2.  Open the _Configuration_ section of the form and set the following options, if desired:

    **Distribution Scope:** The scope where the alert/announcement is displayed. The default _General_ scope sends the alert/announcements to everyone. Alternatively, you can select your site or specific roles as the scope.

    **URL:** A URL (optional) to include with the alert/announcement. For example, an announcement about a news story could include a link to the news article. The URL must be valid and begin with `http://` or `https://`.

    **Type:** The alert/announcement type. This can be _General_, _News_, or _Test_. Note that each user can specify a different delivery mechanism for each type of alert/announcement.&#x20;

    **Priority:** The announcement’s priority. This can be _Normal_ or _Important_. Note that this is disabled for alerts because alerts are always high priority.

    **Display Date:** The display date of the alert/announcement. This determines when the alert/announcement is sent to users and appears in the widget. By default, the _Display Immediately_ box is checked. This sets the display date equal to the creation date. Uncheck this box to enter a custom display date. For example, administrators can create alerts/announcements for display on a later date. This date can be days, weeks, months, or years in the future. Once the _Display Immediately_ box is unchecked, clicking the Display Date field opens the date-picker.

    **Expiration Date:** The date and time the alert/announcement expires. Once an alert/announcement expires, the widget displays it in the Read tab. Clicking the Expiration Date field opens the date-picker.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360040159232/alerts-new-alert-config.png" alt="Figure 3: Configure your new alert or announcement." data-size="original">

    Figure 3: Configure your new alert or announcement.
3. Click _Save_ when you’re done. Your alert/announcement then appears in the widget.

### User Configuration <a href="#user-configuration" id="user-configuration"></a>

Users can configure how they’d like to receive announcements.

1. Open your user menu and select _Account Settings_.
2. On the _Preferences_ tab, select _Alerts and Announcements Delivery_. This shows options for customizing the delivery of alerts and announcements.
3. Select a configuration for each type of alert/announcement (General, News, or Test). For each type, you can enable delivery by email and SMS (text message). Note that the _Website_ delivery option is selected and grayed out for each alert type. This means that each alert/announcement is always viewable in its respective widget on a site.
4.  Click _Save_ when you’re finished.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360040159192/alerts-delivery.png" alt="Figure 4: Each user can choose how they receive alerts and announcements." data-size="original">

    Figure 4: Each user can choose how they receive alerts and announcements.

### Alert and Announcement Roles <a href="#alert-and-announcement-roles" id="alert-and-announcement-roles"></a>

You can also create roles for users to make general announcements. For instance, if you want someone specific to have strict control over announcements, give that person an Announcements Role. Follow these steps to create a simple Announcements Role:

1. Navigate to _Control Panel_ → _Users_ → _Roles_.
2. With the _Regular Roles_ tab selected, click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360040159152/icon-add.png)). This opens the _New Role_ form.
3. Name your Role _Announcements_, give it a description, and click _Save_.
4.  Select the Role’s _Define Permissions_ tab then grant these permissions:

    * In _Control Panel_ → _General Permissions_, select _Add General Announcements_.
    * In _Site Administration_ → _Applications_ → _Announcements_, select all the resource permissions.

    Click _Save_ after selecting each permission. These permissions let the Role add alerts and announcements.

Now you have a simple Announcements Role that can manage your site’s general announcements. Of course, you can adjust this Role’s permissions as needed.
