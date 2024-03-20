# Understanding Pages

Pages display content and applications to users of a site.

### Page Sets <a href="#page-sets" id="page-sets"></a>

When private pages are enabled, the site’s pages are organized into two page sets: public pages and private pages.

NOTE

Private pages are disabled by default for new installations. See Enabling Private Pages if you want to enable them.

Public and private pages differ in the following ways:

| Behavior                                   | Public Pages | Private Pages |
| ------------------------------------------ | ------------ | ------------- |
| Visible to unauthenticated users           | ✔            |               |
| Viewing requires login and site membership |              | ✔             |
| Distinct URL pattern                       | ✔            | ✔             |

Public pages use the `[web-address]/`**web**`/[site-name]` URL pattern, while private pages use `[web-address]/`**group**`/[site-name]`.

### Page Types <a href="#page-types" id="page-types"></a>

There are several page types available when you add a page. The default page type is a content page. You can also create a page based on a page template. Page types and page templates are described in more detail below.

![You must select a page type when adding pages.](<../../../../.gitbook/assets/01 (3).png>)

TIP

The currently viewed page’s type appears at the top of the page to help you determine the administration options you have and where you need to go to configure the page.

#### Content Pages <a href="#content-pages" id="content-pages"></a>

With content pages, users can build professional pages that can be easily managed and have their content edited in-line. Content pages are primarily built using page fragments and widgets.

![Content pages can be composed of multiple fragments.](<../../../../.gitbook/assets/02 (3).png>)

Using content pages allows access to additional powerful features and benefits:

* Personalized and adaptive site experiences
* A/B testing
* Approachable content and site building experience

To learn more about content pages, see the Using Content Pages. To start building one, see Adding Elements to Content Pages.

#### Widget Pages <a href="#widget-pages" id="widget-pages"></a>

A widget page has a set layout for displaying widgets (applications). Widgets can display content or add interactive and dynamic functionality to the page. See Adding Widgets to a Page for more information.

![Widget pages can provide a number of functions, such as a dedicated wiki page solution.](<../../../../.gitbook/assets/03 (2).png>)

#### Content Pages and Widget Pages Compared <a href="#content-pages-and-widget-pages-compared" id="content-pages-and-widget-pages-compared"></a>

This table compares some of the distinctions between a content page and a widget page:

| Feature                         | Content Pages | Widget Pages |
| ------------------------------- | ------------- | ------------ |
| Ability to Add Widgets          | ✔             | ✔            |
| Personalized Site Experiences   | ✔             |              |
| Access to A/B Testing           | ✔             |              |
| Easy Layout and Content Editing | ✔             |              |
| Advanced Custom Layouts         |               | ✔            |
| User-Customizable Columns       |               | ✔            |
| Staging Page Variations         |               | ✔            |

#### Other Page Types <a href="#other-page-types" id="other-page-types"></a>

There are several other available page types - each satisfies particular use cases. See Other Page Types for more information.

### Global Page Templates <a href="#global-page-templates" id="global-page-templates"></a>

When adding a page, you can also choose a page template. Page templates are pre-set pages with applications already deployed to the page. By default, only Global Templates are available, but additional collections you create also appear as an option.

| Global Page Template | Description                                                                                                                                                                                                |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Search               | Creates a widget page with the following widgets: Search Bar, Suggestions, Site Facet, Type Facet, Tag Facet, Category Facet, Folder Facet, User Facet, Modified Facet, Search Results and Search Options. |

See Creating a Page Template to learn more about page templates.

### Child Pages and Page Hierarchy <a href="#child-pages-and-page-hierarchy" id="child-pages-and-page-hierarchy"></a>

Pages can also be created in a hierarchical fashion by creating child pages. Child pages can be useful if you have multiple pages that can be organized into categories.

![You can add a child Page to a top-level page to organize pages hierarchically.](<../../../../.gitbook/assets/04 (1).png>)

TIP

You can navigate among pages of a site hierarchy with the Site Map application. A site administrator can configure a root page and a display depth. The display depth of the Site Map application determines how many levels of nested pages to display.

### Enabling Private Pages <a href="#enabling-private-pages" id="enabling-private-pages"></a>

By default, new portal installations include a release feature flag that prevents users from creating private pages. Follow these steps to enable private pages for your portal system:

1. Open the _Global Menu_ ( ![Global menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _System Settings_.
2. Go to _Release Feature Flags_.
3.  In the Disabled Features dropdown menu, select _Disable Private Pages_.

    ![Select disable private pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/understanding-pages/understanding-pages/images/05.png)
4.  Click _Save_.

    If you ever want to disable private pages again, deselect _Disable Private Pages_ and click _Update_.

\
