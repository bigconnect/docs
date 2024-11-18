# Table

The table allows you to copy your DB and SQL tables as seen in the **Data** section into your apps for end-users.

It is the quickest, most direct way to provide your users with a CRUD interface.

Add a table in a couple of clicks!

1. In the _Design_ section, click `Add component`.
2. Under the _Blocks_ category, click `table`.

> ### 👍
>
> You can also add a table by searching for "Table" and pressing the Enter key.

<figure><img src="https://files.readme.io/f7f25c9-Screenshot_2024-04-10_at_11.28.49.png" alt="Adding the table"><figcaption><p>Adding the table</p></figcaption></figure>

<figure><img src="https://files.readme.io/7306362-Screenshot_2023-11-27_at_11.16.44.png" alt=""><figcaption></figcaption></figure>

With the table added, you will want to select a _Table_ in the Settings Panel.

### Configure columns

Table columns can be configured under the _Columns_ section of the settings. Click the cog icon beside a column to change its label.

Click on `Configure columns` to determine which columns should be included in your table view and to change their labels. The underlying database column name will still be shown above the label for reference.

<figure><img src="https://files.readme.io/5952240-Screenshot_2023-11-27_at_11.21.16.png" alt="Giving the &#x27;Last Name&#x27; column the display label &#x27;Surname&#x27;"><figcaption><p>Giving the 'Last Name' column the display label 'Surname'</p></figcaption></figure>

Columns can be shown or hidden with the toggle switches, and the column order can be set by dragging and dropping columns.

> ### 📘In-app column width and order
>
> The initial width and order of your columns will match the table in the **Data** section.
>
> Users can use the mouse to drag columns to change their order, or drag edges to resize the width. These changes will only apply to the users' session.
>
> Also note that the Display column will always be pinned.

### On row click

Click `Define actions` to run Actions on click of a row.

You will be able to use the `{{ Clicked row }}` Binding to access the object data of the clicked row.

![](https://files.readme.io/d0d0c99-Screenshot\_2022-12-14\_at\_10.35.58.png)

<figure><img src="https://files.readme.io/9c79cbb-Screenshot_2022-12-14_at_11.15.08.png" alt=""><figcaption></figcaption></figure>

If you wanted to access a property of the clicked row, for example the **\_id**, then you would write your binding like so: `{{ Clicked row._id }}`

### Sorting

Select a _Sort column_ and _Sort order_ to set the initial default sorting; either ascending or descending. You can also set an initial sort on a column that is not being displayed.

A user can still override this sort by clicking the three dots beside a column. Text fields can be sorted alphabetically, whereas number fields will be sorted numerically.

### Row settings

The table offers a few row settings, including a mix of style and function.

| Setting              | Description                                                                       |
| -------------------- | --------------------------------------------------------------------------------- |
| Row height           | Set the height of the table rows (Small, Medium and Large). The default is small. |
| High contrast        | If checked, the table rows will be striped.                                       |
| Add/Edit/Delete rows | Uncheck to prevent users from adding/editing/deleting rows.                       |

If you want to increase the number of rows displayed on screen, simply increase the height of the table in the _Styles_ tab.

<figure><img src="https://files.readme.io/b6e89cf-Screenshot_2024-04-10_at_12.06.35.png" alt=""><figcaption></figcaption></figure>

### Search

Search can be added to a table through the use of Bindings and filtering Using a form and text input.

Some field types also allow searching directly within the table's column. If you hover over a column and see a magnifying glass icon, you can click it to add a search input field in the column header. Typing into this field will perform a basic search on the data in that column.

### Selected rows binding

If your table has the _Allow row selection_ checkbox ticked, then you will be able to make use of the `Selected Rows` Binding.

This can be found underneath your table within the bindings drawer:

<figure><img src="https://files.readme.io/0bc2c7d-Screenshot_2024-04-10_at_12.08.42.png" alt=""><figcaption></figcaption></figure>

When using this binding, an array of the selected row data will be made available:

<figure><img src="https://files.readme.io/ce9e495-Screenshot_2024-04-10_at_12.11.10.png" alt=""><figcaption></figcaption></figure>

This binding can be used to Delete the selected rows:

<figure><img src="https://files.readme.io/f04f1fa302d4fe6305a3930b1a97bd5b60141fb98cc89d98486e2fb073d13198-Screenshot_2024-09-11_at_11.51.54.png" alt="Delete selected rows"><figcaption><p>Delete selected rows</p></figcaption></figure>

You could also [pluck](https://docs.budibase.com/docs/array-helpers#pluck) a field from the selected rows, such as an ID, to show which rows have been selected in a readable way: `{{ pluck New Table.Selected rows 'id' }}`

<figure><img src="https://files.readme.io/f7af098-Screenshot_2024-04-10_at_12.15.21.png" alt="Displaying the IDs of the selected rows using the &#x27;pluck&#x27; helper"><figcaption><p>Displaying the IDs of the selected rows using the 'pluck' helper</p></figcaption></figure>

Furthermore, you can combine `pluck` with `sum` to count a total, such as the kilogram _weight_ of the selected items: `{{ toFixed (sum (pluck New Table 1.Selected Rows 'KGS')) 2 }} KG`.

[toFixed](https://docs.budibase.com/docs/number-helpers#tofixed) in this case ensures there are always two decimals points in the printed value.

<figure><img src="https://files.readme.io/b0028e7-Screenshot_2024-02-14_at_11.54.11.png" alt="Displaying the sum total weight of the selected rows"><figcaption><p>Displaying the sum total weight of the selected rows</p></figcaption></figure>

#### Conditional formatting

With conditional formatting, you can now add conditional logic to either a cell or rows within your table. To make your formatting dynamic, you can set rules that change the format of cells based on their value. This can be done on a range of fields types, options, formulas, strings, numbers etc.

You can find conditional formatting options under the column settings:

<figure><img src="https://files.readme.io/7bfe0c0-Screenshot_2024-08-08_at_10.11.36.png" alt=""><figcaption></figcaption></figure>

The above will open a new drawer when clicked, were you can begin adding your conditions in relation to the column you have selected.

<figure><img src="https://files.readme.io/17c5739-Screenshot_2024-08-08_at_10.16.30.png" alt=""><figcaption></figcaption></figure>

There are a few settings you need to be aware of.

* Cell \~ This will only apply styling to the specific cell chosen
* Row \~ This will apply styling to the entire row whenever the condition is met
* Background colour \~ This allows you to change the background colour of a cell or row
* Text colour \~ This allows you to change only the text colour of the cell or row

It’s worth noting that the order of your conditions matter, as Mware Application Development Framework processes these conditions from top to bottom. This is more specific to numbers as depending on the situation you may want to apply styling to higher values first and then apply styling to lower values next. Therefore you will need to order the conditions accordingly.

#### Deprecation - Selected row ids

Prior to **v2.19.2**, the selected rows binding provided a list of Row \_ids only. This binding has been kept for backwards compatibility, however in future it will be removed. This change has been made because the new binding is more useful, allowing you to do the calculations outlined above.

<figure><img src="https://files.readme.io/3b649c6-Screenshot_2024-02-14_at_12.03.12.png" alt="Please update your selected rows binding to the new one"><figcaption><p>Please update your selected rows binding to the new one</p></figcaption></figure>

### Advanced settings

If you need more out of your table such as footers, resizeable and searchable columns, and conditional cell UI, you can check out the community Super Table.

Read more about Custom plugins.
