# Mobile Device Actions

After you’ve created families and applied rules to define those families, you can associate specific actions that occur when a user visits that Site on a device.

To add actions to a selected rule group:

1. Click _Configure_ (![Configure](https://liferay-support.zendesk.com/hc/article\_attachments/360080131251/icon-gear.png)) for the page, page set, or Site where you have configured a device family. The configuration page appears.
2. Click the _Advanced_ tab.
3.  Decline using the same mobile device rules from the public pages by setting the option to _No_. The device family section appears.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360079993772/configure-mobile-device-rule-for-page.png" alt="Figure 1: Disable using public pages device rules." data-size="original">

    Figure 1: Disable using public pages device rules.
4. Click _Select_ to open the Device Families page.
5. In the _Mobile Device Rules_ section, click _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030189032/icon-actions.png)) → _Manage Actions_ next to the device family that you wish to add an action for.
6. Click _Add Action_.

![Figure 2: Getting to the Manage Actions page.](https://liferay-support.zendesk.com/hc/article\_attachments/360079993792/manage-mobile-actions.png)

By default, there are four kinds of actions that can be configured for mobile families:

**Layout Template Modification:** Changes the way portlets are arranged on pages delivered to mobile devices. For example, you could have pages with more complex layouts automatically switch to a simpler template if it detects a mobile device—even if the resolution is theoretically high enough to support the standard layout.

**Theme Modification:** Selects a specific theme for different mobile device families. You’d have to have a mobile version of your Site’s theme that is automatically applied when a device hits your page.

**URL Redirect:** Sends mobile users to any URL. This can be used to direct mobile users to a mobile app download or a mobile version of the page.

**Site Redirect:** Sends mobile users to a different Site on your portal. In some cases, mobile content could be created on a mirror of your Site.

**Tip:** The portal was designed from the ground up to be responsive and adapt to any device that might be accessing it. Before creating new themes or forcing a layout template change, you should test how the Site behaves using the portal out of the box. Certain features, like URL Redirects, can be disruptive and frustrating for users if used improperly.

Like mobile device rules, mobile device actions are extensible. Your developers can define custom actions in addition to the four actions provided by default.

To review, if you want to configure an action or actions that take place when mobile device requests are received, take the following steps:

1. Create a mobile device family to represent the group of devices for which to define an action or actions.
2. Define one or more rules for your family that describe the group of devices represented by your family.
3. Apply your family to an entire page set of a Site (all the public pages of a Site or all the private pages) or to a single page.
4. Define one or more actions for your family that describe how requests should be handled.

### Mobile Device Rules Example <a href="#mobile-device-rules-example" id="mobile-device-rules-example"></a>

Now you’ll look at an example of using mobile device rules. Suppose you want to create a rule so that when a Site is accessed by an Android device, a different layout is used. To set this up, you must follow the same four steps described above.

First create the Mobile Device Family:

1. Navigate to the _Mobile Device Families_ page of _Site Administration_.
2. Click _Add Device Family_ (![Add Device Family](https://liferay-support.zendesk.com/hc/article\_attachments/360030157871/icon-add.png)).
3. Enter _Android_ for the _Name_.
4. Click _Save_.

Next create a rule for the family:

1. From the _Mobile Device Families_ page, click on _Android_.
2. Click _Add Classification Rule_ (![Add Classification Rule](https://liferay-support.zendesk.com/hc/article\_attachments/360030157871/icon-add.png)).
3. Name the rule _Rule 1_.
4. Under _Operating System_ select _Android_ (you can hold or to select multiple items).
5. Under _Device Type_ select _All_,
6.  Click _Save_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030157831/example-classification-rule.png" alt="Figure 3: Create the Classification rule." data-size="original">

    Figure 3: Create the Classification rule.

As with the previous example, you only need one rule to describe your device family. Now you must apply the rule to some pages.

1. Go to _Site Builder_ → _Pages_ in Site Administration.
2. Click on the _Configure_ icon for the _Public Pages_
3. Go to the _Advanced_ tab.
4. Under _Mobile Device Rules_, select the _Android_ device family.

Now you must define an action for your Android rule group to use a different layout.

1. Click _Actions_ → _Manage Actions_ for the _Android_ rule.
2. Click _Add Action_.
3. Enter the name _Layout Template Modification_, and select the _Layout Template Modification_ action type.
4. Select the _1 Column_ layout template.
5. Click _Save_.

![Figure 4: Create the Actions for Android.](https://liferay-support.zendesk.com/hc/article\_attachments/360080131231/example-mobile-action.png)

Now the portal Site’s pages are presented to Android users with the 1 Column layout template.

Mobile Device Rules are a powerful way to manage the way pages and content appear on the various devices that access your Site. But remember to consider the power of modern devices and the experience of your users, and use this great power responsibly—to help users have a great experience on your website and to not interrupt or negatively impact that experience on whatever device they’re using.
