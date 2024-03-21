# Theme Components

### Theme Templates and Utilities <a href="#theme-templates-and-utilities" id="theme-templates-and-utilities"></a>

The default FreeMarker templates provide helpful utilities and handle key pieces of page layout (page) functionality:

* `portal_normal.ftl`: Similar to a static site’s `index.html`, this file is the hub for all the theme templates and provides the overall markup for the page.
* `init.ftl`: Contains variables commonly used throughout the theme templates. Refer to it to look up theme objects. For convenience, the FreeMarker Variable Reference Guide lists the objects. **DO NOT override this file**.
* `init_custom.ftl`: Used to override FreeMarker variables in `init.ftl` and to define new variables, such as theme settings.
* `portlet.ftl`: Controls the theme’s portlets. If your theme uses Portlet Decorators, modify this file to create application decorator-specific theme settings.
* `navigation.ftl`: Contains the navigation markup. To customize pages in the navigation, you must use the `liferay.navigation_menu` macro. Then you can leverage widget templates for the navigation menu. Note that `navigation.ftl` also defines the hamburger icon and `navbar-collapse` class that provides the simplified navigation toggle for mobile viewports, as shown in the snippet below for the Classic theme:

```markup
<#if has_navigation && is_setup_complete>
  <button aria-controls="navigationCollapse" aria-expanded="false" 
  aria-label="Toggle navigation" class="navbar-toggler navbar-toggler-right" 
  data-target="#navigationCollapse" data-toggle="collapse" type="button">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div aria-expanded="false" class="collapse navbar-collapse" id="navigationCollapse">
    <@liferay.navigation_menu default_preferences="${preferences}" />
  </div>
</#if>
```

![Figure 1: The collapsed navbar provides simplified user-friendly navigation for mobile devices.](https://help.liferay.com/hc/article\_attachments/360041593331)

Figure 1: The collapsed navbar provides simplified user-friendly navigation for mobile devices.

* `portal_pop_up.ftl`: Controls pop up dialogs for the theme’s portlets. Similar to `portal_normal.ftl`, `portal_pop_up.ftl` provides the markup template for all pop-up dialogs, such as a portlet’s Configuration menu. It also has access to the FreeMarker variables defined in `init.ftl` and `init_custom.ftl`.

![Figure 2: Each theme template provides a portion of the pages markup and functionality.](https://help.liferay.com/hc/article\_attachments/360041590772)

Figure 2: Each theme template provides a portion of the page's markup and functionality.

* `FTL_Liferay.ftl`: Provides macros for commonly used portlets and theme resources.
* `taglib-mappings.properties`: Maps the portal taglibs to FreeMarker macros. Taglibs can quickly create common UI components. This properties file is provided separately for each app taglib. For convenience, these FreeMarker macros appear in the FreeMarker Taglib Mappings reference guide. See the Taglib reference for more information on using each taglib in your theme templates.

### CSS Frameworks and Extensions <a href="#css-frameworks-and-extensions" id="css-frameworks-and-extensions"></a>

Themes are integrated with [SASS](https://sass-lang.com/), so you can take full advantage of Sass mixins, nesting, partials, and variables in your CSS.

Also important to note is [Clay CSS](https://clayui.com/), the web implementation of the portal’s [Lexicon design language](https://lexicondesign.io/). An extension of Bootstrap, Clay CSS fills the gaps between Bootstrap and the needs of the portal, providing additional components and CSS patterns that you can use in your themes. Clay base, the portal’s Bootstrap API extension, along with Atlas, a custom Bootstrap theme, creates the portal’s Classic theme. See Customizing Atlas and Clay Base Themes for more information.

### Theme Customizations and Extensions <a href="#theme-customizations-and-extensions" id="theme-customizations-and-extensions"></a>

The theme templates, along with the CSS, provide much of the overall look and feel for the page, but additional extension points/customizations are available. The following extensions and mechanisms are available for themes:

* **Color Schemes:** Specifies configurable color scheme settings Administrators can configure via the Look and Feel menu. See the color scheme tutorial for more information.
* **Configurable Theme Settings:** Administrators can configure theme aspects that change frequently, such as the visibility of certain elements, changing a daily quote, etc. See the Configurable Theme Settings tutorial for more information.
* **Context Contributor:** Exposes Java variables and functionality for use in FreeMarker templates. This allows non-JSP templating languages in themes, widget templates, and any other templates. See the Context Contributors tutorial or more information.
* **Theme Contributor:** A package containing UI resources, not attached to a theme, that you want to include on every page. See the Theme Contributors tutorial for more information.
* **Themelet:** Small, extendable, and reusable pieces of code containing CSS and JavaScript. They can be shared with other developers to provide common components for themes. See Generating Themelets for more information.

### Portlet Customizations and Extensions <a href="#portlet-customizations-and-extensions" id="portlet-customizations-and-extensions"></a>

You can customize portlets with these mechanisms and extensions:

* **Portlet FTL Customizations:** Customize the base template markup for all portlets. See the Theming Portlets for more information.
* **Widget Templates:** Provides an alternate display style for a portlet. Note that not all portlets support widget templates. See the Widget Templates User Guide for more information.
* **Portlet Decorator:** Customizes the exterior decoration for a portlet. See Portlet Decorators for more information.
* **Web Content Template:** Defines how structures are displayed for web content. See the Web Content Templates User Guide articles for more information.

![Figure 3: There are several extension points for customizing portlets](https://help.liferay.com/hc/article\_attachments/360041593311)

Figure 3: There are several extension points for customizing portlets

\
