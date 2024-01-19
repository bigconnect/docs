# Google BigQuery

The Google BigQuery destination streams data into Google BigQuery. You can use other destinations to write to Google Bigtable, Google Cloud Storage, and Google Pub/Sub.&#x20;

When you configure the destination, you define the existing BigQuery dataset and table to stream data into. The destination streams each record into a row in a BigQuery table. You can optionally define an expression to specify the insert ID to insert or update. The insert ID is a unique ID for each row. If you do not specify an insert ID, the destination inserts each record into a new row.

The destination maps fields from records to BigQuery columns based on matching names and compatible data types. You can configure the destination to ignore invalid columns when the destination cannot map a field to an existing BigQuery column. You can also configure the table cache size for the destination.

You define the project ID and credentials to use when connecting to Google BigQuery.

For more information about streaming data into Google BigQuery, see the [Google BigQuery documentation](https://cloud.google.com/bigquery/streaming-data-into-bigquery).

### BigQuery Data Types <a href="#ariaid-title2" id="ariaid-title2"></a>

The Google BigQuery destination maps fields from records to BigQuery columns in existing tables based on matching names and compatible data types. If needed, the destination converts Data Pipelines data types to BigQuery data types.

The following table lists the Data Pipelines data types and the BigQuery data types that the destination converts them to:

| Data Pipelines Data Type | BigQuery Data Type          |
| ------------------------ | --------------------------- |
| Boolean                  | Boolean                     |
| Byte Array               | Bytes                       |
| Date                     | Date                        |
| Datetime                 | Datetime or Timestamp       |
| Double                   | Float                       |
| Float                    | Float                       |
| Integer                  | Integer                     |
| List                     | Array                       |
| List-Map                 | Record with repeated fields |
| Long                     | Integer                     |
| Map                      | Record                      |
| Short                    | Integer                     |
| String                   | String                      |
| Time                     | Time                        |

The Google BigQuery destination cannot convert the following Data Pipelines data types:

* Byte
* Character
* Decimal

### Credentials <a href="#ariaid-title3" id="ariaid-title3"></a>

When the Google BigQuery destination streams data into Google BigQuery, the destination must pass credentials to BigQuery.

You can provide credentials using one the following options:

* Google Cloud default credentials
* Credentials in a file
* Credentials in a stage property

For details on how to configure each option, see Security in Google Cloud Stages.
