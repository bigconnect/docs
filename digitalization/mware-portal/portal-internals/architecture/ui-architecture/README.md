# UI Architecture

[Liferay DXP’s UI](https://help.liferay.com/hc/en-us/articles/360028818552-Introduction-to-The-Liferay-Distinction) is a portal for adding sites, pages, widgets, and content. It helps people do work, [collaborate](https://help.liferay.com/hc/en-us/articles/360029040511-Introduction-to-Collaboration), and [share content](https://help.liferay.com/hc/en-us/articles/360028819232-Introduction-to-Web-Experience-Management).

The UI comprises the following parts:

* Content: images, videos, and text.
* [Applications](https://help.liferay.com/hc/en-us/articles/360029028011-Introduction-to-Application-Development): Widgets and portlets that expose functionality for accomplishing tasks.
* [Themes](https://help.liferay.com/hc/en-us/articles/360029045971-Themes): Plugins that use CSS, FreeMarker templates, HTML, and JavaScript to provide a site’s overall look and feel.
* Product navigation sidebars and panels: Use these for administering sites.

### [Content](https://help.liferay.com/hc/en-us/articles/360035581011-UI-Architecture#content) <a href="#content" id="content"></a>

Liferay DXP’s built-in applications help you publish images, video, forms, markup text, and more to site pages. [Documents and Media](https://help.liferay.com/hc/en-us/articles/360029040531-Managing-Documents-and-Media) stores images, videos, and documents to use throughout your site. The [Web Experience Management](https://help.liferay.com/hc/en-us/articles/360028819232-Introduction-to-Web-Experience-Management) suite helps you create, maintain, and organize content. [Liferay Forms](https://help.liferay.com/hc/en-us/articles/360029134031-Introduction-to-Forms) gives you robust form building capability. [Message Boards](https://help.liferay.com/hc/en-us/articles/360029041231-Creating-Forums-with-Message-Boards) facilitate lively discussions and [Blogs](https://help.liferay.com/hc/en-us/articles/360028720692-Publishing-Blogs) let users express themselves with markup text and images. These are just a few of the built-in applications for adding site content.

### [Applications](https://help.liferay.com/hc/en-us/articles/360035581011-UI-Architecture#applications) <a href="#applications" id="applications"></a>

Liferay DXP applications provide content and help users accomplish tasks. They’re [developed the same way](https://help.liferay.com/7-2/appdev/-/knowledge\_base/a/web-front-ends) as other web applications, and Liferay DXP can combine multiple applications on one page.

Liferay DXP supports developing JavaScript-based applications using popular front-end frameworks:

* [Angular](https://help.liferay.com/hc/en-us/articles/360029028031-Developing-an-Angular-Application)
* [React](https://help.liferay.com/hc/en-us/articles/360029028051-Developing-a-React-Application)
* [Vue](https://help.liferay.com/hc/en-us/articles/360029028071-Developing-a-Vue-Application)

Java-based portlet applications use the latest portlet standards and frameworks, including ones familiar to experienced Liferay portlet developers:

* [Liferay MVC Portlet](https://help.liferay.com/hc/en-us/articles/360029028191-Liferay-MVC-Portlet)
* [PortletMVC4Spring](https://help.liferay.com/hc/en-us/articles/360029850931-PortletMVC4Spring)
* [JSF Portlet](https://help.liferay.com/hc/en-us/articles/360029028111-JSF-Portlet)

In the UI, applications are referred to as Widgets and categorized for users to add to pages. Administrative applications are available in the product menu panels.

![Figure 1: Widget pages offer users functionality. Widgets are organized into a page templates rows and columns. This template has two columns: a smaller left column and larger right column. On this page, users select tags in the Tag Cloud widget and the matching tagged images show the Asset Publisher widget.](https://help.liferay.com/hc/article\_attachments/360041590712)

Figure 1: Widget pages offer users functionality. Widgets are organized into a page template's rows and columns. This template has two columns: a smaller left column and larger right column. On this page, users select tags in the Tag Cloud widget and the matching tagged images show the Asset Publisher widget.

### [Themes](https://help.liferay.com/hc/en-us/articles/360035581011-UI-Architecture#themes) <a href="#themes" id="themes"></a>

A [theme](https://help.liferay.com/hc/en-us/articles/360029045971-Themes) styles a site with a unique look and feel. It’s developed as a WAR project that includes CSS, JavaScript, and markup content. You can develop themes using whatever tools you prefer, but Liferay DXP offers [Bootstrap](https://getbootstrap.com/)-based components and [theme tooling](https://help.liferay.com/hc/en-us/articles/360029046031-Developing-Themes) to create and deploy themes in no time.

![Figure 2: You can select an attractive theme and apply it to your site.](https://help.liferay.com/hc/article\_attachments/360041590752)

Figure 2: You can select an attractive theme and apply it to your site.

Here’s a quick demonstration of developing a theme:

1. Create a theme using the [Theme Generator](https://help.liferay.com/hc/en-us/articles/360029147711-Theme-Generator). The theme extends the base theme you specified to the Theme Generator—Liferay’s [Styled theme](https://github.com/liferay/liferay-portal/tree/7.2.x/modules/apps/frontend-theme/frontend-theme-styled) is the default.
2. Run [`gulp build`](https://portal.liferay.dev/docs/7-2/frameworks/-/knowledge\_base/f/building-your-themes-files) to generate the base theme files to the `build` folder subfolders:
   * `templates`: FreeMarker templates specify site page markup. `portal_normal.ftl` is the central file; it includes templates that define the page parts (e.g., header, navigation, footer). The `init.ftl` file defines default variables available to the templates.
   * `css`: SCCS files that provide styling.
   * `font`: Font Awesome and Glyphicons fonts.
   * `js`: JavaScript files; `main.js` is the Styled theme’s JavaScript.
   * `images`: Image files.
3. Override aspects of the base theme by copying relevant files from the `build` subfolders to folders of the same name in your `src` folder. The [Theme Anatomy Guide](https://help.liferay.com/hc/en-us/articles/360028833812-Theme-Reference-Guide) describes all the files. Here’s an example of a customized `portal_normal.ftl`:

```markup
<html class="${root_css_class}">
<head></head>
  <body class="${body_class}">
    <header class="${header_css_class}">
      <a class="${logo_css_class} href="${site_url}"><img src="${site_logo}"/></a>
      <#include "${full_templates_path}/navigation.ftl" />
    </header>
    <section>
      ${portlets}
    </section>
      <#include "${full_templates_path}/footer.ftl" />
  </body>
</html>
```

4. Add custom styling using your theme’s `_custom.scss` file (i.e., `src/css/_custom.scss`). Liferay DXP supports [Bootstrap](https://getbootstrap.com/), as well as [Sass](https://sass-lang.com/), so you can use Bootstrap utilities in your markup and Sass nesting, variables, and more in your CSS files. This snippet styles the logo:

```sass
.logo {
  margin-left: 15px;

  img {
    height: auto;
  }

  @include media-breakpoint-down(md) {
    text-align: center;
    width: 100%;
  }
}
```

![Figure 3: You can provide custom styling using the themes \_custom.sccs file.](https://help.liferay.com/hc/article\_attachments/360041590732)

Figure 3: You can provide custom styling using the theme's \`\_custom.sccs\` file.

5. Deploy your theme by executing `gulp deploy`.

The theme is available to [apply](https://help.liferay.com/hc/en-us/articles/360029046071-Deploying-and-Applying-Themes) to your site.

For details, [Theme Components](https://help.liferay.com/hc/en-us/articles/360035219072-Theme-Components) breaks down a theme’s parts, and the [Themes section](https://help.liferay.com/hc/en-us/articles/360029045971-Themes) provides theme development details.

### [Product Navigation Sidebars and Panels](https://help.liferay.com/hc/en-us/articles/360035581011-UI-Architecture#product-navigation-sidebars-and-panels) <a href="#product-navigation-sidebars-and-panels" id="product-navigation-sidebars-and-panels"></a>

The product navigation sidebars and panels enable administrators to build sites, add pages, apply themes, and configure the portal. It’s also where you can provide administrative functionality for your custom applications. The navigation sidebars and panels are customizable.

![Figure 4: Liferay facilitates integrating custom administrative functionality through navigation menus and administrative applications.](https://help.liferay.com/hc/article\_attachments/360041593291)

Figure 4: Liferay facilitates integrating custom administrative functionality through navigation menus and administrative applications.

As you can see, Liferay DXP’s UI is highly flexible and customizable. Here’s where to learn more:

* [Theme Components](https://help.liferay.com/hc/en-us/articles/360035219072-Theme-Components): Explains available mechanisms and extensions for customizing and theming pages, content, and applications.
* [Understanding the Page Structure](https://help.liferay.com/hc/en-us/articles/360035581031-Understanding-the-Page-Structure): Describes how the page’s UI is organized and introduces tools for populating and developing each section.
