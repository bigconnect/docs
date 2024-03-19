# Adding and Managing Custom States

Some use cases may require assigning multiple states to an object entry at the same time. For example, when building an Order application, you may need to assign and manage order, payment, and delivery statuses for the same entry, each with its own flow and associated business logic. Using Liferay picklists with custom objects, you can achieve this functionality.

With [picklists](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists), you can define lists of strings and use them as object fields to define custom states for entries. Once added to an object definition, you can design the flow for each state and use [validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations) and [actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions) to add business logic for different states. States can also be used alongside [workflow statuses](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/enabling-workflows-for-objects) to define more complex review and approval processes.

![Use picklists to add custom states to object definitions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states/images/01.png)

### [Adding a State Field to Object Definitions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states#adding-a-state-field-to-object-definitions) <a href="#adding-a-state-field-to-object-definitions" id="adding-a-state-field-to-object-definitions"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Picklists_.
2.  Create a picklist with the desired items. These items define object entry states. See [Creating Picklists](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/creating-picklists) for more detailed steps.

    NOTE

    You can add, remove, or edit picklist items for both draft and published object definitions at any time.

    ![Create a picklist with the desired items.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states/images/02.png)
3. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
4. Begin editing the desired custom object.
5.  Go to the _Fields_ tab and click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).

    ![Begin adding a field to the object definition.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states/images/03.png)
6. Enter a _Label_ and _Field Name_.
7. Select the _Picklist type_ and choose the desired list.
8.  Toggle _Mark as State_.

    NOTE

    State fields are set automatically to _mandatory_, and you cannot change them to optional.
9.  Select a _Default Value_ for the state field.

    ![Select a picklist, toggle Mark as State, and set a default value for the field.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states/images/04.png)
10. Click _Save_.

After adding the field to a definition, you can [set up a flow](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states#setting-up-a-flow-for-state-fields) to determine which transitions are available for each state.

### [Setting Up a Flow for State Fields](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states#setting-up-a-flow-for-state-fields) <a href="#setting-up-a-flow-for-state-fields" id="setting-up-a-flow-for-state-fields"></a>

By default, each state can transition to all other states. However, you can restrict transitions to ensure the entry goes through the desired process.

Follow these steps to set up a custom flow for state fields:

1.  While editing a custom object, go to the _State Manager_ tab and select the desired _state field_.

    ![Go to the State Manager tab to set up a flow for state fields.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states/images/05.png)
2.  Under Next Status, click the drop-down menus and check the boxes to determine available transitions for each state.

    You can select any number of transitions for a state.

    ![Determine the available transitions between states.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states/images/06.png)
3. Click _Save_.

Once saved, users can only transition an entry’s state according to your defined flow.

![Set up a custom flow determines available transitions for each state.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states/images/07.png)
