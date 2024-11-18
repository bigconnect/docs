# Relationship picker

Whilst also a dropdown list, the relationship picker enforces select one or many options depending on whether the chosen relationship field is defined as one or many. Furthermore, the available options will automatically be populated from the rows of the related table.

The label column (the option text the user sees) will match the display column of the related table.\
The value column (the option value that will be saved) will be the **\_id** of the selected relationship row(s).

### \_id

The **\_id** field represents a unique row identifier, which will exist on all data tables, including the internal DB and SQL tables within Mware Application Development Framework. It is a URL-safe encoded string.

For testing purposes, you can copy the row **\_id** by right-clicking on a row in the **Data** section, and clicking `Copy row _id`. In the vast majority of cases you will not need to manually access the **\_id** field.

<figure><img src="https://files.readme.io/31a42fd-Screenshot_2023-08-11_at_09.40.03.png" alt="Copy row \_id"><figcaption><p>Copy row _id</p></figcaption></figure>

As Relationship links are stored in this format, the **\_id** field must be used when filtering a table on a chosen relationship picker value.

<figure><img src="https://files.readme.io/45244d6-Screenshot_2023-08-11_at_10.23.49.png" alt="Displaying the underlying values of the chosen relationship links"><figcaption><p>Displaying the underlying values of the chosen relationship links</p></figcaption></figure>

> ### 📘One or Many relationship value
>
> Regardless of whether the relationship picker is a multi-select or a single-option, the relationship picker will always return an array of rows IDs.

#### Default value

As mentioned above, relationship pickers expose encoded row identifiers. Therefore, to provide a Default Value for a relationship picker, we must provide an array of row identifiers.

While you can manually enter an array of hard-coded row identifiers, typically you'll want to base the default value on the known display name, as this is easier to maintain and read.

1. Above your relationship picker, add a Data provider
2. Select the table that matches the source of the relationship field, and untick _Paginate_

<figure><img src="https://files.readme.io/6b32cfc-Screenshot_2023-08-11_at_10.42.09.png" alt="Adding a Data provider to be used by the &#x27;Cities&#x27; relationship picker"><figcaption><p>Adding a Data provider to be used by the 'Cities' relationship picker</p></figcaption></figure>

3. Click the button beside _Filtering_, and add an **Is in** filter as follows:

<figure><img src="https://files.readme.io/69f961b-Screenshot_2023-08-11_at_10.44.33.png" alt="Fetch some default (comma separated) cities"><figcaption><p>Fetch some default (comma separated) cities</p></figcaption></figure>

4. Make sure the relationship picker is nested under the data provider
5. Click the lightning bolt icon beside the _Default value_ field of the relationship picker, and add the following JavaScript code:

JavaScript

```javascript
return $("City Data Provider.Rows")?.map(row => row._id);
```

6. Refresh the page to see the default values load in

<figure><img src="https://files.readme.io/71294c3-Screenshot_2023-08-11_at_10.47.49.png" alt="Relationship picker with default values"><figcaption><p>Relationship picker with default values</p></figcaption></figure>

\


### Filter the list of options

By default, the relationship picker options will be populated by all rows in the related table. This may be appropriate for small numbers of relationships, but for larger lists you will want to filter this down. Furthermore, there is a **hard limit of 100 options**, so if you have more than this you will need to apply a filter.

You can add filters in the Settings Panel by clicking on the _Filtering_ button.

For example, only show related cities of the selected country:

<figure><img src="https://files.readme.io/a384468-4d873bd-Screenshot_2023-08-14_at_11.51.17.png" alt=""><figcaption></figcaption></figure>

> ### 👍
>
> For the full tutorial, check out the Cascading dropdown filters recipe!

### On change

You may want to perform an action when changing the relationship picker value. If you want to do something with the underlying related row data, then you will need to use the **Fetch Row** action.

1. Click the button beside _On change_ in the Settings Panel
2. `Add action`: **Fetch Row** and select the table that matches the relationship table
3. Set the _Row ID_ to `{{ Field Value }}`

<figure><img src="https://files.readme.io/77a8708-Screenshot_2023-08-14_at_13.32.59.png" alt="Fetching the selected row data"><figcaption><p>Fetching the selected row data</p></figcaption></figure>

4. `Add action`: **Show Notification** and use Action chaining to retrieve the value from the **Fetch Row** action, using the Binding `{{ Action 1.Fetched row }}` to retrieve the whole row, or reference specific columns such as the country name.

<figure><img src="https://files.readme.io/cd4adf7-Screenshot_2023-08-14_at_13.35.13.png" alt=""><figcaption></figcaption></figure>

**Result**

<figure><img src="https://files.readme.io/5b28ad6-Screenshot_2023-08-14_at_13.36.52.png" alt=""><figcaption></figcaption></figure>

There are many other Actions that you can make use of.

***

\


### Tutorial: Filter on selected relationship picker value(s)

When using an external **SQL** datasource, such as MySQL, PostgreSQL and SQL Server, it is possible to use the **Relationship Picker** to filter related data.

The Mware Application Development Framework DB could be used in this context, however it is a NoSQL database and generally not ideal for extensive relationship functionality.

For this tutorial we will use PostgreSQL, and assume there are three tables with the following relationships:

* One Owner -> Many Pets
* One Pet -> Many Appointments

For a given owner, we want to display a list of appointments based on the chosen pet.

Follow this guide to see how to connect to your Postgres database, fetch the tables and define the relationships.

<figure><img src="https://files.readme.io/d4ca78e-Screenshot_2022-08-17_at_14.24.20.png" alt=""><figcaption></figcaption></figure>

#### 1. Setup the owner form

Set the _Pet_ and _Owner_ name column as the display column and create the autogenerated screen for the _Owner_ table.

Next Eject the Table block. Select the 'Details' Form block, uncheck all fields except for the _Pets_ relationship field, and also uncheck the _Show save button_ and _Show delete button_ settings. Finally eject the details form block, and once ejected rename the form to 'Details Form' and set the form type to **Create**.

<figure><img src="https://files.readme.io/54a880b-Screenshot_2023-03-08_at_15.42.34.png" alt="Ejected &#x27;Details&#x27; form block"><figcaption><p>Ejected 'Details' form block</p></figcaption></figure>

#### 2. Add the appointments

Within the details form, add a Headline for 'Appointments' and add a Divider below.

Next add a Repeater block for the Pets relationship.

![](https://files.readme.io/7a437f5-Screenshot\_2022-08-17\_at\_15.47.23.png)

_Define filters_ for the repeater block using the **Is in** filter as follows:

<figure><img src="https://files.readme.io/479fe92-Screenshot_2023-03-13_at_10.38.49.png" alt=""><figcaption></figcaption></figure>

Finally, nest a Cards block within the repeater block. Set the datasource to the appointments relationship.

![](https://files.readme.io/1403c33-Screenshot\_2022-08-17\_at\_15.55.33.png)

Set the title to the name of the pet: `{{ Pets Repeater block.Pets.Name }}`\
Set the cards title and subtitle to the date and reason of the appointment, e.g. `{{ Appointments Cards block.Appointments.Reason }}`

The date can be prettified as follows: `{{ date Appointments Cards block.Appointments.Date "dddd, MMMM Do, h:mm a" }}`

<figure><img src="https://files.readme.io/d501633-Screenshot_2022-12-12_at_16.16.16.png" alt="Details side panel final component tree"><figcaption><p>Details side panel final component tree</p></figcaption></figure>

**Result**

<figure><img src="https://files.readme.io/ba7a4e8-filter_with_rel.gif" alt=""><figcaption></figcaption></figure>
