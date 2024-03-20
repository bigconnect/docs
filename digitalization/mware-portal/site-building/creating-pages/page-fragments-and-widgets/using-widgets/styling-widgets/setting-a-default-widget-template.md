# Setting a Default Widget Template

You can change the widget template for individual widgets through their own configuration; however you can also configure a default widget template to apply to all widgets of a particular type. To do this, go to _Control Panel_ → _System Settings_. In System Settings you can find a configuration for every widget in the portal. Widgets that support widget templates have a _Display Style Group ID_ and a _Display Style_ option.

![The widget template configuration in System Settings controls changing the display style.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-widgets/styling-widgets/setting-a-default-widget-template/images/01.png)

**Display Style Group ID:** The Site ID where the widget template is located. For Global templates use 0 for the ID.

**Display Style:** The widget template’s key.

NOTE

To enter a Display Style, you first need the _Template Key_ for the template you want to use. To get the Template Key, go to the _Application Display Template_ list for a given Site and retrieve it from the widget template listing. Then enter the display style as `ddmTemplate_[template-key]`.

### Default Widget Template Example <a href="#default-widget-template-example" id="default-widget-template-example"></a>

For example, configure the Language Selector widget templates like this:

1. Open the _Home_ → _Design_ → _Widget Templates_.
2. Click _Add_ (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select the option _Language Selector Template_.
3. Open _Details_ and find the _Template Key_ - `LANGUAGE-ICON-FTL`

![System Settings shows where you can find the Template Key.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-widgets/styling-widgets/setting-a-default-widget-template/images/02.png)

Now that you have the ID, you can change the template from System Settings.

1. Go to _Control Panel_ → _Configuration_ → _System Settings_.
2. Find _Localization_ under the _Platform_ heading and select _Language Selector_ from the options on the left.
3. In the _Display Style_ field, enter `ddmTemplate_LANGUAGE-ICON-FTL`.

Now any Language Selector widgets are added to a page use the new defaults.

WARNING

Setting a new default widget template does not affect any widgets that are already added to a page and have a configured widget template.
