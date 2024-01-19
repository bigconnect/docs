# Styling Widgets with Widget Templates

Suppose you’re customizing the Lunar Resort Site and want to allow users to use Facebook or Twitter to communicate with other interested travelers. You can add this functionality to an existing widget using widget templates: launch a template editor, create a custom template, and configure your app to host that template. Widget templates let you re-skin your widget and give you ultimate control over its appearance and functionality.

### Creating a Widget Template <a href="#creating-a-widget-template" id="creating-a-widget-template"></a>

Here’s the process of creating a widget template:

1. From Site Administration, click the _Site Selector_ button (![Compass](https://liferay-support.zendesk.com/hc/article\_attachments/360030189612/icon-compass.png)) to choose the Site where you want to create the widget template.
2. Open _Site Builder_ → _Widget Templates_.

If you selected the Global context, this page shows a list of sample templates available for your apps. These sample templates differ from the default templates already configured in the apps. If you choose a Site to host your template, you must create a custom template for that Site’s apps.

1. Click the _Add_ (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030158431/icon-add.png)) button, and you’re prompted to select the type of template to create.

* _Asset Publisher_
* _Blogs_
* _Breadcrumb_
* _Categories Navigation_
* _Language Selector_
* _Media Gallery_
* _Navigation Menu_
* _RSS Publisher_
* _Site Map_
* _Tags Navigation_
* _Wiki_

1. Enter the name and, optionally, open _Details_ to provide a description and a small image to use. You can select the language type for your template.
2. Within _Details_ select a scripting language to use. You can use FreeMarker or Velocity. FreeMarker is recommended.
3. Use the _Script_ section to create the widget template.
4. Click _Save_ when done.

#### THE TEMPLATE EDITOR <a href="#the-template-editor" id="the-template-editor"></a>

On the left side of the template editor is a palette of common variables used for making templates. This is a great reference when creating your template. To place one of the variables into the template editor, position your text cursor where you want it placed, and click the variable name.

Each variable also has a tooltip which displays a detailed description. Because there are multiple kinds of widget templates, there are also different variables for each widget template. Thus, each template has a different set of variables only applicable for that specific template.

![Figure 2: The portal offers a versatile script editor to customize your widget template.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158451/adt-script-editor.png)

You can also use the autocomplete feature to add variables to your template. It can be invoked by typing _${_ which opens a drop-down menu of available variables. By clicking one of the variables, the editor inserts the variable into the editor.

You can also embed same-type templates into other templates. For example, suppose you have an existing Wiki widget template and would like to create another similar Wiki widget template. Instead of starting from scratch, you can import the existing Wiki widget template into your new one and build off of it. In other words, you can utilize widget templates as generic templates which allow for reusable code to be imported by Velocity or FreeMarker templates in the system.

### Configuring Widget Templates <a href="#configuring-widget-templates" id="configuring-widget-templates"></a>

After you’ve saved your widget template, you can manage it through its _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030158411/icon-actions.png)) button. This provides several options:

* _Edit_: lets you modify the widget template’s setup properties.
* _Permissions_: lets you manage the permissions _Update_, _Permissions_, _Delete_, and _View_ for the widget template.
* _Copy_: creates a copy of the widget template.
* _Delete_: deletes the widget template.

Additionally, your widget template generates a static URL and a WebDAV URL. These values access the XML source of your template. You can find these URLs by clicking the widget template from the menu and expanding the _Details_ section. With the WebDAV URL, Site administrators can add, browse, edit, and delete widget templates on a remote server. If you want to learn more about what the WebDAV URL can do, visit the article on [WebDAV access](https://help.liferay.com/hc/zh-cn/articles/360028720352-Desktop-Access-to-Documents-and-Media).

**Note:** Embedding widgets into widget templates, although possible, is not recommended because this could cause conflicts with other widgets or unexpected behavior (e.g., embedding a widget that aggregates data to the breadcrumb). If embedding a widget into a widget template is your only option, make sure it does not interfere with other widgets.

Next you must configure the widget to use the new widget template:

1. Go to the _Configuration_ page for the widget you want to modify and open its _Display Settings_.
2. Under _Display Template_, select your widget template from the drop-down menu.

Also, you can manage Site-specific display templates for your app: do this by clicking the _Manage Display Templates for \[SPECIFIC\_SITE]_ link next to the _Display Template_ drop-down menu. A window appears with a list of your configured templates only available for your Site with options to add new templates or edit existing templates.

![Figure 3: In the Configuration menu of an app, you can edit and manage available widget templates.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158491/adt-configuration.png)

