# Picklist Fields

Picklists define lists of text values usable as single select or multi-select fields in object definitions. By default, single select fields appear as drop-down menus, while multi-select fields appear as checkboxes.

You can configure a single select field to use a default value, which can be static or dynamic. Additionally, you can use single select fields to define custom states for object entries and integrate [validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations), [actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions), and [workflow processes](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/enabling-workflows-for-objects) with the object definition to implement complex business logic.

### [Setting Default Values](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/picklist-fields#setting-default-values) <a href="#setting-default-values" id="setting-default-values"></a>

1. Begin editing the desired Picklist field.
2.  Go to the _Advanced_ tab and toggle _Use Default Value_.

    ![Toggle Use Default Value and select either Input as Value or Expression Builder.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/picklist-fields/images/01.png)
3.  Determine how to set the default value.

    _Input as Value_: Select a value from the picklist.

    _Expression Builder_: Create an expression that sets the value dynamically.
4. Click _Save_.

Now when users create entries, the picklist field defaults to the set value.

IMPORTANT

You cannot delete a list if it is used by an object field, though you can edit and remove list items at any time.

Updating or deleting a picklist item automatically updates all object entries using the item value.

### [Using Picklist Fields with APIs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/picklist-fields#using-picklist-fields-with-apis) <a href="#using-picklist-fields-with-apis" id="using-picklist-fields-with-apis"></a>

When making POST, PATCH, and PUT API calls for objects that include a picklist field, you must use the following request payload formats for Picklist and Multiselect Picklist fields, respectively.

#### [Picklist Field Type](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/picklist-fields#picklist-field-type) <a href="#picklist-field-type" id="picklist-field-type"></a>

```json
{
      "picklistExample": {
        "key": "firstOption",
        "name": "First Option"
      }
}
```

#### [Multiselect Picklist Field Type](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/picklist-fields#multiselect-picklist-field-type) <a href="#multiselect-picklist-field-type" id="multiselect-picklist-field-type"></a>

```json
{
      "picklistExample": [
        {
          "key": "firstOption",
          "name": "First Option"
        },
        {
          "key": "secondOption",
          "name": "Second Option"
        }
      ]
}
```
