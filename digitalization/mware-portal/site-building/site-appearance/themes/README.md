# Themes

Themes customize the default look and feel of your Site. They allow you to define the style and visual identity of your brand or label across your whole Site.

### Themes and Site Design <a href="#themes-and-site-design" id="themes-and-site-design"></a>

Themes are available and used out-of-the-box on any Site you create. There are also a number of other ways (such as Style Books) to define the look and feel of your Site, but themes provide the most flexibility when you need a fully customized solution.

![Site Design hierarchy](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/themes/introduction-to-themes/images/01.png)

Themes are one of the most broad and flexible ways to add customizations, in the hierarchy of ways to design your Site’s appearance. Themes are used for providing the most foundational basis for your Site that more specific tools (such as Master Pages and Style Books) can build on top of. However, you may also use themes to add more customizations that are not possible with those other methods.

WARNING

Adding customizations to your site through a theme can come with increased maintenance when upgrading from one version of Liferay to another. When possible, manage your site’s appearance through the use of Master Pages Templates and Style Books.

### Developing Themes <a href="#developing-themes" id="developing-themes"></a>

Themes can be used to customize your Site in a variety of ways, including styling your pages, creating templates, and even extending functionality at runtime.

#### Styling Your Site <a href="#styling-your-site" id="styling-your-site"></a>

Themes provide the basis for styling on any Site. You can define the UI details for your Site’s pages, customized portlet decorators, and define the color scheme for your Site.

You can also configure a custom Style Book Token Definition based on your theme.

#### Designing Pages <a href="#designing-pages" id="designing-pages"></a>

Themes are also a complement to Page Templates or Master Page Templates for designing a template for your Site’s pages. It can be used in conjunction with these other methods, which add on top of the design from your theme.

For example, you can embed widgets in your theme to make them a permanent fixture of your Site’s pages.

NOTE

You can also create templates for your pages using Master Pages or Page Templates.

#### Bundling Resources <a href="#bundling-resources" id="bundling-resources"></a>

Resources may be bundled into your theme or from other sources to bring together many sources of code at once. This allows you to adapt your strategy depending on how you want to deploy different changes.

* Themelets allow you to customize a small number of files, in a modular way that can apply to many themes.
* Theme contributors allow you to package UI resources and customizations that can be deployed independently of a theme.
* The Resources Importer uses Site Templates to import files and content necessary to implement a theme.

WARNING

The Resources Importer is deprecated as of Liferay DXP 7.1.

#### Modifying Behavior <a href="#modifying-behavior" id="modifying-behavior"></a>

Themes are also a powerful tool for modifying the behavior of different parts of your Site.

* You can add context variables usable in your Page Templates so that these templates can hook into the functionality of your theme.
* You can set variable values with your theme to influence the behavior of existing functionality in DXP.
* You can add token definitions that are used to define categories for styling with Style Books.

### Upgrading Themes <a href="#upgrading-themes" id="upgrading-themes"></a>

The first step to upgrade your theme is to use the Liferay Theme Generator to run the upgrade task. This task updates your theme’s version so that you can deploy it to the new version of DXP. See Upgrading a Theme for more information.

### Tools <a href="#tools" id="tools"></a>

A variety of tools are used in developing or extending themes. See these topics for more information:

* Liferay Theme Generator
* Blade
* Developer Tools Overview
