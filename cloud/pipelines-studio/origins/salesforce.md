# Salesforce

The Salesforce origin reads data from Salesforce. You can configure the origin to read data in one or both of the following ways:

*   Execute a query to read existing data from Salesforce using the Bulk API or SOAP API.

    When processing existing data, you configure the SOQL query, offset field, and optional initial offset to use. When using the Bulk API, you can enable PK Chunking to efficiently process very large volumes of data.

    When processing existing data and not subscribed to notifications, you can configure the origin to repeat the SOQL query. The origin can perform a full or incremental read at specified intervals. And under certain circumstances, you can also process deleted records.
*   Subscribe to notifications to process PushTopic, platform, or change events.

    When subscribing to notifications to process events, you specify the event type and the name of the topic, API, or Change Data Capture object. When subscribing to change or platform events, you can also specify a replay property.

By default, the origin generates Salesforce record header attributes and Salesforce field attributes that provide additional information about each record and field. The origin also includes the CRUD operation type in a record header attribute so generated records can be easily processed by CRUD-enabled destinations.&#x20;

You can specify the prefix to use for Salesforce attributes or disable attribute generation altogether. You can also optionally disable query validation or use an HTTP proxy to connect to Salesforce.

When enabled in Salesforce, you can configure the origin to use mutual authentication to connect to Salesforce.

The origin can generate events for an event stream.

### Querying Data <a href="#ariaid-title2" id="ariaid-title2"></a>

The Salesforce origin can execute a query to read existing data from Salesforce. Use the Salesforce Object Query Language (SOQL) to write the query.

When you configure the origin to query existing data, you specify whether the origin uses the Salesforce Bulk API or SOAP API to read from Salesforce. The Bulk API is optimized to process large sets of data. When using the Bulk API, you can enable PK Chunking for larger data sets. The SOAP API supports more complex queries than the Bulk API. For example, to use aggregate functions, you must use the SOAP API. However, the SOAP API is less practical when processing large sets of data. For more information about when to use the Bulk or SOAP API, see the Salesforce Developer documentation.

The Salesforce origin uses an offset field and an initial offset or start ID to determine where to start reading data within an object. By default, the offset field is defined as the Salesforce Id system field, which contains a unique identifier for each record in a Salesforce object.

When you configure the origin to query existing data and do not subscribe to notifications, you can configure the origin to run the query once or to repeat the query. When running the query once, the pipeline stops when it finishes reading all data from the Salesforce object. If you start the pipeline again, the origin uses the initial offset or start ID to start reading, reading the entire set of existing data again.

If the pipeline stops before it finishes reading all data, the Salesforce origin saves the last read offset value. When the pipeline starts again, the origin uses the last read offset value to continue processing from where it stopped. You can reset the origin to process all requested objects.

When you configure the origin to run the query more than once, the pipeline runs continuously so it can repeat the query at regular intervals. You can choose how the origin repeats the query.&#x20;

In rare cases, the query returns data with a type that does not match the data type specified in the schema for a field. For example, the query might return a float when the schema specifies an integer. Use the advanced Mismatched Types Behavior property to configure how the origin handles such data type mismatches. The origin can retain the returned data, truncate the returned data to match the specified type, or round the returned data to match the specified type.

#### Using the SOAP and Bulk API <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use the SOAP or Bulk API to query existing Salesforce data. When querying existing data, you define the SOQL query and related properties to determine the data returned from Salesforce.

Use the following guidelines when using the SOAP API or the Bulk API without PK Chunking:SOQL queryWhen processing existing data with the SOAP API or the Bulk API, use the following query guidelines:

*   In the WHERE clause, include the offset field and the offset value.

    The origin uses an offset field and value to determine the data that is returned. Include both in the WHERE clause of the query.
*   In the WHERE clause, use the OFFSET constant to represent the offset value.Use ${OFFSET} to represent the offset value. For example, when you start a pipeline, the following query returns all data from the object where the data in the offset field is greater than the initial offset value:

    ```
    SELECT Id, Name FROM <object> WHERE <offset field> > ${OFFSET}
    ```

    Tip: When the offset values are strings, enclose ${OFFSET} in single quotation marks.
*   In the ORDER BY clause, include the offset field as the first field.

    To avoid returning duplicate data, use the offset field as the first field in the ORDER BY clause.

    Note: Using a field that is not the Id field in the ORDER BY clause can slow performance.

When processing existing data with the SOAP API, you can include SOQL aggregate functions in the SELECT statements of SOQL queries. The Bulk API does not support aggregate functions.

The complete SOQL query should use the following syntax:

```
SELECT <offset field>, <field1>, <field2>, ... FROM <object> WHERE <offset field> > ${OFFSET} ORDER BY <offset field>
```

If you specify `SELECT * FROM <object>` in the SOQL query, the origin expands `*` to all fields in the Salesforce object that are accessible to the configured user. Note that the origin adds components of compound fields to the query, rather than adding the compound fields themselves. For example, the origin adds BillingStreet, BillingCity, etc., rather than adding BillingAddress. Similarly, it adds Location\_\_Latitude\_\_s and Location\_\_Longitude\_\_s rather than Location\_\_c.

When necessary, you can configure the origin to skip validating the query. Skip query validation when you know that the query is valid but it does not match validation requirements. For example, you must disable query validation if you omit the ORDER BY clause. You might omit the ORDER BY clause to improve performance on a large query. To disable query validation, use the Disable Query Validation property on the Advanced tab.

Additional propertiesWhen processing existing data with the SOAP API or the Bulk API, configure the following additional properties on the Query tab:

* Offset Field - Typically the Id system field, the offset field should be an indexed field in the record. Default is the Id field.
* Initial Offset - First offset value to use when the pipeline starts or after you reset the origin.
*   Include Deleted Records - An optional property. Determines whether the SOQL query also retrieves deleted records from the Salesforce recycle bin.

    The query can retrieve deleted records when the stage uses the Salesforce SOAP API or the Bulk API version 39.0 or later. Earlier versions of the Bulk API do not support retrieving deleted records.

**Example**

Let's assume that you want to read all names and account numbers from the Salesforce Account object a single time. The object contains a fair number of records, so you choose to use the Salesforce Bulk API.

To process the data, you configure the following properties on the Query tab:

* Use Bulk API - Enable the use of the Bulk API.
*   SOQL Query - Include the offset field and offset value in the WHERE and ORDER BY clauses, as well as the fields to return, as follows:

    ```
    SELECT Id, Name, AccountNumber FROM Account WHERE Id > '${OFFSET}' ORDER BY Id
    ```
* Repeat Query - Set to No Repeat to run the query a single time.
* Initial Offset - Use the default value of fifteen zeros (`000000000000000`) for the offset value to ensure that the origin reads all records in the object.
* Offset Field - Use the default, `Id`, for the offset field.

**Aggregate Functions in SOQL Queries**

When using the SOAP API to query existing Salesforce data, you can include SOQL aggregate functions in the SELECT statements of SOQL queries. The origin places the result from the first function of a query into the `expr0` field, the result from the second function of the same query into the `expr1` field, and so on. The resulting field types depend on the functions and queried fields. The stage does not generate field header attributes for the fields resulting from aggregate functions. You can only include both aggregate functions and non-aggregated fields in the same SELECT statement when you group by the non-aggregated fields.

The following examples demonstrate some uses of aggregate functions in SOQL queries. Each example reads data from the Account object where the name begins with `East`.

**GROUP BY Clause**

You can combine aggregate functions with a GROUP BY clause to compute values for groups of records.

Suppose that for records beginning with `East`, you want a list of industries along with a count of records, the last modified date, and the minimum number of employees grouped by the `Industry` field.

You can enter the following query:

```
SELECT Industry, COUNT(Id), MAX(LastModifiedDate), MIN(NumberOfEmployees) FROM Account 
WHERE Id > '${OFFSET}' AND Name LIKE 'East%' 
GROUP BY Industry
```

The origin places the results from the query into the following fields:

* `Industry`
* `expr0` - Integer field contains the count of records
* `expr1` - Datetime field contains the last modified date
* `expr2` - Integer field contains the minimum number of employees

**Field Aliases**

You can use field aliases in a query to specify the field names where the origin places function results.

Suppose that in the previous example, you want to place the count of records into the `cnt` field, the last modified date into the `max_modify` field, and the minimum number of employees into the `min_employees` field.

You can enter the following query:

```
SELECT Industry, COUNT(Id) cnt, MAX(LastModifiedDate) max_modify, MIN(NumberOfEmployees) min_employees FROM Account
WHERE Id > '${OFFSET}' AND Name LIKE 'East%' 
GROUP BY Industry
```

The origin places the results from the query into the following fields:

* `Industry`
* `cnt`
* `max_modify`
* `min_employees`

You cannot specify a SOQL keyword, such as `count`, as an alias.

#### Using the Bulk API with PK Chunking <a href="#ariaid-title6" id="ariaid-title6"></a>

You can use PK Chunking with the Bulk API to process large volumes of Salesforce data. PK Chunking uses the Id field as the offset field and returns chunks of data based on user-defined chunks of the Id field. For more information about PK Chunking, see the [Salesforce documentation](https://developer.salesforce.com/docs/atlas.en-us.api\_asynch.meta/api\_asynch/async\_api\_headers\_enable\_pk\_chunking.htm?search\_text=chunking) or this informative [blog post](https://developer.salesforce.com/blogs/engineering/2015/03/use-pk-chunking-extract-large-data-sets-salesforce.html).

When performing PK Chunking, the origin cannot process deleted records.

Use the following guidelines when using the Bulk API with PK Chunking to process existing data:

SOQL queryUse the following query guidelines:

* Include the Id field in the SELECT statement.
* Optionally include a WHERE clause, but do not use the Id field in the WHERE clause.
* Do not include an ORDER BY clause.

The complete SOQL query for PK Chunking should use the following syntax:

```
SELECT Id, <field1>, <field2>, ... [WHERE <condition without the Id field>] FROM <object>
```

If you specify `SELECT * FROM <object>` in the SOQL query, the origin expands `*` to all fields in the Salesforce object that are accessible to the configured user. Note that the origin adds components of compound fields to the query, rather than adding the compound fields themselves. For example, the origin adds BillingStreet, BillingCity, etc., rather than adding BillingAddress. Similarly, it adds Location\_\_Latitude\_\_s and Location\_\_Longitude\_\_s rather than Location\_\_c.

Additional properties

Configure the following additional properties on the Query tab:

* Offset Field - The field to use for chunking. Must use the default Id field.
* Chunk Size - The range of values in the Id field to be queried at one time. The default is 100,000 and the maximum size is 250,000.
* Start ID - An optional lower boundary for the first chunk. When omitted, the origin begins processing with the first record in the object.

For example, when using a chunk size of 250,000 and a start ID of 001300000000000, the first query returns data with Id values starting with 001300000000000 with a chunk size of 250,000. The second query returns the next chunk of records.When using PK Chunking, the origin ignores the Initial Offset property and uses the optional Start ID instead.

**Example**

Say you want to replicate all data from the Salesforce Order object. The object contains a large number of records, so you want to use the Salesforce Bulk API with PK chunking.

To process the data, you configure the following properties on the Query tab:

* Use Bulk API - Enable the use of the Bulk API.
* Use PK Chunking - Enable the use of PK Chunking. PK Chunking must also be enabled in your Salesforce environment.
* Chunk Size - Set the chunk size to define the range values in the Id field that can be queried at one time. Use the maximum of 250,000 to return as many records as possible.
* Start Id - To process all available data, do not enter a value for this property. This property is used instead of Initial Offset to determine the lower boundary of the `Id` values to process.
*   SOQL Query - To process all data in the Order object, use the following query:

    ```
    SELECT * FROM Order
    ```

    Note that PK Chunking queries do not include an ORDER BY clause.
* Repeat Query - Set to No Repeat to run the query a single time.
* Initial Offset - Skip this property since PK Chunking uses the Start Id property instead.
* Offset Field - Use the default, `Id`, for the offset field.

#### Repeat Query <a href="#ariaid-title8" id="ariaid-title8"></a>

When the Salesforce origin processes existing data and is not subscribed to notifications, it can repeat the specified query at regular intervals. You can configure the origin to repeat a query in the following ways:

No repeatThe origin does not repeat the query. The origin runs the query once, and then the pipeline stops when it finishes processing all data from the Salesforce object.Repeat full queryWhen the origin repeats a full query, it runs the defined query using the initial offset or start ID as the offset value in the query each time it requests data.

Repeat a full query to capture all record updates. You might use a Record Deduplicator in the pipeline to minimize repeated records. Not ideal for objects with large numbers of records.Repeat incremental queryWhen the origin repeats an incremental query, it uses the initial offset or start ID as the offset value in the first query.

As the origin completes processing the results of the first query, it saves the last offset value that it processes. When it repeats the query, it uses the last-saved offset to perform an incremental query. The incremental query processes only the subset of data that arrived after the last query. When necessary, you can reset the origin to use the initial offset or start ID value.Repeat an incremental query for append-only objects or when you do not need to capture changes to older records.

#### Processing Deleted Records <a href="#ariaid-title9" id="ariaid-title9"></a>

The Salesforce origin can retrieve deleted records from the Salesforce recycle bin for processing.

The origin can process deleted records in either of the following conditions:

* Using the SOAP API version 39.0 or later.
* Using the Bulk API version 39.0 or later, when not using PK Chunking.

To process deleted records, use the Include Deleted Records property on the Query tab.

### Subscribing to Notifications <a href="#ariaid-title10" id="ariaid-title10"></a>

The Salesforce origin can subscribe to notifications to process the following Salesforce event types:

* PushTopic events from the Streaming API to receive notifications for changes to Salesforce data
* Platform events from CometD to process event-driven data
* Change events from CometD to process Change Data Capture data

#### Processing PushTopic Events <a href="#ariaid-title11" id="ariaid-title11"></a>

To configure the origin to subscribe to PushTopic event messages, you must first create a PushTopic in Salesforce based on a SOQL query. The PushTopic query defines which record create, update, delete, or undelete events generate a notification. If the record changes match the criteria of the PushTopic query, a notification is generated and received by subscribed clients.

The Salesforce origin is the client that subscribes to the PushTopic. In the origin configuration, you specify the name of the PushTopic, which subscribes the origin to the PushTopic channel.

When you start a pipeline configured to subscribe to Salesforce notifications, the pipeline runs continuously, receiving any changed data events in the origin as records.

Note: The Streaming API stores PushTopic events for 24 hours. If the pipeline stops and then restarts within 24 hours, the origin can receive notifications about past events. However, if the pipeline is inactive for more than 24 hours, the origin might miss some events.

For more information about creating PushTopic queries, see the Salesforce Streaming API developer documentation.

**PushTopic Event Record Format**

When the PushTopic encounters a change event that generates a notification, it sends the event to the subscribing Salesforce origin as a JSON message in the following format:

```
{
  "channel": "/topic/AccountUpdates",
  "clientId": "j24ylcz8l0t0fyp0pze6uzpqlt",
  "data": {
    "event": {
      "createdDate": "2016-09-15T06:01:40.000+0000",
      "type": "updated"
    },
    "sobject": {
      "AccountNumber": "3221320",
      "Id": "0013700000dC9xLAAS",
      "Name": "StreamSets",
      ...more fields...
    }
  }
}
```

The `data/event/type` property indicates the type of change - created, updated, deleted, or undeleted.

When the Salesforce origin receives the data, it creates a record with field names and values corresponding to the `data/sobject` property of the message.

The record also includes record header attributes corresponding to the `data/event` property of the message, as described in Salesforce Header Attributes.

#### Processing Platform Events <a href="#ariaid-title13" id="ariaid-title13"></a>

The Salesforce origin uses CometD to subscribe to platform events. Before processing platform events, set up the platform event channel name and define the platform event in your Salesforce environment.

When you configure the origin, you specify the channel name and the set of event messages to process. You can enable the Replay Option property to process platform events from the last 24 hours, as well as any new events. By default, the origin processes only the new events that arrive after you start the pipeline.

For more information about platform events, see the [Salesforce documentation](https://developer.salesforce.com/docs/atlas.en-us.platform\_events.meta/platform\_events/platform\_events\_intro.htm).

#### Processing Change Events <a href="#ariaid-title14" id="ariaid-title14"></a>

The Salesforce origin uses CometD to subscribe to change events for objects. For the origin to process change events, you must configure your Salesforce environment to enable Salesforce Change Data Capture for specific objects.

You can configure the origin to process a single object or you can configure the origin to process all objects enabled for Change Data Capture. You can enable the Replay Option property to process change events from the last 72 hours, as well as any new events. By default, the origin processes only the new events that arrive after you start the pipeline.

When processing change events, the origin creates record header attributes from the Salesforce change event header. For each field in the Salesforce change event header, the origin creates a record header attribute by adding the `salesforce.cdc.` prefix to the field. For example, the origin creates the `salesforce.cdc.entityName` record header attribute and sets its value to the value of the `entityName` field in the change event header.

Change events can apply to multiple Salesforce records. The `recordIds` field in the change event header lists the applicable record IDs. The origin creates the `salesforce.cdc.recordIds` record header attribute, which contains a comma-separated list of affected Salesforce records.

The origin sets other record header attributes appropriately. The origin sets the `sdc.operation.type` record header attribute to the CRUD operation value based on the `changeType` field in the change event header. The origin sets the `salesforce.sobjectType` record header attribute to the value of the `entityName` field in the change event header.

Note: The origin writes the value of the `entityName` field from the change event header to two record header attributes.

For more information about change events, see the [Salesforce documentation](https://developer.salesforce.com/docs/atlas.en-us.change\_data\_capture.meta/change\_data\_capture/cdc\_intro.htm).

### Reading Custom Objects or Fields <a href="#ariaid-title15" id="ariaid-title15"></a>

If the origin reads custom Salesforce objects or fields, you might want to use a Field Renamer in the pipeline to rename the custom fields.

If you extend Salesforce objects, custom object and field names are appended with the suffix `__c`. For example, if you create a custom Transaction object, Salesforce names the object `Transaction__c`. The Transaction object might contain fields named `Credit_Card__c, Fare_Amount__c, and Payment_Type__c`.

Instead of using field names appended with the suffix `__c` throughout the rest of the pipeline, you can add a Field Renamer to remove the suffix from the field names.

For more information about Salesforce custom objects, see the Salesforce documentation.

### Salesforce Attributes <a href="#ariaid-title16" id="ariaid-title16"></a>

The Salesforce origin generates Salesforce record header attributes and Salesforce field attributes that provide additional information about each record and field. The origin receives these details from Salesforce.

Salesforce attributes include a user-defined prefix to differentiate the Salesforce attributes from other attributes. By default, the prefix is `salesforce.`. You can change the prefix that the origin uses and you can configure the origin not to create Salesforce attributes.

#### Salesforce Header Attributes <a href="#ariaid-title17" id="ariaid-title17"></a>

The Salesforce origin generates Salesforce record header attributes that provide additional information about each record, such as the source objects for the record. The origin receives these details from Salesforce.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attribute.

The Salesforce origin can provide the following Salesforce header attributes:

| Salesforce Header Attribute          | Description                                                                                                                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| \<Salesforce prefix>sobjectType      | <p>Provides the Salesforce source object for the record.</p><p>Generated when the origin executes a query or subscribes to notifications.</p>                                        |
| \<Salesforce prefix>cdc.createdDate  | <p>Provides the date that the Salesforce PushTopic encountered the change event.</p><p>Generated when the origin subscribes to notifications.</p>                                    |
| \<Salesforce prefix>cdc.type         | <p>Provides the type of change that the Salesforce PushTopic encountered - created, updated, deleted, or undeleted.</p><p>Generated when the origin subscribes to notifications.</p> |
| salesforce.cdc.\<change event field> | <p>Provides the value of a field in the change event header.</p><p>Generated when the origin subscribes to Change Data Capture notifications.</p>                                    |

#### CRUD Operation Header Attribute <a href="#ariaid-title18" id="ariaid-title18"></a>

When the Salesforce origin subscribes to notifications and reads changed data from a PushTopic, the origin includes the CRUD operation type for a record in the sdc.operation.type header attribute.

If you use a CRUD-enabled destination in the pipeline such as JDBC Producer or Elasticsearch, the destination can use the operation type when writing to destination systems. When necessary, you can use an Expression Evaluator or scripting processors to manipulate the value in the `sdc.operation.type` header attribute.

The Salesforce origin uses the following values in the sdc.operation.type record header attribute to represent the operation type:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE
* 5 for unsupported operations
* 6 for UNDELETED

Tip: Records that are undeleted contain only the record ID. If you need the record data, you can use the Salesforce Lookup to retrieve it.

#### Salesforce Field Attributes <a href="#ariaid-title19" id="ariaid-title19"></a>

The Salesforce origin generates Salesforce field attributes that provide additional information about each field, such as the data type of the Salesforce field. The origin receives these details from Salesforce.

You can use the `record:fieldAttribute` or `record:fieldAttributeOrDefault` functions to access the information in the attribute.

The Salesforce origin can provide the following Salesforce field attributes:

| Salesforce Field Attribute         | Description                                                      |
| ---------------------------------- | ---------------------------------------------------------------- |
| \<Salesforce prefix>salesforceType | Provides the original Salesforce data type for the field.        |
| \<Salesforce prefix>length         | Provides the original length for all string and textarea fields. |
| \<Salesforce prefix>precision      | Provides the original precision for all double fields.           |
| \<Salesforce prefix>scale          | Provides the original scale for all double fields.               |
| \<Salesforce prefix>digits         | Provides the maximum number of digits for all integer fields.    |

### Event Generation <a href="#ariaid-title20" id="ariaid-title20"></a>

The Salesforce origin can generate events that you can use in an event stream. When you enable event generation, the origin generates an event when it completes processing the data returned by the specified query.

Salesforce events can be used in any logical way. For example:

*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin processes data based on how you configured the origin. For example, if you configure the origin to repeat an incremental query, the origin saves the offset when the executor stops the pipeline. When it restarts, the origin continues processing from the last-saved offset. If you configure the origin to repeat a full query, when you restart the pipeline, the origin uses the initial offset.
* With the Email executor to send a custom email after receiving an event.
* With a destination to store information about completed queries.

#### Event Record <a href="#ariaid-title21" id="ariaid-title21"></a>

Event records generated by the Salesforce origin have the following event-related record header attributes:

| Record Header Attribute       | Description                                                                                                                                              |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following type:</p><ul><li>no-more-data - Generated when the origin completes processing all data returned by a query.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                             |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                        |

The no-more-data event record includes no record fields.

### Changing the API Version <a href="#ariaid-title22" id="ariaid-title22"></a>

Data Pipelines ships with version 43.0 of the Salesforce Web Services Connector libraries. You can use a different Salesforce API version if you need to access functionality not present in version 43.0.

1. On the Salesforce tab, set the API Version property to the version that you want to use, for example 39.0.
2.  Download the relevant version of the following JAR files from Salesforce Web Services Connector (WSC):

    * WSC JAR file - force-wsc-\<version>.0.0.jar
    * Partner API JAR file - force-partner-api-\<version>.0.0.jar

    Where `<version>` is the API version number, for example, 39.

    For information about downloading libraries from Salesforce WSC, see the Salesforce Developer documentation.
3.  In the following Data Pipelines directory, replace the default force-wsc-43.0.0.jar and force-partner-api-43.0.0.jar files with the versioned JAR files that you downloaded:

    ```
    $SDC_DIST/streamsets-libs/streamsets-datacollector-salesforce-lib/lib/
    ```
4. Restart Data Pipelines for the changes to take effect.
