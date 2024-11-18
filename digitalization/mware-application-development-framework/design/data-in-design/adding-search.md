# Adding search

How to add and connect a search field to your data within Mware Application Development Framework.

Most internal apps involve viewing and interacting with data. Adding search to your data display component, such as a Table provides a better experience for the end-user. Using Mware Application Development Framework, there are three ways to achieve this:

1. Using the Table itself
2. Using a form, table, and text input - the flexible way
3. Using a table and dynamic filter - the filter/search hybrid approach (also very easy)

This guide will cover all three methods for adding search to a table. For each process, you will need data. In the tutorials, we will use the following CSV. If you have data, please continue to the tutorials below. If you don't have data, please download (File > Download) the following CSV and upload it to the DB.

**Right, let's do this 🏁**

***

## 1. Using the table itself

Built into the [Table](https://docs.budibase.com/docs/table) is a search field for every column you choose to display.

_Please note, that the searchable text field will appear only if the data type of the field you choose is text/string. If the field is stored as an option, a picker component will appear instead._

<figure><img src="https://files.readme.io/f5917da0d45ff8556dcfaa171cab51d429048b1be80685665bcae51e8b165c5f-Screen_Recording_2024-10-24_at_16.55.21.gif" alt=""><figcaption></figcaption></figure>

***

## 2. Using a form and text input (flexible way)

Here's a quick overview of what we're going to do:

* Display data in a table
* Add a form and text field
* Connect the table's data to the search input

### Display data in a table

To get started, add a table component to your screen, and in the Settings Panel choose the data source to be the table you wish to display. to select your Datasource. You should now have a table with data.

<figure><img src="https://files.readme.io/86ac8e65d226a72f5f985d0fe5f86429787e155b6c3881f4c7cd27232f97afad-Screenshot_2024-10-25_at_13.29.30.png" alt=""><figcaption></figcaption></figure>

### Add a form and text field

Add a form component, and nest inside it a text field. Give the text field a name (this is how it will appear in bindings) and a label (this is what the user will see). You may also wish to add placeholder or help text.

#### Define filters

Now in the settings menu of the Table Component, click to add a filter

<figure><img src="https://files.readme.io/5bd0b069f3e763e5b676fc81237b0b897825bb5a20999d4b48a8b2da07388f83-image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/846ae6a8ce978c191c33acc2ca2eaa38a3215604f1675f5e52170f1ad95ee65f-Screenshot_2024-10-29_at_11.15.25.png" alt=""><figcaption></figcaption></figure>

With these settings, when no nothing in in the search bar, all rows will be displayed. When something is in the search bar, all rows where the customer name is _like_ (ie a partial or full match) will be returned. When there are no rows that match, the table will be empty. The filter will

<figure><img src="https://files.readme.io/a7e62bbdbf924b237b50109a25816aa2cea648d71705ee6592b083b3142d5463-Screen_Recording_2024-10-25_at_14.01.18.gif" alt=""><figcaption></figcaption></figure>

### &#x20;Adding a search button (optional)

After following these steps you will now have a reactive search field - i.e. whenever you press the enter key or click off the text field the search will trigger.

You may instead prefer to only trigger the search on click of a button. A guide on doing that can be found in this Github discussion.

***

## 3. Using a table and dynamic filter - the filter/search hybrid approach

This approach does not provide a search input. But it is easy to set up and allows your users to filter/search their table data. We can add dynamic filtering to our data in 3 simple steps:

1. Add a data provider and change the Data setting to match your data source - in our case _Sales_.
2. Add a dynamic filter component
3. Add a table component

### Get Started

1. Start by adding a Data Provider, and selecting the data source you wish to serve to the user.

<figure><img src="https://files.readme.io/1512448edb0a52a7f5db1525184d33a6cad4f2d6336bb545c8bba79045d6ea44-image.png" alt=""><figcaption></figcaption></figure>

2. Next, add a Table component, and set it's Data source to be the Data Provider you added int the step above.
3.  Next, add a Dynamic Filter component and set it's Data source to also be the same Data Provider. Your component tree should look similar to this:



    <figure><img src="https://files.readme.io/e3a2a088a7e026c390070ba44b354ea0f6c182bcb0978c343809f8bb6fddb0ca-image.png" alt=""><figcaption></figcaption></figure>

    <figure><img src="https://files.readme.io/560f00c872d7362b4b4b729001213dfb65f0ab093ea824f0618b137f3494520f-image.png" alt=""><figcaption></figcaption></figure>
4.  In the settings of the Dynamic Filter you can now give users the option to set filters based on allowed columns.



    <figure><img src="https://files.readme.io/c4a1291b59df944d5cbe77fe2d5f57c67376db4536918ce0531d1fd8e234cab6-image.png" alt=""><figcaption></figcaption></figure>

As we can see in the example below a user can set their own filters to help them find and display specific rows that they are looking for.

<figure><img src="https://files.readme.io/eee7ba4b38dbf5f38c045846150cc581f84c4e81080859e4d1c2477a547745d8-Screen_Recording_2024-10-25_at_15.28.09.gif" alt=""><figcaption></figcaption></figure>

***

> ### 👍Ask the community about search
>
> If you're running into any issues with adding search functionality to your data, open a discussion within our community forum.
