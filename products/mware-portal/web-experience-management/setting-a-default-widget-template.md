# Setting a Default Widget Template

You can change the widget template for an individual widget through its own configuration, but to configure the default widget template for all widgets of that type, you must go to _System Settings_. In the System Settings you can find a configuration for every widget in MWARE Portal. Any widget that supports widget templates has a _Display Style Group ID_ and a _Display Style_ option.

![Figure 1: The widget template configuration in System Settings lets you change the display style.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158591/adt-system-settings.png)

**Display Style Group ID:** The Site ID where the widget template is located. For Global templates use _0_ for the ID.

* **Display Style:** The widget template’s key.

To enter a Display Style, you first need the _Template Key_ for the template you want to use. To get the Template Key, go to the _Application Display Template_ list for a given Site and retrieve it from the widget template listing. Then enter the display style as `ddmTemplate_[template-key]`.

### [Default Widget Template Example](https://help.liferay.com/hc/zh-cn/articles/360028819872-Setting-a-Default-Widget-Template#default-widget-template-example) <a href="#default-widget-template-example" id="default-widget-template-example"></a>

For example, configure the Language Selector widget templates like this:

1. Open the _Product Menu_.
2. Using the _Site Selector_, select the _Global_ site.
3. Go to _Site Builder_ → _Widget Templates_
4. Create a _Language Selector Template_.
5. Click the _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030158571/icon-actions.png)) button for the new widget template.
6. Open _Details_ and find the _Template Key_ - `LANGUAGE-ICON-FTL`

![Figure 2: System Settings shows where you can find the Template Key.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158611/adt-template-key.png)

Now that you have the ID, you can change the template from System Settings.

1. Go to the _Control Panel_ → _Configuration_ → _System Settings_.
2. Find _Localization_ under the _Platform_ heading and select _Language Selector_ from the options on the left.
3. In the _Display Style_ field, enter `ddmTemplate_LANGUAGE-ICON-FTL`.

Now any Language Selector widgets are added to a page use the new defaults. This doesn’t affect widgets already added to a page and configured.

![Figure 3: You can see the new default configuration.](https://liferay-support.zendesk.com/hc/article\_attachments/360030189692/adt-new-default.png)

