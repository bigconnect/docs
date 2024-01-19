# Google Bigtable

The Google Bigtable destination writes data to Google Cloud Bigtable. You can use other destinations to write to Google BigQuery, Google Cloud Storage, and Google Pub/Sub.&#x20;

Before you configure the destination, you must complete several prerequisites including installing the BoringSSL library and configuring the Google Cloud default credentials.

When you configure the destination, you define the instance ID, project ID, and the table that you want to write to. The destination can write to an existing table or can create and write to a new table. You specify the row key for the table, and then map fields from the pipeline to Cloud Bigtable columns.

When necessary, you can configure a time basis and specify the number of records to buffer in each commit to Cloud Bigtable.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you can write to Cloud Bigtable, you must complete the following prerequisites:

* Download and install the BoringSSL library.
* Configure the Google Cloud credentials.

#### Install the BoringSSL Library <a href="#ariaid-title3" id="ariaid-title3"></a>

The Google Bigtable destination requires the BoringSSL library. You must download and install the external library so that the Google Bigtable destination can access it.

Download the BoringSSL library from the following location:

[https://repo1.maven.org/maven2/io/netty/netty-tcnative-boringssl-static/1.1.33.Fork19/netty-tcnative-boringssl-static-1.1.33.Fork19.jar](https://repo1.maven.org/maven2/io/netty/netty-tcnative-boringssl-static/1.1.33.Fork19/netty-tcnative-boringssl-static-1.1.33.Fork19.jar)

#### Configure Credentials <a href="#ariaid-title4" id="ariaid-title4"></a>

Configure the Google Cloud default credentials that the Google Bigtable destination uses to connect to Google Cloud Bigtable.

1.  Create a JSON file that contains the Google Application Default Credentials.

    To create the credentials file, use the Google API Console or run the following Google Cloud Platform command: `gcloud auth login`.

    For more information about creating the credentials file, search for Google Application Default Credentials in the Google Developer documentation.
2.  Store the generated credentials file in a local directory external to the Data Pipelines installation directory.For example, if you installed Data Pipelines in the following directory:

    ```
    /opt/sdc/
    ```

    you might store the credentials file at:

    ```
    /opt/sdc-credentials
    ```
3.  Add the `GOOGLE_APPLICATION_CREDENTIALS` environment variable to the appropriate file and point it to the credentials file.

    Modify environment variables using the method required by your installation type.

    Set the environment variable as follows:

    ```
    export GOOGLE_APPLICATION_CREDENTIALS="/opt/sdc-credentials/keyfile.json"
    ```
4. Restart Data Pipelines to enable the changes.

### Row Key <a href="#ariaid-title5" id="ariaid-title5"></a>

Each Google Cloud Bigtable table has one index, the row key. When you configure the Google Bigtable destination, you define which field or fields in the record to use as the row key.

Define the row key in one of the following ways:

Single field as the row key

You can define a single field as the row key by clearing the Row Key is Composite property and then defining the field to use. Enter the path to each field.

For example, the following configuration uses the ID field in the record as the row key:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Bigtable-SingleRowKey.png)

**Multiple fields as the row key**

You can define multiple fields as the row key by selecting the Row Key is Composite property and then defining multiple fields to use. Enter the path to each field.

When you define multiple fields, the destination concatenates the values of the fields into the row key. The destination adds fields to the composite row key in the order that they are listed in the stage configuration.If you include a field with the String data type in the composite row key, then you must also define the column width of the field. The destination pads or truncates the string value to the specified width as needed. Fields with other data types use their standard width, for example 8 bytes for the Long data type and 4 bytes for the Integer data type. The destination ignores column widths defined for fields with data types other than String.

For example, let's assume that you are writing to a Cloud Bigtable table that uses the customer last name and phone number as the row key. In the destination, you select Row Key is Composite and then define the LastName and PhoneNumber fields as the row key fields. You define the column width of the LastName field as 8, as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Bigtable-CompositeRowKey.png)

When the pipeline runs, a last name of "Lee" is padded with five binary zeroes and then concatenated with the phone number, while a last name of "Patterson" is truncated and then concatenated with the phone number.You must design the row key in the Cloud Bigtable table to include the same multiple values. Otherwise, your Google application cannot retrieve the data from the composite row key. For more information about using multiple values in a single row key, see the Google Cloud Bigtable documentation.

If one or more fields defined as the row key are missing, the destination sends that record to the pipeline for error handling.

### Cloud Bigtable Data Types <a href="#ariaid-title6" id="ariaid-title6"></a>

When you map record fields to Google Cloud Bigtable columns, you specify whether the Cloud Bigtable storage type is text or binary. The destination converts the data types of the record fields to the Cloud Bigtable storage types.

The destination cannot convert the Data Pipelines List and Map data types. The destination converts Date, Datetime, and Time data types to the Text storage type with a default date format. To use a specific date format, use a Field Type Converter processor earlier in the pipeline to convert Date, Datetime, and Time fields to the String data type with the required date format. Then, you can map the String field to a Cloud Bigtable column with the Text storage type.

The following table lists the Data Pipelines data types that the destination can convert to a Cloud Bigtable storage type:

| Data Pipelines Data Type | Cloud Bigtable Storage Type |
| ------------------------ | --------------------------- |
| Boolean                  | Text, Binary                |
| Byte                     | Text, Binary                |
| Byte Array               | Text, Binary                |
| Character                | Text, Binary                |
| Date                     | Text                        |
| Datetime                 | Text                        |
| Decimal                  | Text, Binary                |
| Double                   | Text, Binary                |
| Float                    | Text, Binary                |
| Integer                  | Text, Binary                |
| Long                     | Text, Binary                |
| Short                    | Text, Binary                |
| String                   | Text                        |
| Time                     | Text                        |

### Column Family and Field Mappings <a href="#ariaid-title7" id="ariaid-title7"></a>

When you configure the Google Bigtable destination, you map record fields to Google Cloud Bigtable columns. You define the Cloud Bigtable columns to write to by defining the column family and column qualifier.

You can specify a default column family to use for all columns, or you can explicitly define the column family for each field mapping using the following format: `<column family>:<qualifier>`. Use the default or the explicit column family mapping based on the following scenarios:

All columns belong to the same column familyWhen all columns written to the table belong to the same column family, you can enter a default column family, clear the Explicit Column Family Mapping property, and then enter only the qualifier when you map fields to columns.

Note: When you enter a default column family and clear the Explicit Column Family Mapping, any column family defined in a field to column mapping is ignored. The destination always uses the default column family.

For example, to map record fields to the Cloud Bigtable columns `cf:a`, `cf:b`, and `cf:c`, enter `cf` as the default column family. Clear Explicit Column Family Mapping, and then in the field to column mapping, define the columns using only the qualifier, like so: `a`, `b`, `c`.The following image shows the destination configured to use a default column family for all columns:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Bigtable-SameColumnFamily.png)

Columns belong to different column familiesWhen columns written to the table belong to different column families, do not enter a default column family. Instead, select the Explicit Column Family Mapping property, and then enter the column family and qualifier when you map fields to columns using the following format: `<column family>:<qualifier>`.

For example, to map record fields to the Cloud Bigtable columns `cf1:a`, `cf1:b`, and `cf2:a`, do not enter a value for the default column family. Select Explicit Column Family Mapping, and then in the field to column mapping, define the columns using the `<column family>:<qualifier>` format, like so: `cf1:a`, `cf1:b`, `cf2:a`.

The following image shows the destination configured to explicitly define the column family for each field mapping:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Bigtable-DifferentColumnFamily.png)

Columns belong to different column families, but you’d like to use a default value for a group of columnsWhen columns written to the table belong to different column families, you can use a default column family for a group of columns. Then, you can override the default for some of the columns by selecting the Explicit Column Family Mapping property. When you map fields to columns, enter only the qualifier to use the default column family. Enter the column family and qualifier using the format `<column family>:<qualifier>` to override the default column family.

For example, to map record fields to the Cloud Bigtable columns `cf1:a`, `cf1:b`, and `cf2:a`, enter `cf1` for the default column family. Select Explicit Column Family Mapping so that you can override the default column family. Then in the field to column mapping, define the columns like so, using the `<column family>:<qualifier>` format to override the default column family: `a`, `b`, `cf2:a`.The following image shows the destination configured to use a default column family where some field mappings override the default value:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Bigtable-DifferentColumnFamilyWithDefault.png)

### Time Basis <a href="#ariaid-title8" id="ariaid-title8"></a>

The time basis determines the timestamp value added for each column written to Google Cloud Bigtable.

You can use the following times as the time basis:

**Pipeline start time**

When you use pipeline start time as the time basis, the destination uses the time that the pipeline started as the timestamp value. The timestamp value is an epoch or UNIX time in milliseconds. The processing time is calculated once per pipeline.

**Batch start time**

When you use batch start time as the time basis, the destination uses the time that Data Pipelines starts processing the current batch as the timestamp value. The timestamp value is an epoch or UNIX time in milliseconds. The processing time is calculated once per batch.System time for each recordWhen you use system time as the time basis, the destination uses the timestamp value automatically generated by Cloud Bigtable when the column is written to Cloud Bigtable. The timestamp value is an epoch or UNIX time in milliseconds.

This is the default time basis.Long value from field in the recordWhen you use the time associated with the record as the time basis, you specify a field in the record that contains a Long value with an epoch or UNIX time in milliseconds. The destination uses the field value as the timestamp value. If the field value is not a Long, then the destination sends the record to the pipeline for error handling.

Tip: You can use a time function, such as time:dateTimeToMilliseconds or time:extractLongFromDate, in a processor earlier in the pipeline to convert a Datetime value to the Long data type.
