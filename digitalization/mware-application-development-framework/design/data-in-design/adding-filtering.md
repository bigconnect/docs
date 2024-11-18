# Adding filtering

Below are the different methods of filtering within Budibase.

### Dynamic filtering

If you have a Table component connected to a Data provider component, you can add dynamic filtering by simply adding the Dynamic filter component. This should work as soon as you add it.

<figure><img src="https://files.readme.io/d70ff30f59c11009b6a3de339f2f03590690a6347015d9679f151d0d2aa4bc61-Screenshot_2024-10-28_at_15.09.44.png" alt=""><figcaption></figcaption></figure>

### Static filtering

Once you have linked a Datasource to a component, you can access the filter drawer by clicking the button beside the _Filtering_ setting in the Settings Panel.

<figure><img src="https://files.readme.io/ac1dd7d-Screenshot_2023-09-07_at_11.40.50.png" alt="Adding filters to a Data provider"><figcaption><p>Adding filters to a Data provider</p></figcaption></figure>

The filter drawer is where you select the columns you want to filter on. For example, you may want to return sales records only where any of the sales reps made more than 60 sales, or all sales made in the first week of the month.

<figure><img src="https://files.readme.io/fef799165a2b90ffb11b9609f86ad8fde74d93d05da48a7388044edc81534a4e-Screenshot_2024-10-28_at_15.13.53.png" alt=""><figcaption></figcaption></figure>

It is also possible to filter on multiple columns, and even filter on the same column multiple times.

### Filtering data from the front-end

Often you will find yourself wanting to let the user filter the data on the front-end. This can be achieved as well but requires a bit more work.To get started, add a Data Provider and set the data source. In this example we want to let the user filter to narrow down by day\_of\_month.

Next, add a Form Component, and add two Number fields. set the first to be day\_start, and label it "Day of month (start)". Take similar steps i for the other Text Field, naming it day\_end and labelling it "Day of month (end)"

In the data provider, select the define filters button. Next, add an expression and select the column you wish to filter on - in this case `day_of_month`, `More than or equal to` and `Binding`. Select the lightning bolt icon to the right and select the number field choose "start\_day". Repeat the process for the the upper-limit of the filter, this time using "less than or equal to". Now your user can specify a start and end day and view sales made in that particular timeframe.

<figure><img src="https://files.readme.io/34b558aae741bea5c8c63fe3ff94aad67baa3fea32c44466f34eeea649e3db65-Screenshot_2024-10-28_at_15.19.47.png" alt=""><figcaption></figcaption></figure>

### Filtering options & Filter Groups

When filtering, there are a couple of settings that can be tweaked that determine how the filters are handled.

<figure><img src="https://files.readme.io/f98f3f42b0fabfc4f17b28a10814b96ad789836d993d882b18ef4987ca88e0f7-Screen_Recording_2024-10-28_at_15.49.08.gif" alt="Notice the change in the second filter group from And to Or"><figcaption><p>Notice the change in the second filter group from And to Or</p></figcaption></figure>

**Behaviour when combining Filter Groups**

* **Show data which matches all filter groups**: all filters must match when returning records
* **Show data which matches any filter** : records are returned when they match on any of the provided filter groups

**Behaviour in Filter Groups themselves**

* **Show data when any of the following filters are matches**: Only one of the filters needs to match in order to return this record
* **Show data when all of the following filters are matches** : All of the filters in this group need to match in order to return this record

\


**When filter empty**

This setting is specific to filters that are using Bindings. It determines what data should be returned if the binding values are blank.

By default, if all of the filter binding values are empty, then all the rows will be returned. If _Return no rows_ is selected and the binding values are empty, then no rows will be returned. This can be particularly useful when pairing a table with search fields, as an empty search field will result in all rows being shown, but as a user types a search-term it will narrow the list down.

<figure><img src="https://files.readme.io/78acfe3680e2c9a5e30e83f4edac80d310280cd5b647520cb61b00bb76fce7ad-Screenshot_2024-11-01_at_15.41.11.png" alt=""><figcaption></figcaption></figure>

As shown, the filters are bound to two form fields which are currently empty, therefore _all table rows_ are returned.

<figure><img src="https://files.readme.io/298de251b99ae44e9f4b8015c42d06e2d00dfd9c0bc9b4a931b8f99b0c42f7db-Screenshot_2024-11-01_at_15.43.20.png" alt=""><figcaption></figcaption></figure>

In this screenshot we can see that the _Return no rows_ option is selected, which prevents rows being displayed as the bound form values are empty. Below we can see that now rows are returned when filters are empty, rows matching the search term are found and returned, and then no rows are returned when nothing matches the filter.

<figure><img src="https://files.readme.io/c1bd1c4408cc71fee5b90ac995c6a57e9e8eb2af31424dd7e3edd9d52f8c0f16-Screen_Recording_2024-11-01_at_16.11.53.gif" alt=""><figcaption></figcaption></figure>

You can also filter using a relationship picker.
