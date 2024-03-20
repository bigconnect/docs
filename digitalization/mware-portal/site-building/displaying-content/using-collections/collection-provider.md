# Collection Provider

Collection providers group items into collections for display on your pages. Several are available out-of-the-box, or you can create them using the Information Framework to group items into the collections you need.

NOTE

The term _items_ can refer to more than content, such as products and categories, and they can be located outside the current site or portal instance.

### Default Collection Providers <a href="#default-collection-providers" id="default-collection-providers"></a>

The following collection providers are available out-of-the-box and function the same as the widgets with the same name:

* Most Viewed Assets: displays the most viewed assets on the current site from most to least.
* Highest Rated Assets: displays rated assets from highest to lowest.
* Recent Content: displays recently modified assets from the most recent to least recent.

![When configuring a collection display fragment, you can choose collection providers such as Most Viewed Assets, Highest Rated Assets, and Recent Content.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/collection-providers/images/01.png)

Some collection providers are created automatically by the portal’s applications. For example,

* When an object is published, the portal creates a collection provider for it. See Using the Collection Provider for Objects for more information.
* When a search blueprint is published, the portal creates a collection provider for it. See Collections with Search Blueprints for more information.

### Related Items Collection Providers <a href="#related-items-collection-providers" id="related-items-collection-providers"></a>

Related items collection providers show lists of items related to the main items being displayed. You can access these collection providers in two ways:

1. By adding a collection display fragment to a display page.
2. By nesting collections within a collection display fragment on a content page.

![When configuring a collection display fragment, you can choose Related Items collection providers.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/collection-providers/images/02.png)

**Related Assets:** shows items related to the main item on display.

**Categories for This Item:** shows the categories of the current item or asset.

**Items with Categories in the Same Vocabularies:** shows items in the same vocabulary as the main asset being viewed. For example, if the main asset has the category _football_, and _football_ is part of the _sports_ vocabulary, all other categories under the _sports_ vocabulary are shown.

**Items with the Same Categories:** shows items with the same category as the main item being viewed. For example, if the main item has the category _football_, items with the same category appear. You can further refine the results by selecting _Any Category of the Same Vocabulary_ or _A Specific Category_ under _Advanced Rule_.

![Items with the Same Categories requires you to choose an item type.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/collection-providers/images/04.png)

**Objects in a Relationship:** (when viewing an object) shows other objects that have a relationship to the object being viewed. A new related item collection provider appears for every relationship the main object has. For example, if the main object is a _country_ and has a one-to-many relationship with a _city_ object (with many cities mapped to one country), all city object entries related to that country entry are shown.

#### Advanced Rules for Items with the Same Categories <a href="#advanced-rules-for-items-with-the-same-categories" id="advanced-rules-for-items-with-the-same-categories"></a>

You can further refine displayed items while filtering them based on their types.

![When configuring the Items with the Same Categories provider, you can choose advanced rules from the drop-down.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/collection-providers/images/03.png)

**Any Category of the Same Vocabulary:** shows all items with categories in the same vocabulary as the main asset. For example, if the main asset has the category _football_ all other items in the _sports_ vocabulary are shown.

**A Specific Category:** shows results from an additional category to further refine the list. For example, you can add an additional category, _breaking news_, to display assets with the category _football_ and _breaking news_, which shows assets matching breaking news for football.

TIP

By default, selecting a parent category automatically selects its child categories. This can be changed in the Control Panel under _Instance Settings_.
