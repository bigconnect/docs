# Avro Data Format

Data Collector can read and write Avro data.

## Reading Avro Data

When reading Avro data, file- and object-based origins, such as the Directory and Amazon S3 origins, generate a Data Collector record for every Avro record within the processed file or object.

Message-based origins, such as the Kafka Consumer or TCP Server origins, generate a Data Collector record for every processed message.

Processors that read Avro data generate records as described in the processor overview.

Generated records include the Avro schema in the `avroSchema` record header attribute. They also include a `precision` and `scale` field attribute for each Decimal field.You can configure most stages to use Avro schemas stored in one of the following locations:

* An `avroSchema` record header attribute
* A stage configuration property
* Confluent Schema Registry

Some stages require that the Avro schema be stored in a particular location.

Some stages read data compressed by Avro-supported compression codecs without requiring additional configuration. You can configure some stages to read data compressed by other codecs.

## Writing Avro Data

When writing Avro data, destinations and processors write the data based on an Avro schema. The Avro schema can be located in one of the following locations:

* An `avroSchema` record header attribute
* A stage configuration property
* Confluent Schema Registry

Tip: When needed, you can use the Schema Generator processor to generate an Avro schema and write the schema to the `avroSchema` record header attribute.

Some stages automatically include the Avro schema in the output. Other stages can be configured to include the Avro schema in the output. You can compress the output data using an Avro-supported compression codec.

