# Understanding the Page Structure

Understanding the page’s structure is crucial to targeting the correct markup for styling, organizing your content, and creating your site. Your page layout is unique to the requirements and design for your site. The Unstyled theme’s default page layout is organized into three key sections in its `portal_normal.ftl` template:

* **Header:** Contains the navigation, site logo and title (if shown), and sign-in link when the user isn’t logged in.
* **Main Content:** Contains the portlets or fragments for the page.
* **Footer:** contains additional information, such as the copyright or author.

![Figure 1: The page layout is broken into three key sections.](https://help.liferay.com/hc/article\_attachments/360041590792)

Figure 1: The page layout is broken into three key sections.

### [Portlets or Fragments](https://help.liferay.com/hc/en-us/articles/360035581031-Understanding-the-Page-Structure#portlets-or-fragments) <a href="#portlets-or-fragments" id="portlets-or-fragments"></a>

The `#content` `Section` makes up the majority of the page. Portlets or fragments are contained inside the `#main-content` `div`. Liferay DXP ships with a default set of applications that provide common functionality, such as forums and Wikis, documents and media, blogs, and more. For more information on using Liferay DXP and its native portlets, see the [User & Admin documentation](https://help.liferay.com/documentation/user). You can also create custom portlets for your site. Portlets can be added via the Add Menu (referred to as widget), included in a sitemap through the [Resources Importer](https://help.liferay.com/hc/en-us/articles/360028746532-Importing-Resources-with-a-Theme), or they can be [embedded in the page’s theme](https://help.liferay.com/hc/en-us/articles/360029067011-Embedding-Portlets-in-Themes). See the [portlet tutorials section](https://help.liferay.com/hc/en-us/articles/360029046351-Introduction-to-Portlets) for more information on creating and developing portlets.

You can target the elements and IDs shown in the table below to style the page:

| Element   | ID                           | Description                                           |
| --------- | ---------------------------- | ----------------------------------------------------- |
| `div`     | `#wrapper`                   | The container div for the page contents               |
| `header`  | `#banner`                    | The page’s header                                     |
| `section` | `#content` > `#main-content` | The main contents of the page (portlets or fragments) |
| `footer`  | `#footer`                    | The page’s footer                                     |

![Figure 2: Each section of the page has elements and IDs that you can target for styling.](https://help.liferay.com/hc/article\_attachments/360041590852)

Figure 2: Each section of the page has elements and IDs that you can target for styling.

As shown in the diagram above, you can also add [fragments](https://help.liferay.com/hc/en-us/articles/360028726832-Page-Fragments) to a page. Fragments are components—composed of CSS, JavaScript, and HTML—that provide key pieces of functionality for the page (i.e. a carousel or banner). Liferay DXP provides an editor for creating collections of fragments that you can then add to the page. These fragments can be edited on the page to suit your vision.

### [Layout Templates, Page Templates, and Site Templates](https://help.liferay.com/hc/en-us/articles/360035581031-Understanding-the-Page-Structure#layout-templates-page-templates-and-site-templates) <a href="#layout-templates-page-templates-and-site-templates" id="layout-templates-page-templates-and-site-templates"></a>

The page layout within the `#content` Section is determined by the [Layout Template](https://help.liferay.com/hc/en-us/articles/360028726612-Layout-Templates). Several layout templates are included out-of-the-box. You can also [create custom layout templates manually](https://help.liferay.com/hc/en-us/articles/360028726612-Layout-Templates) or with the [Liferay Theme Generator’s layout sub-generator](https://help.liferay.com/hc/en-us/articles/360029147731-Generating-Layout-Templates-with-the-Theme-Generator).

Layout templates can be pre-configured depending on the [page type](https://help.liferay.com/hc/en-us/articles/360029132211-Creating-Pages) you choose when the page is created. Along with setting the types of portlets to include on the page, the page template may also define the default layout template for the page. Climbing further up the scope chain, you can create [Site Templates](https://help.liferay.com/hc/en-us/articles/360028819712-Building-Sites-from-Templates), which can define the pages, page templates, layout templates, and theme(s) to use for site pages.

### [Product Navigation Sidebars and Panels](https://help.liferay.com/hc/en-us/articles/360035581031-Understanding-the-Page-Structure#product-navigation-sidebars-and-panels) <a href="#product-navigation-sidebars-and-panels" id="product-navigation-sidebars-and-panels"></a>

The main page layout also contains a few notable sidebars an administrative user can trigger through the Control Menu. These are listed below:

* **Add Menu:** For adding portlets (widgets) and fragments (if applicable) to the page
* **Control Menu:** Provides the main navigation for accessing the Add Menu, Product Menu, and Simulation Panel
* **Product Menu:** Contains administrative apps, configuration settings, and user account settings, profile, and dashboard page
* **Simulation Panel:** Simulates how the page appears on different devices

![Figure 3: Remember to account for the product navigation sidebars and panels when styling your site.](https://help.liferay.com/hc/article\_attachments/360041593371)

Figure 3: Remember to account for the product navigation sidebars and panels when styling your site.

![Figure 4: The Add Menu pushes the main contents to the left.](https://help.liferay.com/hc/article\_attachments/360041590812)

Figure 4: The Add Menu pushes the main contents to the left.

![Figure 5: The Product Menu pushes the main contents to the right.](https://help.liferay.com/hc/article\_attachments/360041593351)

Figure 5: The Product Menu pushes the main contents to the right.

![Figure 6: The Simulation Panel pushes the main contents to the left.](https://help.liferay.com/hc/article\_attachments/360041590832)

Figure 6: The Simulation Panel pushes the main contents to the left.

When styling the page, you must keep the navigation menus in mind, especially for absolutely positioned elements, such as a fixed navbar. If the user is logged in and can view the Control Menu, the fixed navbar must have a top margin equal to the Control Menu’s height.

See the [Product Navigation articles](https://help.liferay.com/hc/en-us/articles/360031167931-Introduction-to-Product-Navigation) for more information on customizing these menus.

\
