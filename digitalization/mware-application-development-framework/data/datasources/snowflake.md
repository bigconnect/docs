# Snowflake

Snowflake is a solution for data warehousing, data lakes, data engineering, data science, data application development, and securely sharing and consuming shared data.

Whilst you can add primary keys and use Snowflake for OLTP, by default tables do not have primary keys.\
Particularly within Mware Application Development Framework, the Snowflake connector is designed for Custom queries, thus is most appropriate for OLAP purposes.\
[https://www.snowflake.com/guides/olap-vs-oltp](https://www.snowflake.com/guides/olap-vs-oltp)

### Setup

Snowflake has the following hierarchy for data:

* Warehouse
* Database
* Schema
* Table

To use the Snowflake connector in Mware Application Development Framework, you will need to have each of these elements.\
Upon creation of a new Snowflake account, there will be sample data that you can use to verify your connection.

<figure><img src="https://files.readme.io/3a7c341-Screenshot_2022-07-04_at_10.47.24.png" alt=""><figcaption></figcaption></figure>

Warehouses can be found and added under the _Admin_ tab. The default warehouse is _COMPUTE\_WH_.

You will also need your **account name**. This can be found under _Admin > Accounts_.

Click on the clip icon beside the account. The account name will be your snowflakecomputing subdomain, in this case: `hffwdhe-bn85610`

<figure><img src="https://files.readme.io/e5f601c-Screenshot_2022-07-04_at_11.18.43.png" alt=""><figcaption></figcaption></figure>

### Connect

When you add a new Snowflake Data Source you will be presented with a config dialog to fill in.

<figure><img src="https://files.readme.io/d2e759d-Screenshot_2022-07-04_at_12.57.13.png" alt=""><figcaption></figcaption></figure>

### Query

Snowflake can perform the standard SQL queries as you would expect, however it also has some nifty syntax to query JSON as well.

The sample _WEATHER_ schema contains a JSON object with a nested array of weather information.\
This can be seen when running a query via the [SnowSQL CLI](https://docs.snowflake.com/en/user-guide/snowsql.html)

<figure><img src="https://files.readme.io/a2be443-Screenshot_2022-07-04_at_14.13.55.png" alt=""><figcaption></figcaption></figure>

This structure can be flattened as needed, and then queried using the `:` notation to grab nested properties.\
[https://docs.snowflake.com/en/user-guide/querying-semistructured.html](https://docs.snowflake.com/en/user-guide/querying-semistructured.html)

<figure><img src="https://files.readme.io/d15f8a5-Screenshot_2022-07-04_at_14.28.41.png" alt=""><figcaption></figcaption></figure>

You can also see how Bindings can be used, in this case as a filter.

It would be possible to Transform the data rather than do the mapping as part of the query, however this is not advisable as the performance will be much better on the cloud servers.

**Preview**

<figure><img src="https://files.readme.io/21489f6-Screenshot_2022-07-04_at_14.32.49.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Limit
>
> A data warehouse query could potentially return huge amounts of data that could strain the UI.\
> For this reason, if you're not targeting specific data with a filter, use a limit in your queries, e.g. SELECT \* FROM MyTable LIMIT 1000;
