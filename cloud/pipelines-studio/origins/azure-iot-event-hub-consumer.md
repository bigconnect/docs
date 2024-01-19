# Azure IoT/Event Hub Consumer

The Azure IoT/Event Hub Consumer origin reads data from Microsoft Azure Event Hub. The origin can use multiple threads to enable parallel processing of data from a single Azure event hub.&#x20;

Before you use the Azure IoT/Event Hub Consumer origin, make sure you have the required Microsoft Azure storage account and container.

When you configure the Azure IoT/Event Hub Consumer, you specify the Microsoft Azure namespace and event hub names. You also define the shared access policy name and connection string key. You specify the consumer group to use and an event processor prefix that the origin uses when communicating with Azure Event Hub.

You configure the storage account details, such as the storage account name and key. And you specify the number of threads to use during processing.

### Storage Account and Container Prerequisite <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you use the Azure IoT/Event Hub Consumer origin, you need a Microsoft Azure storage account and at least one container.

The origin stores offsets in a storage account container, so to ensure the integrity of offset information, you must use a different container for each pipeline that includes an Azure IoT/Event Hub Consumer origin.

For example, say you use the Azure IoT/Event Hub Consumer as the origin for an IoT pipeline and a Transactions pipeline. To keep the offset data for these pipelines separate, you need to use two different storage account containers. They can be in the same storage account or in different storage accounts. When you configure the origins, you specify the storage account and container to use.

To create a new container for the pipeline:

1. Log into the Microsoft Azure portal: [https://portal.azure.com](https://portal.azure.com/)
2. In the Navigation panel, click Storage Accounts.
3.  Select the storage account to use.

    If you need to create a storage account, click the Add icon. Enter a name for the storage account, and enter or select a resource group name. You can use the defaults for all other properties.
4. In the storage account view, click + Container to create a container.
5. Enter a container name, and click OK.Tip: Use a name that can be easily identified as the container for the pipeline that you want to use it in.

If these steps are no longer accurate, see the Microsoft Azure Event Hub documentation.

## Resetting the Origin in Event Hub <a href="#ariaid-title3" id="ariaid-title3"></a>

You cannot use BigConnect Pipelines to reset the origin for Azure IoT/Event Hub Consumer pipelines because the offset is stored in Azure Event Hub.

To reset the origin in Microsoft Azure Event Hub:

1. In the Microsoft Azure portal, navigate to the storage account.
2.  To delete the offset information stored for the pipeline, delete the container that the pipeline uses.

    This can take some time. Allow the portal to complete the removal of the container before continuing.
3. To enable the pipeline to store new offset information when you restart the pipeline, create a new container with the same name. Or, use a different name and update the Container Name property in the pipeline.

## Multithreaded Processing <a href="#ariaid-title4" id="ariaid-title4"></a>

The Azure IoT/Event Hub Consumer origin performs parallel processing and enables the creation of a multithreaded pipeline.

The Azure IoT/Event Hub Consumer origin uses multiple concurrent threads to read from an event hub based on the Max Threads property. When you start the pipeline, the origin creates the number of threads specified in the Max Threads property. Each thread connects to the origin system, creates a batch of data, and passes the batch to an available pipeline runner.

A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin. Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

For example, say you set the Max Threads property to 5. When you start the pipeline, the origin creates five threads, and BigConnect Pipelines creates a matching number of pipeline runners. Upon receiving data, the origin passes a batch to each of the pipeline runners for processing.

Each pipeline runner performs the processing associated with the rest of the pipeline. After a batch is written to pipeline destinations, the pipeline runner becomes available for another batch of data. Each batch is processed and written as quickly as possible, independent from other batches processed by other pipeline runners, so batches may be written differently from the read-order.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

### Data Formats <a href="#ariaid-title5" id="ariaid-title5"></a>

The Azure IoT/Event Hub Consumer origin reads data from Microsoft Azure Event Hub based on the data format that you select. You can use the following data formats:

**Binary**

Generates a record with a single byte array field at the root of the record.When the data exceeds the user-defined maximum data size, the origin cannot process the data. Because the record is not created, the origin cannot pass the record to the pipeline to be written as an error record. Instead, the origin generates a stage error.

**JSON**

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin processes the object based on the error handling configured for the stage.

**Pipelines Record**

Generates a record for every record. Use to process records generated by a BigConnect Pipelines pipeline using the Pipelines Record data format.For error records, the origin provides the original record as read from the origin in the original pipeline, as well as error information that you can use to correct the record.When processing error records, the origin expects the error file names and contents as generated by the original pipeline.

**Text**

Generates a record for each line of text or for each section of text based on a custom delimiter.When a line or section exceeds the maximum line length defined for the origin, the origin truncates it. The origin adds a boolean field named Truncated to indicate if the line was truncated.

### &#x20;<a href="#ariaid-title6" id="ariaid-title6"></a>

\
