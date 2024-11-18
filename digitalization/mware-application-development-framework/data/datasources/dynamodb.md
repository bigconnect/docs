# DynamoDB

DynamoDB is a fast and flexible NoSQL database service offered by Amazon Web Services (AWS).

### Setup

To connect to DynamoDB you will need to know the **region** that your database is located.

In addition, you will need an **access key** and **secret key**. Both of these can be obtained by following this [guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SettingUp.DynamoWebService.html).

### Connect

Add a new datasource, and select DynamoDB.

<figure><img src="https://files.readme.io/849b8bf-Screenshot_2022-08-16_at_18.04.34.png" alt=""><figcaption></figcaption></figure>

You will be prompted to enter in the necessary credentials.

It is important to note that if you are hosting on _amazonaws.com_ then your **region** field should match the region used in the **endpoint** field.

<figure><img src="https://files.readme.io/87ecfb2-Screenshot_2022-08-16_at_16.19.18.png" alt=""><figcaption></figcaption></figure>

### Query types

The DynamoDB connector has a variety of query types beyond the typical CRUD options.

| Name         | Description                                                                                                                                                                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create (Put) | Creates a new item, or replaces an old item with a new item                                                                                                                                                                                                    |
| Scan         | Returns one or more items and item attributes by accessing every item in a table or a secondary index (limit of 1 MB of data).                                                                                                                                 |
| Describe     | Returns information about the table, including the current status of the table, when it was created, the primary key schema, and any indexes on the table. [More info](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API\_DescribeTable.html) |
| Get          | Returns a single item given the primary key of that item                                                                                                                                                                                                       |
| Update       | Edits an existing item's attributes, or adds a new item to the table if it does not already exist                                                                                                                                                              |
| Delete       | Deletes a single item in a table by primary key                                                                                                                                                                                                                |
| Read (Query) | Returns one or more items and item attributes by accessing every item in a table or a secondary index (maximum of 1 MB of data).                                                                                                                               |

More information on the client library used can be found [here](https://github.com/dabit3/dynamodb-documentclient-cheat-sheet).

DynamoDB query operations are further detailed [here](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html).

### Create

To create an item, select the _Create_ function, enter a table name, and provide an _Item_ object with the appropriate table fields.

<figure><img src="https://files.readme.io/9bbe739-Screenshot_2022-08-16_at_17.15.04.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/7e15f7c-Screenshot_2022-08-16_at_17.17.06.png" alt=""><figcaption></figcaption></figure>

If the primary key in your query matches an existing record, then Create will update that record. For example assuming the entry for 'Carol' has just been created, the following would perform an update:

JSON

```json
{
	"Item": {
		"id": "3",
		"Name": "Donna"
	}
}
```

<figure><img src="https://files.readme.io/2061cf2-Screenshot_2022-08-16_at_18.01.15.png" alt=""><figcaption></figcaption></figure>

Furthermore, any additional fields that are included within the _Item_ payload will be added to your documents.

### Scan

If you exclude the payload, then the scan query will simply return all rows of the specified table.

You can however provide a filter expression in the query box such as:

JSON

```json
{
  "FilterExpression": "#studentName = :studentName",
  "ExpressionAttributeValues": { ":studentName": "Adam" },
  "ExpressionAttributeNames": { "#studentName": "Name" }
}
```

<figure><img src="https://files.readme.io/0ded3d5-Screenshot_2022-08-16_at_17.54.26.png" alt=""><figcaption></figcaption></figure>

### Get

This function will return a single item based on a primary key specified as follows:

JSON

```json
{
	"Key": {
		"id": "2"
	}
}
```

To return the item data, a minor change will be required in the [Transformer](https://docs.budibase.com/docs/transformers)

<figure><img src="https://files.readme.io/109a215-Screenshot_2022-08-16_at_18.11.24.png" alt=""><figcaption></figcaption></figure>

### Update

Similar to the _Get_ function, _Update_ requires a key and thus only updates one item at a time.

To perform an update, an update expression must also be provided. For example:

<figure><img src="https://files.readme.io/ce3d071-Screenshot_2022-08-16_at_18.27.15.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/7b15c2e-Screenshot_2022-08-16_at_18.27.42.png" alt=""><figcaption></figcaption></figure>

As you can see, you can also include bindings within your expressions.

### Delete

To delete a single item, select the _Delete_ function, and provide the primary key of the document within your query payload.

JSON

```json
{
	"Key": {
		"id": "1"
	}
}
```

<figure><img src="https://files.readme.io/00583eb-Screenshot_2022-08-16_at_18.31.06.png" alt=""><figcaption></figcaption></figure>
