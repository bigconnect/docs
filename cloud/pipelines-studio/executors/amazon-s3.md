# Amazon S3

The Amazon S3 executor performs a task in Amazon S3 each time it receives an event.&#x20;

Upon receiving an event, the executor can perform one of the following tasks:

* Create a new Amazon S3 object for the specified content
* Copy an object under 5 GB to another location in the same bucket and optionally delete the original object
* Adds tags to an existing object

Each Amazon S3 executor can perform one type of task. To perform additional tasks, use additional executors.

Use the Amazon S3 executor as part of an event stream. You can use the executor in any logical way, such as writing information from an event record to a new S3 object, or copying or tagging objects after they are written by the Amazon S3 destination.

When you configure the Amazon S3 executor, you specify the connection information, such as access keys, region, and bucket. You configure the expression that represents the object name and location. When creating new objects, you specify the content to place in the objects. When copying objects, you specify the location of the object and the location for the copy. You can also configure the executor to delete the original object after it is copied. When adding tags to an existing object, you specify the tags that you want to use.

You can optionally use an HTTP proxy to connect to Amazon S3.

You can also configure the executor to generate events for another event stream. For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

### Authentication Method <a href="#ariaid-title2" id="ariaid-title2"></a>

You can configure the Amazon S3 executor to authenticate with Amazon Web Services (AWS) using an instance profile or AWS access keys. When accessing a public bucket, you can connect anonymously using no authentication.

For more information about the authentication methods and details on how to configure each method, see Security in Amazon Stages.

### Create New Objects <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use the Amazon S3 executor to create new Amazon S3 objects and write the specified content to the object when the executor receives an event record.

When you create an object, you specify where to create the object and the content to write to the object. You can use an expression to represent both the location for the object and the content to use.

For example, say you want the executor to create a new Amazon S3 object for each object that the Amazon S3 destination writes, and to use the new object to store the record count information for each written object. Since the object-written event record includes the record count, you can enable the destination to generate records and route the event to the Amazon S3 executor.

The object-written event record includes the bucket and object key of the written object. So, to create a new record-count object in the same bucket as the written object, you can use the following expression for the Object property, as follows:

```
${record:value('/bucket')}/${record:value('/objectKey')}.recordcount
```

The event record also includes the number of records written to the object. So, to write this information to the new object, you can use the following expression for the Content property, as follows:

```
${record:value('/recordCount')}
```

Tip: Stage-generated event records differ from stage to stage. For a description of stage events, see "Event Record" in the documentation for the event-generating stage. For a description of pipeline events, see Pipeline Event Records.

### Copy Objects <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use the Amazon S3 executor to copy an object to another location within the same bucket when the executor receives an event record. You can optionally delete the original object after the copy. The object must be under 5 GB in size.

When you copy an object, you specify the location of the object to be copied, and the location for the copy. The target location must be within the same bucket as the original object. You can use an expression to represent both locations. You can also specify whether to delete the original object.

A simple example is to move each written object to a Completed directory after it is closed. To do this, you configure the Amazon S3 destination to generate events. Since the object-written event record includes the bucket and object key, you can use that information to configure the Object property, as follows:

```
${record:value('/bucket')}/${record:value('/objectKey')}
```

Then, to move the object to a Completed directory, retaining the same object name, you can configure the New Object Path property, as follows:

```
${record:value('/bucket')}/completed/${record:value('/objectKey')}
```

You can then select Delete Original Object to remove the original object.

To do something more complicated, like move only the subset of objects with a \_west suffix to a different location, you can add a Stream Selector processor in the event stream to route only events where the /objectKey field includes a \_west suffix to the Amazon S3 executor.

### Tag Existing Objects <a href="#ariaid-title5" id="ariaid-title5"></a>

You can use the Amazon S3 executor to add tags to existing Amazon S3 objects. Tags are key-value pairs that you can use to categorize objects, such as product: \<product>.

You can configure multiple tags. When you configure a tag, you can define a tag with just the key or specify a key and value. You can also use expressions to define tag values.

For example, you can use an expression to specify the number of records that were written to an object based on the recordCount field in the event record, as follows:

```
key: processed records
value: ${record:value('/recordCount')}
```

For more information about tags, including Amazon S3 restrictions, see the [Amazon S3 documentation](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using\_Tags.html).

### Event Generation <a href="#ariaid-title6" id="ariaid-title6"></a>

The Amazon S3 executor can generate events that you can use in an event stream. When you enable event generation, the executor generates events each time it creates a new object, adds tags to an existing object, or completes copying an object to a new location.

Amazon S3 events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title7" id="ariaid-title7"></a>

Event records generated by the Amazon S3 executor have the following event-related record header attributes. Record header attributes are stored as String values.

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                              |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| sdc.event.type                | <p>Event type. Uses the following event types:</p><ul><li>file-changed - Generated when the executor adds tags to an existing object.</li><li>file-created - Generated when the executor creates a new object.</li><li>file-moved - Generated when the executor completes copying an object to a new location.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                             |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                        |

The Amazon S3 executor can generate the following types of event records:file-changed

The executor generates a file-changed event record when it adds tags to an existing object.

File-changed event records have the `sdc.event.type` record header attribute set to `file-changed` and include the following field:

| Event Field Name | Description               |
| ---------------- | ------------------------- |
| object\_key      | Key of the tagged object. |

file-created

The executor generates a file-created event record when it creates a new object.

File-created event records have the `sdc.event.type` record header attribute set to `file-created` and include the following field:

| Event Field Name | Description                |
| ---------------- | -------------------------- |
| object\_key      | Key of the created object. |

file-moved

The executor generates a file-moved event record when it completes copying an object to a new location.

File-moved event records have the `sdc.event.type` record header attribute set to `file-moved` and include the following field:

| Event Field Name | Description               |
| ---------------- | ------------------------- |
| object\_key      | Key of the copied object. |
