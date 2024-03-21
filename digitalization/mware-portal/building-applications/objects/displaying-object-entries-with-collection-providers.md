# Displaying Object Entries with Collection Providers

When you publish an object definition, the portal creates a collection provider for it. You can use this provider with the Collection Display fragment and collection pages to display its entries. Objects support all collection display features, including filtering, styling, and display options.

### Using an Object’s Collection Provider <a href="#using-an-objects-collection-provider" id="using-an-objects-collection-provider"></a>

1.  Create a new content page or display page template. Alternatively, begin editing ( ![Edit Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit-pencil.png) ) an existing one.

    NOTE

    You can also create a collection page using the collection provider. See Collections and Collection Pages for more information.
2. Drag and drop the _Collection Display_ fragment into the page or template from the Fragments and Widgets (![Fragments and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png)) sidebar.
3.  Select the _Collection Display Fragment_ to access its configuration options and click _Select Collection_ in the General tab.

    ![Select the fragment and click Select Collection.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries-with-collection-providers/images/01.png)
4.  In the modal window, click the _Collection Providers_ tab and select the _object’s provider_.

    ![In the Collection Providers tab, select the object's provider.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries-with-collection-providers/images/02.png)
5.  (Optional) Filter the displayed object entries. You can filter by any picklist or boolean fields in the object:

    Click _Collection Options_ (![Collection Options Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Filter Collection_.

    ![Click the Collection Options button and select Filter Collection.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries-with-collection-providers/images/03.png)

    Then select the desired _filters_ and click _Apply_.

    ![Select the Filters to apply.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries-with-collection-providers/images/04.png)

After selecting the desired provider, use additional fragment options to determine how Object entries appear, including _List Style_ and _Pagination_.

### Mapping Fragments to Object Fields <a href="#mapping-fragments-to-object-fields" id="mapping-fragments-to-object-fields"></a>

When you select an object for a Collection Display fragment, the object is set as its mapping source. Now you can add fragments to it and map their sub-elements to object fields to create flexible page displays for individual object entries. This is true for collection pages as well.

For example, you can embed a button in a Collection Display fragment mapped to an object and then configure the button’s link to display the name of each item in the collection.

![Map fragment sub-elements in Collection Display fragments to object fields.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries-with-collection-providers/images/05.png)

### Mapping Fragments to Related Object Fields <a href="#mapping-fragments-to-related-object-fields" id="mapping-fragments-to-related-object-fields"></a>

IMPORTANT

Mapping fragment elements to related object fields is behind a release feature flag. Read Release Feature Flags for more information.

If the mapped object is on the child side of a one-to-many relationship, you can map fragment elements to fields in the parent object. The embedded Collection Display fragment uses a unique related items collection provider to form the new collection.

For example, if a `City` object (child) is related to a `Country` object (parent), you can map fragments to fields in the `Country` object.

To map fragments to a related object’s fields,

1. Use the Object’s collection provider with a Collection Display fragment.
2.  Embed another Collection Display fragment within the initial Collection Display.

    TIP

    Using the _Browser_ tab in the page edit menu can help you position the fragment more precisely within the page hierarchy.

    ![Use the Browser tab to help properly embed the Collection Display fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries-with-collection-providers/images/06.png)
3. Click _Select Collection_ in the new Collection Display fragment’s configuration options.
4. Click the _Related Items Collection Providers_ tab.
5.  Select the collection provider with the name of the object’s relationship. The associated object type appears under the relationship’s name. In this example, the relationship is called `cities in country`:

    ![Select the relationship that you created with between the displayed object and another object type.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries-with-collection-providers/images/07.png)

Now you can embed fragments in the new Collection Display and map them to fields in the related object.

![Map fragment sub-elements to fields in parent objects.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries-with-collection-providers/images/08.png)
