# MongoDB

MongoDB is a modern NoSQL database, that can be connected to Mware Application Development Framework with a few clicks.

Before we get started, make sure you've got the following:

* `ConnectionString` for connecting to the database remotely including username and password
* The Database name

In addition, ensure you have added the IP to the Mongo Atlas IP Access List.\
Check Whitelisting for the Mware Application Development Framework IP if you're using the cloud-hosted solution.

Now that you have everything you need to hook up your MongoDB installation to Mware Application Development Framework , let's get started.

> ### 📘ConnectionString
>
> Don't know how to get the ConnectionString? Check this useful guide by MongoDB.

> ### 👍Don't have a MongoDB instance?
>
> The easiest way to get started is by signing up for the (from) free offering from Mongo directly: MongoDB Atlas. We've also used MongoDB Atlas for writing this guide, and testing our integrations!

### Connect your MongoDB

To connect your MongoDB Data Source, head over to the data section in the builder UI. Then, click the `Add source` button in the left-hand settings panel to add a new data source.

<figure><img src="https://files.readme.io/7a77d59-Screenshot_2023-06-27_at_12.10.36.png" alt="Press the &#x60;+&#x60; icon to add a data source"><figcaption><p>Press the <code>+</code> icon to add a data source</p></figcaption></figure>

You will be presented a dialog, in which you choose `MongoDB`. Then you will be presented with the connection configuration.

<figure><img src="https://files.readme.io/7acce0f-mongo-connectionstring.jpeg" alt="673"><figcaption><p>A standard connectionstring when connecting to MongoDB Atlas</p></figcaption></figure>

Paste your ConnectionString, and enter the database name which you want to connect with, and click `Save and continue to query`.

Pressing this button will save the ConnectionString into your installation.

> ### 📘Connect to MongoDB Atlas
>
> Connecting to a hosted instance of MongoDB Atlas should be straightforward, but do check the useful guide on connecting to a database

### Adding a query

Now that you have added the connection to your database, you can start querying your data to use it straight away in your application.

The first step is to click the 'Add query' button, which is located on the data source page you just added. If you're not on this page, click on the MongoDB connection in the list of data sources on the left.

![](https://files.readme.io/29d5e4b-queries.png)

#### Query configuration

![](https://files.readme.io/d478411-query-configuration.png)

Configuring your query is essential in making sure it runs correctly. In the table below you'll find what each field means.

| Field        | Description                                                                                                                                             |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Query Name   | The name you want to give the query, this is a visual name. The name is used when selecting a Data Sources inside the design section                    |
| Function     | The action you want to perform. This is a MongoDB specified function. Choosing a function influences the options you get offered to you for actionTypes |
| Access Level | Which level do you want to be able to access this query?                                                                                                |
| Collection   | The MongoDB collection you want to query                                                                                                                |
| Action Types | The action-type defined by MongoDB, based on the Function you've selected.                                                                              |

> ### 👍What do the action types mean?
>
> The action types are defined by MongoDB. You can read more about those in the [MongoDB Documentation](https://docs.mongodb.com/v4.4/reference/method/js-collection/)

#### Create query

You have the option to insert one or many documents into a collection.

When inserting a single document, provide a payload within the query box.

<figure><img src="https://files.readme.io/cd3e4b3-Screenshot_2022-06-17_at_11.50.46.png" alt=""><figcaption></figcaption></figure>

In this case, an ObjectId is automatically generated for the new document.\
If you want to specify an ObjectId, you can do that as follows:

<figure><img src="https://files.readme.io/2a5b095-Screenshot_2022-06-17_at_11.54.46.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Document ID
>
> In MongoDB you can use most data types as a document ID, however the default and recommended type is [ObjectId](https://www.mongodb.com/docs/manual/reference/method/ObjectId/)

When inserting many, you must provide an array of objects.

<figure><img src="https://files.readme.io/1139ebd-Screenshot_2022-06-17_at_12.03.59.png" alt=""><figcaption></figcaption></figure>

#### Read query

The most basic query you can do is a _find_ with no query body.\
This will return all data available within the specified collection.

<figure><img src="https://files.readme.io/b80361e-Screenshot_2022-06-17_at_11.44.02.png" alt=""><figcaption></figcaption></figure>

A filter object can be added to the query box. For example, get all documents in the collection where age is greater than 40:

<figure><img src="https://files.readme.io/18cecf7-Screenshot_2022-06-17_at_11.43.33.png" alt=""><figcaption></figcaption></figure>

A full list of query operators can be found here: [https://www.mongodb.com/docs/manual/reference/operator/query/](https://www.mongodb.com/docs/manual/reference/operator/query/)

#### Update query

When performing an update you must provide two objects:

1. **\[Required]** Filter - find the documents to change.
2. **\[Required]** Update - instructions for the modifications to make.
3. **\[Optional]** Options - additional update parameters (more [here](https://www.mongodb.com/docs/v4.4/reference/method/db.collection.updateOne/))

For example, set the _retired_ property to true for all documents that have an age over 65:

<figure><img src="https://files.readme.io/f5d94e4-Screenshot_2022-06-17_at_12.23.37.png" alt=""><figcaption></figcaption></figure>

If _updateOne_ was used instead, then only the first filter match would have been updated.

A full list of update operators can be found here: [https://www.mongodb.com/docs/manual/reference/operator/update/](https://www.mongodb.com/docs/manual/reference/operator/update/)

#### Delete query

To delete ALL records from a collection, run a query with no filter and the _deleteMany_ action type set.\
**Careful!**

Otherwise, you can add a filter to delete all matching records:

<figure><img src="https://files.readme.io/b172b92-Screenshot_2022-06-17_at_17.13.47.png" alt=""><figcaption></figcaption></figure>

If _deleteOne_ was used instead, then only the first filter match would have been deleted.

In addition, a number of non-mandatory options are available: [https://www.mongodb.com/docs/v4.4/reference/method/db.collection.deleteOne/](https://www.mongodb.com/docs/v4.4/reference/method/db.collection.deleteOne/)

> ### 🚧Stringify the queries
>
> If you take a look at the MongoDB documentation, and you find queries there, they're most likely not stringified. As the `Fields` box only supports JSON objects, you should convert it to strings.

#### Bindings

Bindings in Queries allow you to insert data when using the query. You can use Handlebar [Bindings](https://docs.budibase.com/docs/bindings) inside the query. A simple, but common, example of a binding would be an ObjectID. This allows you to query a single ID based on a binding you've specified, even from the UI of your application.

Let's take a look at an example where we pass the ID from a binding into the query. Our binding looks like this.

<figure><img src="https://files.readme.io/f13b018-Screenshot_2022-06-17_at_12.51.35.png" alt=""><figcaption></figcaption></figure>

Then, adjusting our query, we add the `id` using handlebars directly into the string

<figure><img src="https://files.readme.io/2830edf-Screenshot_2022-06-17_at_12.47.08.png" alt=""><figcaption></figcaption></figure>

Now, wherever you use the created query as a data source in a [Data Provider](https://docs.budibase.com/docs/data-provider), you can configure the bindings directly from there, using the gearwheel behind the provider selection

<figure><img src="https://files.readme.io/c136975-configure-binding.png" alt=""><figcaption></figcaption></figure>

This will then open up the configuration modal, where you can bind the value, for example using a [Form](https://docs.budibase.com/docs/forms) input.

<figure><img src="https://files.readme.io/d6e9ca3-mongodb-query-binding.png" alt=""><figcaption></figcaption></figure>

#### Transformer

The transformer is intended for you to have a layer between the data received from your MongoDB database, and how it ends up in your interface. By default you `return` the data directly received, but manipulating, or restructuring, data in the transformer can be very useful to be able to limit what is returned, or to make it more eye-friendly.

The `transformer` box accepts JavaScript, and is ran within a function. This means you have to return the data you want to get back from the query.

> ### 👍Transformer
>
> To understand how Transformers work, read about them on the Transformers page

### Saving the query

Now that you have configured the entire query correctly, it's time to save the query. You can't save the query without running it first so we know it actually is working.

So the first thing you want to do now, is click the `Run Query` button

<figure><img src="https://files.readme.io/b2b5724-mongo-run-query.png" alt=""><figcaption></figcaption></figure>

As soon as you click the button, the query will be executed. When the query is successful, the result will be displayed below. There you can inspect the result, configure the schema and preview it in a table.

<figure><img src="https://files.readme.io/70ff61e-mongo-runquery-result.png" alt=""><figcaption></figcaption></figure>

#### Configuring schema

Configuring the schema is important for Mware Application Development Framework , as it allows the interface to understand what kind of data it is dealing with. In order for you to configure the data, click the `Schema` Tab, which is next to the `JSON` tab in the results section. You should then be presented with a page like this:

<figure><img src="https://files.readme.io/2edbd43-mongo-results-schema.png" alt=""><figcaption></figcaption></figure>

### Aggregations

MongoDB aggregations allow multiple documents to be grouped and processed.

This is done through _pipelines_ which contain a number of _stages_. More from the official docs here: MongoDB aggregations.

To use aggregations within Mware Application Development Framework , select the **Aggregate** function.

<figure><img src="https://files.readme.io/db42f82-Screenshot_2022-11-11_at_10.34.04.png" alt=""><figcaption></figcaption></figure>

There are two modes in which you can interact with aggregations: JSON and Pipeline.

#### Json

This is the default mode. Simply enter an array of stages to be processed.

<figure><img src="https://files.readme.io/95a2d7d-Screenshot_2022-11-11_at_10.42.11.png" alt=""><figcaption></figcaption></figure>

#### Pipeline

Whilst the JSON mode can do everything you need, you may find the GUI mode makes your pipeline easier to manage.

Add as many stages as you like with the `+` button, delete stages with the trashcan icon, and use the arrows to swap stages.

<figure><img src="https://files.readme.io/0efef57-Screenshot_2022-11-11_at_10.47.35.png" alt=""><figcaption></figcaption></figure>

A benefit of using the Pipeline mode is that it provides the available aggregation stages in a dropdown, and selecting a stage will fill out a template of relevant properties, which can be easily altered.

***

\


### Video tutorial

***

\


### Displaying related nested data

In MongoDB it is common to use nested documents instead of relationships. For example, this **transactions** collection returns an array of transaction meta data, with nested transaction data:

<figure><img src="https://files.readme.io/967a23d-nested_schema.png" alt=""><figcaption></figcaption></figure>

Looking at the `Schema` tab, we can see a property called _transactions_ of type _Array_:

<figure><img src="https://files.readme.io/0c6e00d-array_type.png" alt="Array of transactions"><figcaption><p>Array of transactions</p></figcaption></figure>

When displaying nested array query data, Mware Application Development Framework will automatically generate the schema for the nested objects.

To make use of the nested data, first add a Repeater Block to your screen for the query.

<figure><img src="https://files.readme.io/ec47d35-Screenshot_2024-02-19_at_11.55.12.png" alt="Selecting the query"><figcaption><p>Selecting the query</p></figcaption></figure>

Next nest a Table block under the repeater block. The nested data source can be selected under the _JSON Arrays_ section:

![](https://files.readme.io/5042e35-Screenshot\_2024-02-19\_at\_11.57.08.png)

Alternatively you can nest another **Repeater block**, which would give you access to the nested data row bindings:

![](https://files.readme.io/4931ab3-repeater.png)

The data types are determined automatically based on the primitive types provided by the Mongo query:

<figure><img src="https://files.readme.io/d6b85ac-mongo.png" alt="Example nested transaction data in Mongo Atlas"><figcaption><p>Example nested transaction data in Mongo Atlas</p></figcaption></figure>

\


#### Tutorial: Displaying array items as tags

In the previous example we had a nested array of objects. Sometimes you will have a simple array of strings instead, which can be used nicely with the [Tag](https://docs.budibase.com/docs/tag) component.

In this tutorial you will see how to display a list of product tags underneath each account. Here is the result of the Mongo query:

<figure><img src="https://files.readme.io/9ba62f1-Screenshot_2024-02-23_at_10.27.08.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/376a01a-Screenshot_2024-02-23_at_10.29.48.png" alt="Account schema with products array"><figcaption><p>Account schema with products array</p></figcaption></figure>

To begin create your screen and add a **Repeater block**. Select the _Read accounts_ query.

<figure><img src="https://files.readme.io/4379932-Screenshot_2024-02-23_at_10.30.44.png" alt=""><figcaption></figcaption></figure>

Next add a Headline and display the _account\_id_ with the following Binding: `Account ID: {{ Accounts Repeater Block.Read accounts.account_id }}`

Now we'll nest a second **Repeater block**, this time for the associated account products. Under the _Fields_ section of the _Data_ setting, select the _products_ field:

<figure><img src="https://files.readme.io/f7331e9-Screenshot_2024-02-23_at_10.37.48.png" alt="Selecting the related account products"><figcaption><p>Selecting the related account products</p></figcaption></figure>

Under the second repeater block, nest a **Tag** component. Your final Component Tree should look roughly like so:

<figure><img src="https://files.readme.io/16bc62a-Screenshot_2024-02-23_at_10.39.11.png" alt="Final component tree"><figcaption><p>Final component tree</p></figcaption></figure>

In the Settings Panel, set the _Text_ property of the tag to the products value: `{{ Products Repeater Block.products.value }}`

<figure><img src="https://files.readme.io/b736afa-Screenshot_2024-02-23_at_10.41.04.png" alt="Select the &#x27;Value&#x27; binding from the products repeater"><figcaption><p>Select the 'Value' binding from the products repeater</p></figcaption></figure>

Use Conditional UI to give the tags a custom colour based on the product:

<figure><img src="https://files.readme.io/c6199ef-Screenshot_2024-02-23_at_10.42.59.png" alt=""><figcaption></figcaption></figure>

**Result**

<figure><img src="https://files.readme.io/ecdb242-Screenshot_2024-02-23_at_10.44.26.png" alt=""><figcaption></figcaption></figure>
