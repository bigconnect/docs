# Kafka Message Keys

You can configure the Kafka Consumer and Kafka Multitopic Consumer origins to capture the message keys included in each Kafka message and store them in generated records. Kafka message keys can be string values or Avro messages, depending on how your Kafka system is configured.

Store message key values in a record when you want to use the values in pipeline processing logic or to write them to destination systems. When you store message key values, you can also configure a Kafka Producer destination to pass those values to Kafka as Kafka message keys. This allows Kafka to reuse the original message keys instead of generating new message keys for the data.

If you have no need for message keys, you can discard them. The Kafka Consumer and Kafka Multitopic Consumer origins discard message keys by default.

### Storing Message Keys <a href="#ariaid-title2" id="ariaid-title2"></a>

You can configure the Kafka Consumer and Kafka Multitopic Consumer origins to capture Kafka message keys and store them in records. Store message key values when you want to use them in the pipeline, write them to destinations, or pass them to Kafka to use as message keys. By default, the origins discard Kafka message keys.

Kafka origins can store message key values in the following locations:In a specified record fieldStore message key values in a specified field when you want to perform processing based on field values. When you store message key values in a field, you can access the values easily with expression-based and field-based stages, such as the Stream Selector or Field Replacer processors.You might also store message key values in a field when you want to write message key values to destinations.In a specified record header attributeStore message key values in a specified record header attribute when you want to use the Kafka Producer to pass message key values to Kafka as Kafka message keys.You can use `record:attribute` functions to access string message key values from within the pipeline. You cannot use functions to access Avro message key values in record header attributes because the Avro data is binary.By default, record header attributes are not included in records when records are written to destination systems. To easily write message key values to destination systems, store the values in record fields.In a specified record field and record header attributeStore message key values in both a specified record field and a specified record header attribute when you have multiple uses for message key values.This option enables easy access to message key values in record fields and allows passing message key values to Kafka as Kafka message keys.

### Message Key Formats <a href="#ariaid-title3" id="ariaid-title3"></a>

Kafka message keys can be string values or Avro messages, depending on how your Kafka system is configured.

The format of the message keys determines how message key values are stored in the record, and how you work with those values.

#### String Message Keys <a href="#ariaid-title4" id="ariaid-title4"></a>

When Kafka message keys are string values, the string values are simply written to the specified record field or record header attribute, as expected.

The expression that you use to access message key values differs depending on where the values are stored:In record fieldsTo access message key values stored in a field, use the `record:value` function, as follows:

```
${record:value('/<message key field name>')}
```

Specify the field where message key values are stored, as defined in the Kafka origin. By default, Kafka origins use the following field path: `/kafkaMessageKey`.In record header attributesTo access message key values stored in a record header attribute, use the `record:attribute` function, as follows:

```
${record:attribute('<message key attribute name>'}
```

Specify the name of the attribute where message key values are stored, as defined in the Kafka origin. By default, Kafka origins use the following attribute name: `kafkaMessageKey`.

#### Avro Message Keys <a href="#ariaid-title5" id="ariaid-title5"></a>

When Kafka message keys are Avro messages, the message key values are handled differently depending on where they are stored:In record fieldsWhen you store Avro message key values in a field, the origin converts the binary Avro message key to a map and writes it to the specified field.Store Avro message key values in a field when you want to use message key values within a pipeline. You can access data within the map as you would any other map field.For example, to access string data in a first level `info` field in the `kafkaMessageKey` map field, you might use the following expression:

```
${record:value('/kafkaMessageKey/info')}
```

In record header attributesWhen you store Avro message key values in a record header attribute, the origin performs the following tasks:

* Stores the Avro schema in an `avroKeySchema` attribute.
* Encodes the binary Avro message key using Base64, then stores it in the specified attribute.

Store Avro message key values in a record header attribute when you want to pass the message key values to Kafka as Kafka message keys.Tip: To perform processing within a pipeline using Avro message key values, store them as record fields. To pass Avro message keys to Kafka as Kafka message keys, store them in record header attributes. To perform both tasks, store them in both locations.

### Passing Key Values to Kafka <a href="#ariaid-title6" id="ariaid-title6"></a>

You can configure the Kafka Producer destination to pass message key values stored in record header attributes to Kafka to be used as message keys. This enables Kafka to use existing message keys instead of creating new keys for the records.

To configure the destination to pass message key values to Kafka, configure the following properties:

* Kafka Message Key property on the Kafka tab - Define an expression that indicates the location of the message key values in the records.
* Message Key Format property on the Data format tab - Select the format of the message key values: String or Avro.

Tip: When you select the Message Key Format property, the destination provides a default expression in the Kafka Message Key property that you can edit as needed.The expression that you use in the Kafka Message Key property depends on the data format of the original message keys:StringFor string message keys, use the following expression for the Kafka Message Key property:

```
${record:attribute('<message key attribute name>'}
```

Specify the name of the attribute where message key values are stored, as defined in the Kafka origin. By default, Kafka origins use the following attribute name: `kafkaMessageKey`.AvroFor Avro message keys, use the following expression for the Kafka Message Key property:

```
${avro:decode(record:attribute('avroKeySchema'),base64:decodeBytes(record:attribute('<messsage key attribute name')))}
```

The first part of the expression defines the location of the schema for Avro message keys. Do not change this part of the expression.The second part of the expression defines where the message key values are stored. Specify the name of the attribute where message key values are stored, as defined in the Kafka origin. By default, Kafka origins use the following attribute name: `kafkaMessageKey`.

### Example: Storing and Passing Kafka Message Keys <a href="#ariaid-title7" id="ariaid-title7"></a>

Say you want to use string Kafka message key values to route records to different processing streams. Then, after all pipeline processing is complete, you want to write records to a different Kafka topic and you want the resulting messages to use the original message keys.

To do this, use either the Kafka Consumer or Kafka Multitopic Consumer origin. Then, on the Kafka tab:

* Set the Key Capture Mode property to Record Header and Field.
* Use the default value for the Key Capture Field property, `/kafkaMessageKey`.
* Use the default value for the Key Capture Header Attribute property, `kafkaMessageKey`.

This enables the origin to capture and store the Kafka message keys in the specified field and attribute.

Then, add a Stream Selector to the pipeline to route data to different processing streams based on the values in the `/kafkaMessageKey` field path.

To write the data and pass message key values to Kafka, add a Kafka Producer destination, and then:

* On the Data Format tab of the destination, select String for the Message Key Format property.
* On the Kafka tab, verify that the default expression for the Kafka Message Key property provides message key values to Kafka: `${record:value('/kafkaMessageKey')}`.

This enables the Kafka Producer to pass message key values stored in the `kafkaMessageKey` record header attribute to Kafka to be used as message keys.
