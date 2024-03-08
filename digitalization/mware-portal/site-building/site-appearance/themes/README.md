# Themes

Themes customize the default look and feel of your Site. They allow you to define the style and visual identity of your brand or label across your whole Site.

### [Themes and Site Design](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes#themes-and-site-design) <a href="#themes-and-site-design" id="themes-and-site-design"></a>

Themes are available and used out-of-the-box on any Site you create. There are also a number of other ways (such as [Style Books](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance)) to define the look and feel of your Site, but themes provide the most flexibility when you need a fully customized solution.

![Site Design hierarchy](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/themes/introduction-to-themes/images/01.png)

Themes are one of the most broad and flexible ways to add customizations, in the hierarchy of ways to design your Site’s appearance. Themes are used for providing the most foundational basis for your Site that more specific tools (such as [Master Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/defining-headers-and-footers/master-page-templates) and [Style Books](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance)) can build on top of. However, you may also use themes to add more customizations that are not possible with those other methods.

WARNING

Adding customizations to your site through a theme can come with increased maintenance when upgrading from one version of Liferay to another. When possible, manage your site’s appearance through the use of [Master Pages Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/defining-headers-and-footers/master-page-templates) and [Style Books](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance).

### [Developing Themes](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes#developing-themes) <a href="#developing-themes" id="developing-themes"></a>

Themes can be used to customize your Site in a variety of ways, including styling your pages, creating templates, and even extending functionality at runtime.

#### [Styling Your Site](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes#styling-your-site) <a href="#styling-your-site" id="styling-your-site"></a>

Themes provide the basis for styling on any Site. You can define the UI details for your Site’s pages, customized portlet decorators, and define the color scheme for your Site.

You can also configure a custom [Style Book Token Definition](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/developer-guide/style-book-token-definitions) based on your theme.

#### [Designing Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes#designing-pages) <a href="#designing-pages" id="designing-pages"></a>

Themes are also a complement to [Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template) or [Master Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/defining-headers-and-footers/master-page-templates) for designing a template for your Site’s pages. It can be used in conjunction with these other methods, which add on top of the design from your theme.

For example, you can [embed widgets in your theme](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/theme-development/working-with-templates/embedding-widgets-via-templates) to make them a permanent fixture of your Site’s pages.

NOTE

You can also create templates for your pages using [Master Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/defining-headers-and-footers/master-page-templates) or [Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template).

#### [Bundling Resources](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes#bundling-resources) <a href="#bundling-resources" id="bundling-resources"></a>

Resources may be bundled into your theme or from other sources to bring together many sources of code at once. This allows you to adapt your strategy depending on how you want to deploy different changes.

* [Themelets](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/theme-development/bundling-resources/bundling-and-installing-resources-into-your-theme-via-themelets) allow you to customize a small number of files, in a modular way that can apply to many themes.
* Theme contributors allow you to package UI resources and customizations that can be deployed independently of a theme.
* The Resources Importer uses Site Templates to import files and content necessary to implement a theme.

WARNING

The Resources Importer is deprecated as of Liferay DXP 7.1.

#### [Modifying Behavior](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes#modifying-behavior) <a href="#modifying-behavior" id="modifying-behavior"></a>

Themes are also a powerful tool for modifying the behavior of different parts of your Site.

* You can add context variables usable in your [Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/creating-a-page-template) so that these templates can hook into the functionality of your theme.
* You can set variable values with your theme to influence the behavior of existing functionality in DXP.
* You can add [token definitions](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/developer-guide/style-book-token-definitions) that are used to define categories for styling with [Style Books](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance).

### [Upgrading Themes](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes#upgrading-themes) <a href="#upgrading-themes" id="upgrading-themes"></a>

The first step to upgrade your theme is to use the Liferay Theme Generator to run the upgrade task. This task updates your theme’s version so that you can deploy it to the new version of DXP. See [Upgrading a Theme](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/upgrading-a-theme) for more information.

### [Tools](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes#tools) <a href="#tools" id="tools"></a>

A variety of tools are used in developing or extending themes. See these topics for more information:

* Liferay Theme Generator
* [Blade](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/blade-cli/generating-projects-with-blade-cli)
* [Developer Tools Overview](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/developer-tools-overview)
