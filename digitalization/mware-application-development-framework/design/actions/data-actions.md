# Data actions

### Save Row

Saving a row can mean two things: updating or adding a new row. Depending on what you want to do you will have to structure this action differently.

If you're adding, updating or viewing any row the easiest way is to add a Form Block component. Here you can choose to create, update or read, depending on your selection you will be prompted with additional fields. For update and read you will have to pass in the `Row ID` into the row field, this is what will be use to update the specific record back to your chosen data source. In this instance you don't have to worry about the `Save Row` action as it already preconfigured for you. (You can eject blocks if you want to see a good example of how the `Save row` action is configured). However, if you did go down the regular Form route your save row action would look something to the below.

<figure><img src="https://files.readme.io/2ce7e8dac7a57880ecc88fde95d7f1daa937dc3983ed22e02ff76c33d443c570-Screenshot_2024-09-04_at_10.16.01.png" alt=""><figcaption></figcaption></figure>

Updating a row can be done in a number of different ways. If you have a structure containing a Data provider and a Repeater, selecting the repeater as your Datasource will make it so you can correctly get the values from the specific row. To change a field you then take advantage of the add column button that is at the bottom. Click it, select the field you want to change, and type in the value you want to change it to (or use a binding). A descriptive image of this is shown below:

<figure><img src="https://files.readme.io/bf0a134c427d3c1a08a21bf40381c561e719097eb1cefbffc2c813141046d634-Screenshot_2024-09-04_at_11.42.03.png" alt=""><figcaption></figcaption></figure>

If you are not using a repeater, an alternative way to update a row is to add the **\_id** column and provide a value that matches an existing id.\
For example, you could be passing a task id in the URL:

<figure><img src="https://files.readme.io/a76c3a3-Screenshot_2022-05-10_at_16.48.11.png" alt=""><figcaption></figcaption></figure>

### Duplicate Row

This action allows you to clone a row into any table with shared column names.\
If you are performing this action from a row button click, then no **\_id** needs to be provided.

Alternatively, your button could be nested within a Repeater. The below example shows how this would be setup with a repeater.

<figure><img src="https://files.readme.io/ac788efae56722be540fffd8114fd18f744b76cad019378f2fde70d21d489e2b-Screenshot_2024-09-04_at_12.25.26.png" alt=""><figcaption></figcaption></figure>

### Delete Row

This action is used to delete rows from your data source. It's used much in the same way that you would when updating a row. The main difference being you don't have to select a data source. You only have to select the table and pass the correct `Row ID` binding. For this example we will pass the `Row ID` via a URL variable, then the `Row ID` will be passed into the delete on click action used to delete the record from the Budibase DB.

<figure><img src="https://files.readme.io/064db25f7fc894975bef01482ed14ebece1201d4df3ce551416a1246cdf3e64e-Screenshot_2024-09-04_at_12.28.06.png" alt=""><figcaption></figcaption></figure>

### Fetch row

This action allows you to fetch a table row from any table within your app. This action works the same `Delete Row` where you have select a table and reference the `Row ID`.

<figure><img src="https://files.readme.io/35152f1-Screenshot_2023-05-18_at_11.07.01.png" alt=""><figcaption></figcaption></figure>

This can be used alongside other actions a good example of this is with [App state](https://docs.budibase.com/docs/app-state). You can fetch the row then create a state to hold all the data related to the row fetched. This can then be used to as the basis to bind to different components within your app.

Below there is a simple demo showing this in action. This is a very simple demo and its only outputting the entire fetch object to a paragraph component. You can break this up into multiple components and output different parts of the object if you so choose.

<figure><img src="https://files.readme.io/dcc05cffdaad59549869fdb6aacc76fa93f5c42e0f61930c309b1c2ac9c30ebf-Screen_Recording_2024-09-04_at_12.35.35.gif" alt=""><figcaption></figcaption></figure>

### Execute Query

The `Execute Query` action requires separate configuration as the queries need to be setup before they can be used by this action within your apps. There are many ways to use this action. For instances, you can use it to send the query results to a REST API, retrieve results from a query created using an External data source, and so on.

In the example below we will be focusing on using this with a REST datasource. This example won't be too in-depth and shall only show a basic get REST query. This will also be used in a similar fashion to `Fetch rows` action, where we will storing it as state to display the object.

<figure><img src="https://files.readme.io/19d84155946db9499a2f5dd4a33963863687f9d8ba802d4280903cf3c9a693ef-Screenshot_2024-09-04_at_13.05.04.png" alt=""><figcaption></figcaption></figure>

For anything more advance have a read through the REST API docs. It will show you how to import and configure everything necessary to perform a more advanced action.

<figure><img src="https://files.readme.io/8daaae841b75b4a7f39d5659b900bc6067f7ee2cb14c4bd8a34a8c3cf40ca018-Screen_Recording_2024-09-04_at_13.06.06.gif" alt=""><figcaption></figcaption></figure>

### Refresh Data Provider

This is mainly used for External datasources for example the REST API as it requires the recalling of queries in order to refresh Data providers after other actions have been performed. A good example for this is when you update a user record within a Form inside a Side panel. Once updated you would want to chain the `Refresh Data Provider` action afterwards to refresh the changes made without having to refresh the page to see these changes. Another example of this might be for the Candlestick Chart where new data is available over time, and it may be useful to refresh at the click of a button.

### Update State

Allows you to set a state value, or delete the key entirely. When entering an identifying _Key_ you can select an existing one from the dropdown, or simply manually type into the text box to create a new key.

<figure><img src="https://files.readme.io/1ddf044-Screenshot_2024-01-11_at_10.57.54.png" alt=""><figcaption></figcaption></figure>

Your state variable can then be accessed, anywhere in your app, via the state Binding, in this case: `{{ State.employee_id }}`.

See App state for further reading and examples.

### Upload File To S3

To be used with the S3 File Upload component.

### Export Data

This action is used to export selected rows within a table to either a CSV or JSON file. To set this up quickly add a Table component to a screen, and set it to display the Employees table from the sample DB data. Add a button component underneath, and add an On Click action "Export Data".

<figure><img src="https://files.readme.io/0dcfa20025a361af1d05cc8f63027546879f1adafdc7461f02d82d3af48206c6-Screenshot_2024-09-04_at_14.30.10.png" alt=""><figcaption></figcaption></figure>

Once this has been done, it should export the rows selected and output it as the selected export.

<figure><img src="https://files.readme.io/541f4653cc4daef0e0d44daf04a123625b0441d7ef39c572ff16bf3ae9a6b8eb-Screen_Recording_2024-09-04_at_14.32.15.gif" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/bb41e11-Screenshot_2023-05-19_at_07.57.15.png" alt=""><figcaption></figcaption></figure>

**Configure delimiter**

The default delimiter (separator) for CSV exports is a comma `,` . It is however possible to change this to one of a preset list of options. This feature does not apply to JSON file exports.

<figure><img src="https://files.readme.io/5b707976952429c2a8fa88fdc4b12aad7f1db43b28a59c7240487fb251ec5351-Screenshot_2024-09-04_at_14.37.02.png" alt="Using the tab key delimiter"><figcaption><p>Using the tab key delimiter</p></figcaption></figure>

**Configure export columns**

By default _all_ table columns will be included in the export, however you can use a subset and provide custom labels.

Click the _Export columns_ button and add the specific columns you want to export. You can also drag-and-drop the columns to rearrange the column order in the export.

<figure><img src="https://files.readme.io/4e7070d-Screenshot_2024-02-27_at_13.07.38.png" alt="Only include these three columns in the data export"><figcaption><p>Only include these three columns in the data export</p></figcaption></figure>

<figure><img src="https://files.readme.io/af5c4ac-csv.png" alt="Exported CSV file"><figcaption><p>Exported CSV file</p></figcaption></figure>

> ### 🚧Limitations
>
> **Export Data** does not work with the REST API datasource or other Custom queries.

### Clear Row Selection

> ### 🚧This action is only compatible with the now-deprecated Table Block.

Deselects any selected rows of the chosen table.

<figure><img src="https://files.readme.io/a26a4a3-clear_row_selection.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/7dea71c-selected_rows.gif" alt=""><figcaption></figcaption></figure>

Can also be useful in combination with the **Open Side Panel** action.

### Download File

Initiates the download of a specified file or files.

#### Download from Attachment

<figure><img src="https://files.readme.io/44a00e9-image.png" alt=""><figcaption></figcaption></figure>

Specify the table and column from which the file(s) will be downloaded - only columns that have the type `Single Attachment` or `Multi Attachment` will appear in the `Column` dropdown.

Downloading from Multi Attachment columns will result in a `.zip` file containing all items being downloaded with a filename comprising of the name of the Display Column followed by the name of the Multi Attachment Column.

#### Download from URL

<figure><img src="https://files.readme.io/73c0df1-image.png" alt=""><figcaption></figcaption></figure>

Specify the URL of the file to be downloaded. If `File name` is left blank the file downloaded will be named "undefined" and appended with the file extension.
