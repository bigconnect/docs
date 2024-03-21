# Using Manual Actions

When you use the standalone trigger for object actions, the actions become manual instead of automatic. Trigger these actions using these methods:

* The object’s UI
* Dedicated action APIs
* Buttons in pages

This example creates a standalone action for sending notifications.

![You can use the standalone trigger to run any type of object action manually.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions/images/01.png)

See Defining Object Actions for more information.

### Trigger Actions via the Object Entry UI <a href="#trigger-actions-via-the-object-entry-ui" id="trigger-actions-via-the-object-entry-ui"></a>

Active standalone actions appear in the Actions menu for object entries. To trigger these actions via the UI,

1. Navigate to the object definition’s application page.
2.  Click the _Actions_ button (![Action Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired entry and select the standalone action.

    In this example, the action is Send Reminder.

    ![Click the Actions button for the desired entry and select the standalone action.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions/images/02.png)

### Trigger Actions via APIs <a href="#trigger-actions-via-apis" id="trigger-actions-via-apis"></a>

The portal automatically generates REST APIs for published object definitions. These include APIs for active standalone actions.

To view these APIs,

1. Open the portal ’s API Explorer at `[server]:[port]/o/api` (e.g., `localhost:8080/o/api`).
2. Click _REST Applications_ and select the desired object definition. Custom objects use the `c/[pluralobjectlabel]` naming pattern (e.g., `c/tickets`).

For each standalone action, the portal generates two PUT APIs: one that uses the entry’s ID and another that uses the entry’s external reference code (ERC).

![The portal generates two PUT APIs: one that uses the entry's ID and another that uses the entry's ERC.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions/images/03.png)

Calling either API triggers the action for the specified entry.

![Calling the API triggers the action for the specified entry.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions/images/04.png)

### Trigger Actions via Pages <a href="#trigger-actions-via-pages" id="trigger-actions-via-pages"></a>

You can map standalone actions to buttons on pages (content pages or display page templates).

If the portal’s out-of-the-box button fragment doesn’t satisfy your use case, you can map actions to custom buttons by adding the attributes `data-lfr-editable-id="action"` and `data-lfr-editable-type="action"` to their HTML tag. To learn more about adding attributes to fragments, see Fragment Specific Tags and Attributes Reference.

Here is an example of an action-ready custom button:

```html
<button class="btn btn-${configuration.buttonSize} btn-${configuration.buttonType}"
        data-lfr-editable-id="action" data-lfr-editable-type="action">
    Go Somewhere
</button>
```

#### Mapping Buttons to Object Actions in Content Pages <a href="#mapping-buttons-to-object-actions-in-content-pages" id="mapping-buttons-to-object-actions-in-content-pages"></a>

1. Create a Page.
2. Add a _Button_ fragment to your layout. See Adding Elements to Content Pages to learn how to edit a content page.
3. Click on the button once. Under Button Options, select _Action_ as the type. Now you can map actions to the button.
4. Click on the button a second time. An action menu opens on the right.
5. Under the Mapping tab, click on the _Item_ field or the _Select Item_ icon (![Select Item](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-app.png)) beside the field.
6.  Select your object from the list at the top.

    The available object entries appear at the bottom.
7.  Choose a specific object entry.

    ![Select a specific object entry](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions/images/05.png)
8. Select the _Field_ to be mapped to the button label.
9. Under the Action tab, select a specific object entry.
10. Choose an action.
11. Optionally, set the Success and Error interactions. See Managing Success and Error Interactions for more information.

TIP

You can use collection display fragments or form fragments with components mapped to actions. See Displaying Collections and Using Fragments to Build Forms for more information.

The button now executes the selected action when clicked.

#### Mapping Buttons to Object Actions in Display Page Templates <a href="#mapping-buttons-to-object-actions-in-display-page-templates" id="mapping-buttons-to-object-actions-in-display-page-templates"></a>

1.  Create a Display Page Template and choose your object as the content type.

    This makes your object the default source when mapping the button or the action.

    ![Choose your object as the content type when creating a display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions/images/06.png)
2. Add a _Button_ fragment to your layout.
3.  Click on the button once. Under Button Options, select _Action_ as the type.

    This configuration allows you to map actions to the button.
4.  Click on the button a second time.

    An action menu opens on the right. Under the Mapping and Action tabs, your object is already set as a default source.

    NOTE

    If you need to choose specific content, click on the _Source_ field, select _Specific Content_, and choose the specific _Item_.
5. Select the _Field_ to be mapped to the button label.
6. Under the Action tab, choose an action to trigger once a user clicks the button.
7. Optionally, set the Success and Error interactions. See Managing Success and Error Interactions for more information.

The button now executes the selected action when clicked.

#### Managing Success and Error Interactions <a href="#managing-success-and-error-interactions" id="managing-success-and-error-interactions"></a>

When a user clicks on a component mapped to an action, you can choose what happens when that action succeeds or fails.

There are four options available for both outcomes and an extra success interaction:

1. **None**: Nothing happens after the action is performed.
2. **Show Notification**: Notify the user. You can write your custom success/error message, preview the notification, and configure the page to reload after the action is triggered.
3. **Go to Page**: Navigate to a Success/Error Page in your site.
4. **Go to External URL**: Navigate to an external URL.
5. **Go to Entry Display Page** (Success Interactions only): When mapped to a specific object entry with an action in place, you can select this option as a success interaction. It redirects the user to a display page template.

![Go to Entry Display Page is an extra Success Interaction option that can be used to redirect the user to a display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions/images/07.png)

The selected option is executed after the user triggers the action.

### Action Permissions <a href="#action-permissions" id="action-permissions"></a>

The portal automatically generates permissions for managing which roles can trigger standalone actions. Each permission follows the `action.[actionName]` naming pattern (e.g., `action.sendReminder`). See Permissions Framework Integration for more information on object permissions.

![Standalone permissions use the action.actionName naming pattern.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions/images/08.png)
