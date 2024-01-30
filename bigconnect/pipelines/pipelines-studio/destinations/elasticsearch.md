# Elasticsearch

\
The Elasticsearch destination writes data to an Elasticsearch cluster, including Elastic Cloud clusters (formerly Found clusters) and Amazon Elasticsearch Service clusters.&#x20;

The destination uses the Elasticsearch HTTP module to access the Bulk API and write each record to Elasticsearch as a document.

When you configure the Elasticsearch destination, you configure the HTTP URLs used to connect to the Elasticsearch cluster and specify whether security is enabled on the cluster. When Data Pipelines shares the same network as the Elasticsearch cluster, you can enter one or more node URLs and automatically detect additional Elasticsearch nodes on the cluster.

The Elasticsearch destination can use CRUD operations defined in the `sdc.operation.type` record header attribute to write data. You can define a default operation for records without the header attribute or value. You can also configure how to handle records with unsupported operations. For information about Data Pipelines change data processing and a list of CDC-enabled origins, see Processing Changed Data.

You can add advanced Elasticsearch properties as needed.

### Security <a href="#ariaid-title2" id="ariaid-title2"></a>

When security is enabled for the Elasticsearch cluster, you must specify the authentication method:

**Basic**

Use Basic authentication for Elasticsearch clusters outside of Amazon Elasticsearch Service. With Basic authentication, the stage passes the Elasticsearch user name and password.

**AWS Signature V4**

Use AWS Signature V4 authentication for Elasticsearch clusters within Amazon Elasticsearch Service. The stage must sign HTTP requests with Amazon Web Services credentials. For details, see the [Amazon Elasticsearch Service documentation](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-request-signing.html). Use one of the following methods to sign with AWS credentials:

**Instance profile**

When Data Pipelines runs on an Amazon EC2 instance that has an associated instance profile, the engine uses the instance profile credentials to automatically authenticate with AWS.

To use an instance profile, do not configure the Access Key ID and Secret Access Key properties.For more information about associating an instance profile with an EC2 instance, see the Amazon EC2 documentation.

**AWS access key pair**

When the execution engine does not run on an Amazon EC2 instance or when the EC2 instance doesn’t have an instance profile, you must specify the Access Key ID and Secret Access Key properties.

### Time Basis and Time-Based Indexes <a href="#ariaid-title3" id="ariaid-title3"></a>

The time basis is the time used by the Elasticsearch destination to write records to time-based indexes. When indexes have no time component, you can ignore the time basis property.

You can use the time of processing or the time associated with the data as the time basis.

For example, say you define the Index property using the following datetime variables:

```
logs-${YYYY()}-${MM()}-${DD()}
```

If you use the time of processing as the time basis, the destination write records to indexes based on when it processes each record. If you use the time associated with the data, such as a transaction timestamp, then the destination writes records to the indexes based on that timestamp.

You can use the following times as the time basis:

Processing Time

When you use processing time as the time basis, the destination writes to indexes based on the processing time and the index. To use the processing time as the time basis, use the following expression:

```
${time:now()}
```

This is the default time basis.

Record Time

When you use the time associated with a record as the time basis, you specify a date field in the record. The destination writes data to indexes based on the datetimes associated with the records.To use a time associated with the record, use an expression that calls a field and resolves to a datetime value, such as `${record:value("/Timestamp")}`.

### Document IDs <a href="#ariaid-title4" id="ariaid-title4"></a>

When appropriate, you can specify an expression that defines the document ID. When you do not specify an expression, Elasticsearch generates IDs for each document.

When you configure the destination to perform create, update, or delete operations, you must define the document ID.

For example, to perform updates for documents with IDs based on the EmployeeID field, define the write operation as update and define the Document ID as follows: `${record:value('/EmployeeID')}`.

You can also optionally define a parent ID for each document to define a parent/child relationship between documents in the same index.

### Define the CRUD Operation <a href="#ariaid-title5" id="ariaid-title5"></a>

The Elasticsearch destination can create, update, delete, or index data. The destination writes the records based on the CRUD operation defined in a CRUD operation header attribute or in operation-related stage properties.

You define the CRUD operation in the following ways:

CRUD record header attributeYou can define the CRUD operation in a CRUD operation record header attribute. The destination looks for the CRUD operation to use in the `sdc.operation.type` record header attribute.The attribute can contain one of the following numeric values:

* 1 for CREATE, the equivalent of INSERT
* 2 for DELETE
* 3 for UPDATE
* 4 for INDEX, the equivalent of UPSERT
* 8 for UPDATE with `doc_as_upsert`, the equivalent of MERGE

If your pipeline includes a CRUD-enabled origin that processes changed data, the destination simply reads the operation type from the `sdc.operation.type` header attribute that the origin generates. If your pipeline uses a non-CDC origin, you can use the Expression Evaluator or a scripting processor to define the record header attribute. For more information about Data Pipelines changed data processing and a list of CDC-enabled origins, see Processing Changed Data.

Operation stage properties

You define a default operation in the destination properties. The destination uses the default operation when the `sdc.operation.type` record header attribute is not set.You can also define how to handle records with unsupported operations defined in the `sdc.operation.type` header attribute. The destination can discard them, send them to error, or use the default operation.
