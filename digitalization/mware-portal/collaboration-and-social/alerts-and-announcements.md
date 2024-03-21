# Alerts and Announcements

You can use the Alerts widget or the Announcements widget to broadcast important information.

The Alerts widget is designed for displaying high-priority information (e.g. planned downtime alerts, security alerts, etc.). Each alert is labeled with a red _Important_ tag.

![Example of an alert with the red important tag.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/alerts-and-announcements/sending-alerts-and-announcements/images/01.png)

The Announcements widget can display all other information you might want to broadcast. Each announcement lacks the red tag. To separate important alerts from more mundane announcements, you can place the Alerts widget and Announcements widget on different pages. You can use either widget, however, to display any information you wish.

Besides the red _Important_ tag, they function the same. You can also scope your alerts and announcements to specific groups of Users.

These widgets have two tabs:

**Unread:** Non-expired alerts/announcements that you haven’t read.

**Read:** Alerts/announcements that have expired, or that you’ve read.

### Creating Alerts and Announcements <a href="#creating-alerts-and-announcements" id="creating-alerts-and-announcements"></a>

1.  Add the widget by clicking the _Add_ icon (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-app.png)) at the top of a page. Find the Alerts widget or the Announcements widget under the News section and drag it to your page.

    ![Locate the Alerts widget or the Announcements widget and add it to your page.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/alerts-and-announcements/sending-alerts-and-announcements/images/02.png)
2.  To create an alert or announcement click on _Add Alert_ or _Add Announcement_. A new window opens.

    ![A new window opens for you to create your alert or announcement.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/alerts-and-announcements/sending-alerts-and-announcements/images/03.png)
3. Use the Title field to give the alert or announcement a title. Add additional text in the Content field.
4. Set additional settings under the _Configuration_ section below.
5. Click _Save_ when you’re done. Your alert/announcement now appears in the widget.

**Distribution Scope:** The scope where the alert/announcement should appear. The default General scope sends the alert/announcements to everyone. Alternatively, you can select your Site or specific Roles.

**URL:** A URL (optional) to include with the alert/announcement. For example, an announcement about a news story could include a link to the news article. The URL must be valid and begin with `http://` or `https://`.

**Type:** The alert/announcement type. This can be General, News, or Test. Note that each user can specify a different delivery mechanism for each type of alert/announcement. See User Configuration for details.

**Priority:** The announcement’s priority. This can be Normal or Important. Note that this is disabled for alerts because alerts are always high priority.

**Display Date:** The date the alert/announcement should be displayed. This determines when the alert/announcement is sent and appears in the widget. By default, the _Display Immediately_ box is checked. This sets the display date equal to the creation date. Uncheck this box to enter a custom display date. For example, you can create alerts/announcements for display on a later date. This date can be days, weeks, months, or years in the future. Once the Display Immediately box is unchecked, clicking the Display Date field opens the date-picker.

**Expiration Date:** The date and time the alert/announcement expires. Once an alert/announcement expires, the widget displays it in the Read tab. Clicking the Expiration Date field opens the date-picker.

### User Configuration <a href="#user-configuration" id="user-configuration"></a>

Users can configure how they’d like to receive announcements.

1. Click on your User profile at the top right and click on _Account Settings_.
2.  Click on the _Preferences_ tab to find the _Alerts and Announcements Delivery_ option.

    ![Click on the preferences tab to find the settings.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/alerts-and-announcements/sending-alerts-and-announcements/images/06.png)
3. Alerts and announcements are always visible from the page on which the widget is configured. Users can choose to receive alerts and announcements by email as well. Select between General, News, or Test type.
4. Click _Save_ when you’re finished.

### Alert and Announcement Roles <a href="#alert-and-announcement-roles" id="alert-and-announcement-roles"></a>

You can also create Roles for Users to be able manage alerts and announcements:

1. Click on the _Applications Menu_ icon (![Applications Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → _Control Panel_ → _Roles_.
2. With the _Regular Roles_ tab selected, click the _Add_ icon (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)). This opens the New Role form.
3. Name your Role _Announcements_, give it a title and description, and click _Save_.
4. Click on the _Options_ icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) of the Role and click _Edit_.
5.  Click on the _Define Permissions_ tab.

    Under Control Panel, click on _General Permissions_, select _Add General Announcements_. Click _Save_.

    ![Click on the define permissions tab for these options.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/alerts-and-announcements/sending-alerts-and-announcements/images/07.png)

    Go back to the Define Permissions tab. Navigate to _Site and Asset Library Administration_ → _Applications_ → _Announcements_. Select all the resource permissions and click _Save_.
6. Now any User assigned to this Role can manage alerts and announcements.
