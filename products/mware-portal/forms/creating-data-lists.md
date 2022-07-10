# Creating Data Lists

There are two places to create dynamic data lists:

1.  **Site Administration:** Open the Menu and expand your Site’s menu (the Site Administration menu). Then select _Content & Data_ → _Dynamic Data Lists_. This opens the Dynamic Data Lists screen. A table contains any existing lists. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030163771/icon-add.png)) to open _New List_ form.

    To add Dynamic Data Lists, you must have permission to access the Dynamic Data Lists app in Site Administration.
2.  **Dynamic Data Lists Display widget:** Navigate to the Site page where you want this widget and add it to the page from _Add_ (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030195452/icon-add-app.png)) → _Widgets_ → _Collaboration_ → _Dynamic Data Lists Display_. Then click the widget’s _Add List_ button. This opens the _New List_ form.

    To do this, you must have permission to create a new list in the widget.

Either option leads to the New List form:

1. Give the list a name and a description.
2. Select the list’s data definition: click _Select_ under the _Data Definition_ field, then click the definition you want to use.
3. To use a workflow with this list, select it from the _Workflow_ field.
4. To change the list’s default permissions, expand the form’s _Permissions_ section and make your selections.
5.  Click _Save_ to finish creating the list. Your new list appears in the table.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030195472/ddl-add-list.png" alt="Figure 1: The New List form." data-size="original">

    Figure 1: The New List form.

### Creating List Records <a href="#creating-list-records" id="creating-list-records"></a>

By default, only administrators have permission to create list records. Follow these steps to give other users this permission:

1. Navigate to _Content & Data_ → _Dynamic Data Lists_ in Site Administration.
2. Click _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030195392/icon-actions.png)) → _Permissions_ for the list getting the new permissions.
3. Select _Add Record_ for the Roles that should have that permission, then click _Save_. Allow unauthenticated Users to add records by giving Guest the Add Record permission.

Create new records in a list from the same places you can create the lists themselves:

1. **Site Administration:** In Site Administration, navigate to _Content & Data_ → _Dynamic Data Lists_. Click a list in the table to view any existing records, then click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030163771/icon-add.png)). This opens a form based on the list’s data definition, which you can then fill out and submit to create a new record. To do this, you must have permission to access the Dynamic Data Lists app in Site Administration.
2.  **Dynamic Data Lists Display widget:** See above for instructions on adding this widget to a page. You must then configure the widget to display a list’s records.

    To configure the widget to display a list’s records:

    * Click the widget’s _Select List_ button.
    * In the dialog that appears, select a list, click _Save_, then close the dialog. The widget then displays the list’s existing records.

    To add a record:

    * Click the widget’s _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030163771/icon-add.png)).
    * Fill out the form that appears and click _Publish_.

See the section below for more information on configuring the widget.

![Figure 2: Dynamic Data Lists Display widget.](https://liferay-support.zendesk.com/hc/article\_attachments/360030195432/ddl-widget.png)

### Configuring the Dynamic Data Lists Display Widget <a href="#configuring-the-dynamic-data-lists-display-widget" id="configuring-the-dynamic-data-lists-display-widget"></a>

The widget’s default display template isn’t exciting, but it shows the list’s contents, and with permission, add and/or edit list items. To configure the widget, click its _Options_ menu (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030195492/icon-app-options.png)) and select _Configuration_. This opens the Configuration dialog, with the _Setup_ tab selected by default. The Setup tab contains two other tabs:

**Lists:** Select the list that the widget displays. The currently displayed list appears at the top of the tab, while the available lists appear in a table. To change the widget’s list, select the list from the table and click _Save_.

**Optional Configuration:**

**Display Template:** Select the display template for the list.

**Form Template:** Select the form template for the list.

**Editable:** Whether users can add records to the widget’s list.

**Form View:** Whether to display the Add Record form by default, instead of the List View. Note that even without this selected, users can still add records via the widget’s _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030163771/icon-add.png)).

**Spreadsheet View:** Whether the List View displays each record in a row, with columns for the record attributes.

When finished, click _Save_ and close the Configuration dialog.

![Figure 3: The Dynamic Data Lists Display widgets optional configuration.](https://liferay-support.zendesk.com/hc/article\_attachments/360030163791/ddl-widget-options.png)
