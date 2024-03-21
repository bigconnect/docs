# Using Read-Only Fields

Some use cases may require data fields to be read-only. For example, data integrity may require some field values to remain immutable, or you may need to prevent unauthorized users from updating specific fields (e.g., state fields). You can configure any custom field to be read-only. This prevents users from directly updating the field’s value except under specified conditions. You can also use actions to update these field values.

### Adding a Read-Only Field <a href="#adding-a-read-only-field" id="adding-a-read-only-field"></a>

1. Begin editing the desired custom field.
2. Go to the _Advanced_ tab.
3.  Select a Read Only option:

    **True**: Mark the field as read-only.

    **False**: Allow users to change the field’s value.

    **Conditional**: Use expression builder to set conditions for when the field is read-only.

    ![Select a Read Only option: true, false, or conditional.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/using-read-only-fields/images/01.png)
4. Click _Save_.

### Example Use Case: Event Speaker Applications <a href="#example-use-case-event-speaker-applications" id="example-use-case-event-speaker-applications"></a>

Consider an event solution with these requirements:

* Users must submit applications to speak at a company event.
* Event managers must track each application’s status during the review process.
* Event managers must track when the state field was last updated.

To achieve this, first create a picklist for defining application statuses. Then, create an object for storing submissions and use the picklist as a state field. Set this field to read-only and add standalone actions for manually updating the field to specific values. Only assign the action permissions to event managers, so only they can update the state field.

For the final requirement, add a read-only date field. Then, create an automatic action that updates the field to the `modifiedDate` value whenever the state field is updated.

\
