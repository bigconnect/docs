# Azure Event Hub Producer

The Azure Event Hub Producer writes data to Microsoft Azure Event Hub.

To write to Microsoft Azure Data Lake Storage, use the Azure Data Lake Storage destination. To write to Microsoft Azure IoT Hub, use the Azure IoT Hub Producer destination.&#x20;

When you configure the Azure Event Hub Producer, you specify the Microsoft Azure namespace and event hub names. You also define the shared access policy name and connection string key.

### Data Formats <a href="#ariaid-title2" id="ariaid-title2"></a>

The Azure Event Hub Producer destination writes data to Microsoft Azure Event Hub based on the data format that you select. You can use the following data formats:

**Binary**

The stage writes binary data to a single field in the record.

**JSON**

The destination writes records as JSON data. You can use one of the following formats:

* Array - Each file includes a single array. In the array, each element is a JSON representation of each record.
* Multiple objects - Each file includes multiple JSON objects. Each object is a JSON representation of a record.

**Record**

The destination writes records in the Record data format.

**Text**

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

**XML**

The destination creates a valid XML document for each record. The destination requires the record to have a single root field that contains the rest of the record data. For details and suggestions for how to accomplish this, see Record Structure Requirement.

The destination can include indentation to produce human-readable documents. It can also validate that the generated XML conforms to the specified schema definition. Records with invalid schemas are handled based on the error handling configured for the destination.
