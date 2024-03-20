# Setting Widget Scopes

By default, portal’s page widgets are _scoped_ by site. This means all widget instances in a site share the same data and display the same content. For example, adding the Message Boards widget to two pages in the same site creates two copies of the same widget, because they use the same data. Adding the Message Boards widget to two different sites creates two different Message Boards with different data.

You can, however, scope widget instances to the page instead of to the site. Then you can add the Message Boards widget to multiple site pages, and each instance has its own categories and threads.

The portal provides three scope options for page widgets:

| Scope   | Description                                                                                                                                                                        |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Default | Scope widget data to the current site.                                                                                                                                             |
| Global  | Scope widget data to the Global site. For this scope, you can only manage the widget’s content from the Global site.                                                               |
| Page    | Scope widget data to an individual page. You must create the scope when configuring a page widget. Once created, other widgets can use it, including widgets from different pages. |

Follow these steps to configure a widget’s scope:

1. Begin editing a page.
2. Hover over the desired widget, click its _Options_ button (![Options Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)), and select _Configuration_.
3.  Go to the _Scope_ tab and use the drop-down menu to select a scope.

    This menu includes the current site (the current site refers to the default scope), the Global site, the current page, and any other page scopes available in the site.

    If you haven’t created a scope for the current page, its name is followed by `(Create New)` (e.g., Home (Create New)).

    ![In the Scope tab, use the drop-down menu to select a scope.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-widgets/configuring-widgets/setting-widget-scopes/images/01.png)
4. Click _Save_.

Once you create a new widget scope, a Scope Configuration button (![Scope Configuration Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog.png)) appears under Content & Data in the Site Menu (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)). You can use it to select the default scope or available page scopes. Your selection changes the content that appears when you manage each type.

![Use the Scope Configuration button under Content & Data to determine which scope to manage content for.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-widgets/configuring-widgets/setting-widget-scopes/images/02.png)

NOTE

You can only delete a scope by deleting its associated page.

\
