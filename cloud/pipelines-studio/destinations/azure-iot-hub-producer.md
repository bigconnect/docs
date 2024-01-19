# Azure IoT Hub Producer

The Azure IoT Hub Producer destination writes data to Microsoft Azure IoT Hub. To write to Microsoft Azure Data Lake Storage, use the Azure Data Lake Storage destination. To write to Microsoft Azure Event Hub, use the Azure Event Hub Producer destination.&#x20;

Use the Azure IoT Hub Producer destination in a pipeline when you want to send device data - such as temperature or sensor data - to Azure IoT Hub after processing the data.

Data Pipelines functions as a simulated device that sends messages to Azure IoT Hub. Before you configure the Azure IoT Hub Producer destination, register Data Pipelines as a simulated device in your IoT hub.

When you configure the Azure IoT Hub Producer destination, you specify connection information such as the IoT hub name and device ID. You also specify the authentication key that the destination uses to identify itself when it sends messages to your IoT hub.

### Register Data Pipelines as an IoT Hub Device <a href="#ariaid-title2" id="ariaid-title2"></a>

Data Pipelines functions as a simulated device that sends messages to Azure IoT Hub. Before you configure the Azure IoT Hub Producer destination, register Data Pipelines as a device in your IoT hub.

Data Pipelines cannot connect to your IoT hub unless it has an entry in the identity registry. When you register Data Pipelines as a simulated device, Azure IoT Hub generates a unique device ID and authentication key that Data Pipelines uses to identify itself when it sends messages to your IoT hub.

Note: If you run pipelines that contain multiple Azure IoT Hub Producer destinations, you can configure each destination to use the same Data Pipelines device ID. Or, if you need to separate the data being sent by each pipeline, you can create a unique device ID for each destination.

1. Log in to the Azure portal: [https://portal.azure.com](https://portal.azure.com/).
2. Open your IoT hub.
3. In the Navigation panel, click Device Explorer.
4. To create a device, click Add.
5. On the Add Device page, enter the following information:
   * Device ID: Enter the ID to use for Data Pipelines. Device IDs are case sensitive.
   * Authentication Type: Select Symmetric Key.
   * Auto Generate Keys: Select the checkbox.
   * Connect device to IoT Hub: Click Enable.
6. Click Save.
7. In the Device Explorer pane, select the newly created device.
8.  Make a note of the device ID and the primary key. When you configure the Azure IoT Hub Producer destination, use these values for the Device ID property and the Shared Access Key stage property.

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/AzureIoTHubProducer\_Device.png)

### Data Formats <a href="#ariaid-title3" id="ariaid-title3"></a>

The Azure IoT Hub Producer destination writes data to Microsoft Azure IoT Hub based on the data format that you select. You can use the following data formats:

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
