# Data provider

The data provider component is one of the most important components in Mware Application Development Framework. It exposes data to any components that expect data input on that particular screen.

For example, all components under the _Data_ section can be provided data.

<figure><img src="https://files.readme.io/af2aa63-Screenshot_2022-10-07_at_11.16.05.png" alt="Add component: Data section"><figcaption><p>Add component: Data section</p></figcaption></figure>

In addition, the following components can make use of data providers:

* Options pickers
* Blocks
* Charts

<figure><img src="https://files.readme.io/be54d51-data_provider_nested.png" alt="All of these nested components can access the New Data Provider"><figcaption><p>All of these nested components can access the New Data Provider</p></figcaption></figure>

<figure><img src="https://files.readme.io/381cfaf-Screenshot_2022-10-07_at_12.01.14.png" alt="Selecting a Data Provider"><figcaption><p>Selecting a Data Provider</p></figcaption></figure>

\


### Making use of your datasources

A data provider can make all Datasources available for consumption within your apps. This includes, but is not limited to:

* The internal DB tables
* External SQL Datasources
* Custom queries
* REST API

Once you have added your data provider you need to select which datasource you want to use. Simply click on the _Data_ dropdown.

<figure><img src="https://files.readme.io/cf5976e2b6ced99e339048791acd4f2a65cb929a34ead243fde1c7241bdc30ad-Screenshot_2024-10-23_at_15.17.22.png" alt="Providing data from the ServiceLog table"><figcaption><p>Providing data from the Users table</p></figcaption></figure>

> ### 🚧Nested data provider
>
> Whilst you can nest data providers, you may find there to be some performance issues.
>
> It is worth considering if you could display the additional data in another Screen or Modal.

\


In addition to controlling which datasource that should be available to your child components, data providers also allow you to sort, limit, paginate and filter the data that will ultimately be presented in your app screen.



### Pagination and sorting

To determine the sorting of the data rows provided, select a _Sort Column_ and _Sort Order_.

<figure><img src="https://files.readme.io/4b30f599334bc7f971e2e8e764c1a11b8709461946b544415949bc2605f1de38-Screenshot_2024-10-23_at_15.28.03.png" alt=""><figcaption></figcaption></figure>

In this instance, the [Table](https://docs.budibase.com/docs/table) component can override the default sorting set by the data provider, however with other components the sorting will be static.

<figure><img src="https://files.readme.io/5dd8b90529e27ac3cf464e488d493f612654b6d7bc4d50a6142c6b896e9c4e6f-Screenshot_2024-10-23_at_15.28.48.png" alt=""><figcaption></figcaption></figure>

You can also see that a **limit** of 5 has been set. This sets the maximum number of rows that can be presented per page.

Limits, sorting and pagination are done through server side queries where possible for performance, e.g. if the datasource was PostgreSQL, or the Internal DB. It is worth noting however, that Custom queries and REST queries data provider settings are handled client side. As a result, in these cases, sorting will apply on a per page basis only.

The type of sorting will be determined by the data type of the chosen column. For example text columns will be sorted alphabetically, whereas number columns will be sorted numerically.

With pagination checked, this can be used by not only tables, but all components that are linked to that data provider. For example, a paginated data provider for a chart component will cause the chart to re-render with the new data as each page is cycled through.

### &#x20;Filtering

To add filters to a data provider, click **Define filters**.

<figure><img src="https://files.readme.io/3c1c350736c0243469bec5996c110d633b23b16ffd27fe3d62345afe34737d8a-Screenshot_2024-10-23_at_15.39.08.png" alt=""><figcaption></figcaption></figure>

This will bring up a drawer from which you can select _match all_ or _match any_ mode.

You can then select the column you wish to filter on, alongside the operator. Click **Add filter** to filter on additional columns.

<figure><img src="https://files.readme.io/3709bba6df6956bb7a48c1f3c9acae7f6fd4afefe50a44781aa6f1c4c5990a63-Screenshot_2024-10-23_at_15.47.50.png" alt=""><figcaption></figcaption></figure>

Click here for more on filtering. In this example, we are using a form component with a single text field in it, as seen in the builder screenshots above.

Similar to sorting, filtering will be performed server side for SQL datasource and the internal DB, however for custom queries the filtering will be client side. Thus when using custom queries, it is recommended that you provide any filters within the query.

\


### When to use a repeater

If you want to build something a bit more custom than a table; maybe you want a container with some buttons that trigger an action or automation, then you will need the [Repeater](https://docs.budibase.com/docs/repeater) component. Here's an example of a list of improvements suggested by staff that could help minimize risks and improve safety.

<figure><img src="https://files.readme.io/9787e977924a35030afbd62f797cbaa6d4913c92759c3e51daf636431b7cb844-Screenshot_2024-10-23_at_15.58.53.png" alt=""><figcaption></figcaption></figure>
