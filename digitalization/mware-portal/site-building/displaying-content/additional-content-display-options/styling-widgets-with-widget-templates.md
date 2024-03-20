# Styling Widgets with Widget Templates

You can use widget templates to customize the appearance and functionality of out-of-the-box widgets, such as the _Asset Publisher_ and _Media Gallery_ widgets.

### Creating a Widget Template <a href="#creating-a-widget-template" id="creating-a-widget-template"></a>

1. From the _Site Menu_ ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png), click _Design_ → _Templates_.
2.  Go to the _Widget Templates_ tab.

    ![Access the Widget Templates page from the Templates application.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/styling-widgets-with-widget-templates/images/01.png)
3.  Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select a widget for the template (e.g., Asset Publisher).

    ![The Widget Template creation page provides controls to easily add fields or variables to use.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/styling-widgets-with-widget-templates/images/02.png)
4. Add a name for your widget template.
5.  Enter your FreeMarker script into the template editor. You can use the Elements side panel ( ![Elements](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-list-ul.png) ) to add fields, variables, and utilities to your template. Alternatively, you can type `${` to see field suggestions.

    NOTE

    For versions 7.4+, widget templates only support FreeMarker. Velocity is no longer supported.
6. Click _Save_.

This creates a new widget template, which you can apply to the corresponding type of widget on a page.

### Applying a Widget Template <a href="#applying-a-widget-template" id="applying-a-widget-template"></a>

1. Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the appropriate widget, then click _Configuration._
2.  Find the Display Template drop-down menu and select the desired template.

    ![Select your widget template from the Display Template drop-down menu in the corresponding widget's configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/styling-widgets-with-widget-templates/images/03.png)

    NOTE

    This drop-down menu is on the default tab, Setup, for most widgets. For the Asset Publisher widget, this setting is under the Display Settings tab.
3.  Click _Save_.

    ![After you save your widget's configuration to use your widget template, it uses your template for its display.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/styling-widgets-with-widget-templates/images/04.png)

The widget now uses the custom template. By using more advanced widget template scripts, you can further enhance the capabilities of your widgets.

\
