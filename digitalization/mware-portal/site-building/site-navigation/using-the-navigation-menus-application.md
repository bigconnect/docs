# Using the Navigation Menus Application

Use the Navigation Menus application to create custom menus for site pages and content. With it, you can determine which elements are included in a menu, and how they’re organized. Once created, you can display your custom menus using page fragments or widgets.

NOTE

For version 7.3 and earlier, pages are the only entity that can be added to a navigation menu. As of version 7.4+, navigation menus support adding additional entity types to a navigation menu.

![Creating custom menus for site pages and content.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/01.png)

TIP

On version 7.3 +, you can use custom fields to add more information to items in navigation menus (under _Site Navigation Menu Item_ in the _Custom Fields_ menu). You can then use this information with Application Display Templates to further customize your navigation menus.

### Creating Navigation Menus <a href="#creating-navigation-menus" id="creating-navigation-menus"></a>

Follow these steps to create a Navigation Menu:

1.  Open the _Site Menu_ ( ![Site menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Site Builder_ → _Navigation Menus_.

    ![Access navigation menus in the site menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/02.png)
2. Click _Add_ ( ![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ).
3.  Enter a _Name_ for the navigation menu.

    ![Create a navigation menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/03.png)
4. Click _Save_.

The new custom menu, where you can begin adding elements (e.g., pages, documents, and categories), appears.

#### Adding New Elements to a Navigation Menu <a href="#adding-new-elements-to-a-navigation-menu" id="adding-new-elements-to-a-navigation-menu"></a>

1. Click _Add_ ( ![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ).
2.  Select the type of element you want to add to the menu (e.g., page, document, or category). This opens an item selector window.

    You can add multiple elements to the same menu.

![Select the type of element you want to add to the menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/04.png)

1.  Select the elements you want to add and click _Select_.

    You can select multiple elements.

    IMPORTANT

    When the added element does not have an associated display page, a warning appears. Items without a display page do not have links and are hidden from menus.

    ![A warning appears when the added element does not have an associated display page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/05.png)
2.  Optionally, drag and drop menu elements to reorganize them or arrange them in a hierarchy.

    TIP

    In addition to structuring menu elements into a hierarchy, you can also use the _Submenu_ type to add subsections to a menu. If you delete an element with child elements, the child elements are also removed.

    ![Adding elements to navigation menus.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/06.png)
3.  Optionally, you can add a child navigation menu element from the navigation menu tree.

    Click _Actions_ ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired navigation menu element, select _Add Child_, and choose a child element type.

    ![Add child navigation menu elements right from the navigation menu tree](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/07.png)

    TIP

    Click on one of the navigation menu elements, and use the keyboard arrows to navigate through the navigation menu tree.
4.  Optionally, define custom display names for each of your menu elements. You can define custom names for page elements or for any content type elements that use display pages (including blog entries, web content articles, documents, vocabularies, and categories).

    NOTE

    You can only define custom display names for content type elements using display pages on versions 7.4 +.

    To do this, select a menu element, check _Use Custom Name_ in the side panel, enter a name, and click _Save_.

    ![Customize the display of menu elements.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/08.png)

All changes to the menu are saved.

#### Using Dynamic Vocabulary Elements <a href="#using-dynamic-vocabulary-elements" id="using-dynamic-vocabulary-elements"></a>

Vocabulary navigation menu elements are dynamic; when one is displayed on a page, it adds all of the categories and subcategories in its current hierarchy instead of a single element. Any changes to the vocabulary’s contents are automatically reflected in the menu.

Dynamic vocabulary elements allow you to quickly and easily update your navigation menus when you have changing hierarchies of items. For example, you can use categories to represent types of products in a [catalog](https://learn.liferay.com/commerce/latest/en/product-management/catalogs/creating-a-new-catalog.html), and then let any updates to your categories automatically update any menus using the vocabulary. Then, depending on the display page used for those categories, any changes to the categorization may automatically reflect in the pages and the navigation menu.

![Dynamic vocabulary elements automatically update to reflect their hierarchy of categories in your navigation menus.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/09.png)

#### Previewing Navigation Menus <a href="#previewing-navigation-menus" id="previewing-navigation-menus"></a>

You can a preview a navigation menu while you are editing it to see how it looks when displayed on a page. Click the _Eye icon_ ( ![Eye icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-view.png) ) to open a popup window with a preview of the navigation menu.

![Preview a navigation menu to see how it looks when displayed on a page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/10.png)

You can also select different options from the drop-down menu to see different styles the menu can be displayed in, such as the List or Tabs styles.

### Using Menus During Page Creation <a href="#using-menus-during-page-creation" id="using-menus-during-page-creation"></a>

When users are creating site pages, they are shown a list of all existing navigation menus. You can modify this behavior by hiding specific menus from the list of available menus.

![Add the new page to an existing navigation menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/11.png)

Follow these steps to configure this behavior for a Navigation Menu:

1. Open the _Site Menu_ ( ![Site menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Site Builder_ → _Navigation Menus_.
2. Select the desired menu.
3. Click _Actions_ ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ).
4.  Uncheck/Check the setting.

    ![You can select from existing navigationmenus when creating site pages](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/12.png)
5. Click _Save_.

### Configuring Navigation Menu Type <a href="#configuring-navigation-menu-type" id="configuring-navigation-menu-type"></a>

Follow these steps to configure the navigation menu’s type:

1. Open the _Site Menu_ ( ![Site menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Site Builder_ → _Navigation Menus_.
2. Click _Actions_ ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired menu.
3.  Select the desired navigation type: Primary Navigation, Secondary Navigation, or Social Navigation.

    ![Select the desired navigation type.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/using-the-navigation-menus-application/images/13.png)

    Once selected, each menu’s configuration appears in the Marked As column.

| Navigation Menu Type     | Usage                                                                                                      |
| ------------------------ | ---------------------------------------------------------------------------------------------------------- |
| **Primary Navigation**   | Primary Navigation is the main navigation for a page.                                                      |
| **Secondary Navigation** | Secondary Navigation is a second level of navigation, possibly a sidebar or a separate menu within a page. |
| **Social Navigation**    | Social Navigation is for menus that contain links for sharing content on social media or similar tasks.    |

### Deleting a Navigation Menu <a href="#deleting-a-navigation-menu" id="deleting-a-navigation-menu"></a>

1. Open the _Site Menu_ ( ![Site menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Site Builder_ → _Navigation Menus_.
2. Click _Actions_ ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired menu then click _Delete_.
3. Click _OK_ in the confirmation.

The navigation menu is deleted.

\
