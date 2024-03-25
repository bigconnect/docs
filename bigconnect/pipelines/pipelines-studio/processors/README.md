# Processors

A processor stage represents a type of data processing that you want to perform. You can use as many processors in a pipeline as you need.

You can use different processors based on the execution mode of the pipeline: standalone or cluster. To help create or test pipelines, you can use development processors.

### Standalone Pipelines Only

In standalone pipelines, you can use the following processor:

* [Record Deduplicator](record-deduplicator.md) - Removes duplicate records.

### Standalone or Cluster Pipelines

In standalone or cluster pipelines, you can use the following processors:

* [Base64 Field Decoder](base64-field-decoder.md) - Decodes Base64 encoded data to binary data.
* [Base64 Field Encoder](base64-field-encoder.md) - Encodes binary data using Base64.
* [BigConnect Cypher ](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/950/bigconnect/pipelines/pipelines-studio/processors/bigconnect-cypher)- Execute Cypher queries for each row
* [Data Generator](data-generator.md) - Serializes a record into a field using the specified data format.
* [Data Parser](data-parser.md) - Parses NetFlow or syslog data embedded in a field.
* [Delay](delay.md) - Delays passing a batch to the rest of the pipeline.
* [Encrypt and Decrypt Fields](encrypt-and-decrypt-fields.md) - Encrypts or decrypts fields.
* [Expression Evaluator](expression-evaluator.md) - Performs calculations on data. Can also add or modify record header attributes.
* [Field Flattener](field-flattener.md) - Flattens nested fields.
* [Field Hasher](field-hasher.md) - Uses an algorithm to encode sensitive data.
* [Field Mapper](field-mapper.md) - Maps an expression to a set of fields to alter field paths, field names, or field values.
* [Field Masker](field-masker.md) - Masks sensitive string data.
* [Field Merger ](field-merger.md)- Merges fields in complex lists or maps.
* [Field Order](field-order.md) - Orders fields in a map or list-map root field type and outputs the fields into a list-map or list root field type.
* [Field Pivoter](field-pivoter.md) - Pivots data in a list, map, or list-map field and creates a record for each item in the field.
* [Field Remover](field-remover.md) - Removes fields from a record.
* [Field Renamer](field-renamer.md) - Renames fields in a record.
* [Field Replacer ](field-replacer.md)- Replaces field values.
* [Field Splitter](field-splitter.md) - Splits the string values in a field into different fields.
* [Field Type Converter](field-type-converter.md) - Converts the data types of fields.
* [Field Zip](field-zip.md) - Merges list data from two fields.
* [Geo IP-](geo-ip.md) Returns geolocation and IP intelligence information for a specified IP address.
* [Groovy Evaluator](groovy-evaluator.md) - Processes records based on custom Groovy code.
* [HBase Lookup](hbase-lookup.md) - Performs key-value lookups in HBase to enrich records with data.
* [Hive Metadata](hive-metadata.md) - Works with the Hive Metastore destination as part of the Drift Synchronization Solution for Hive.
* [HTTP Client](http-client.md) - The HTTP Client processor sends requests to an HTTP resource URL and writes the results to a field.
* [HTTP Router](http-router.md) - Routes data to different streams based on the HTTP method and URL path in record header attributes.
* [JavaScript Evaluator](javascript-evaluator.md) - Processes records based on custom JavaScript code.
* [JDBC Lookup](jdbc-lookup.md) - Performs lookups in a database table through a JDBC connection.
* [JDBC Tee](jdbc-tee.md) - Writes data to a database table through a JDBC connection, and enriches records with data from generated database columns.
* [JSON Generator](json-generator.md) - Serializes data from a field to a JSON-encoded string.
* [JSON Parser](json-parser.md) - Parses a JSON object embedded in a string field.
* [Jython Evaluator](jython-evaluator.md) - Processes records based on custom Jython code.
* [Log Parser ](log-parser.md)- Parses log data in a field based on the specified log format.
* [MLeap Evaluator](mleap-evaluator.md) - Uses a machine learning model stored in an MLeap bundle to generate evaluations, scoring, or classifications of data.
* [MongoDB Lookup](mongodb-lookup.md) - Performs lookups in MongoDB to enrich records with data.
* [PMML Evaluator](pmml-evaluator.md) - Uses a machine learning model stored in a PMML document to generate predictions or classifications of data.
* [PostgreSQL Metadata](postgresql-metadata.md) - Tracks structural changes in source data then creates and alters PostgreSQL tables as part of the Drift Synchronization Solution for PostgreSQL.
* [Redis Lookup](redis-lookup.md) - Performs key-value lookups in Redis to enrich records with data.
* [Schema Generator](schema-generator.md) - Generates a schema for each record and writes the schema to a record header attribute.
* [Spark Evaluator](spark-evaluator.md) - Processes data based on a custom Spark application.
* [SQL Parser ](sql-parser.md)- Parses SQL queries in a string field.
* [Start Pipelines](../origins/start-pipelines.md) - Starts a pipeline.
* [Static Lookup](static-lookup.md) - Performs key-value lookups in local memory.
* [Stream Selector](stream-selector.md) - Routes data to different streams based on conditions.
* [TensorFlow Evaluato](tensorflow-evaluator.md)r - Uses a TensorFlow machine learning model to generate predictions or classifications of data.
* [Whole File Transformer](whole-file-transformer.md) - Transforms Avro files to Parquet.
* [Windowing Aggregator](windowing-aggregator.md) - Performs aggregations within a window of time, displays the results in Monitor mode, and writes the results to events when enabled. This processor does not update the records being evaluated.
* [XML Flattener](xml-flattener.md) - Flattens XML data in a string field.
* [XML Parser](xml-parser.md) - Parses XML data in a string field.

