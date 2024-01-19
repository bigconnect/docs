# Creating Mobile Device Rules

To configure mobile device rules, you need a way to find out the characteristics of the device. While some of the characteristics are provided by the device, most are not. For this reason, there are databases that contain information about thousands of devices. These databases make it possible to learn device details from the device type. The portal’s Mobile Device Rules can connect to device databases so that you can use their device characteristics in your rules.

You can develop plugins that integrate with other device databases. Even if you don’t have a device database, you can still set up mobile device rules. They won’t, however, be effective until a database is deployed, because the portal won’t have enough information about the devices being used to make page requests.

To access the Mobile Device Families administrative page,

1. Open the _Product Menu_.
2. Use the _Site Selector_ (![Site Selector](https://liferay-support.zendesk.com/hc/article\_attachments/360030188952/icon-compass.png)) to choose the Site that you want to define Mobile Device Rules for.
3. Select _Configuration_ → _Mobile Device Families_.

You can also add families for all Sites by navigating to the Control Panel → _Sites_ → _Sites_ → _Global_. The Mobile Device Families administrative page displays a list of defined families and lets you add more. To add rules, you must first add a family.

1. Click _Add_ button (![Add Family](https://liferay-support.zendesk.com/hc/article\_attachments/360030188972/icon-add.png)) to add a _New Device Family_.
2. Enter a _Name_ and _Description_.
3. Click _Save_.
4. Click on the name of the Mobile Device Family to access the rules page.

![Figure 1: Create a Mobile Device Family so you can create rules.](https://liferay-support.zendesk.com/hc/article\_attachments/360079993752/mobile-device-families.png)

The rules defined for a family, along with the priorities of the families selected for a particular Site or page, determine which family’s actions are applied to a given request. From the New Classification Rule page for a specific rule set, you can add a rule by specifying an operating system, rule type, physical screen size, and screen resolution. Remember that you can add as many rules to a family as you need in order to classify the devices on which you’d like to take actions.

1. Click on the _Add_ button (![Add Classification Rule](https://liferay-support.zendesk.com/hc/article\_attachments/360030188972/icon-add.png)) to add a new rule.
2. Enter a _Name_ and _Description_.
3. Select the classifications you want for this rule from _Operating System and Type_, _Physical Screen Size_, and _Screen Resolution_.
4. Click _Save_.

You’ll notice after saving the classification rule that it’s characterized as a _Simple Rule_. Only Simple Rules are included with the portal, but the rules are designed to be extensible, and additional rule types can be added by your developers.

![Figure 2: Select the operating system and device type for your rule.](https://liferay-support.zendesk.com/hc/article\_attachments/360080131211/mobile-device-editing-rule.png)

Once you’ve created some mobile device families and added some rules to them, you’re ready to create some actions. The actions defined for a family determine what happens to a request when the device is detected and the family has been found to apply.

You can add families to a Site, individual page, or page set from their respective configuration pages. To do it for a Page Set:

1. Go to _Site Builder_ → _Pages_ in your Site.
2. Click on _Configure_ (![Configure](https://liferay-support.zendesk.com/hc/article\_attachments/360030188992/icon-gear.png)) for the Public Pages.
3. Select the _Advanced_ tab and open the _Mobile Device Rules_ option in the bottom menu.
4. Click _Select_ to open the list of families that can be applied.

From the same page, you can access the configuration for an individual page, or you can configure Mobile Device Rules for an entire Site from _Configuration_ → _Site Settings_. You can select multiple families for a particular Site or page and order them by priority. The families are checked in decreasing order of priority: the actions defined by the first family that applies are executed.

![Figure 3: You can select a mobile device family to apply for a Site or page.](https://liferay-support.zendesk.com/hc/article\_attachments/360080131191/mobile-device-selection.png)

