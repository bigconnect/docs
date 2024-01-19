# Processors

A processor stage represents a type of data processing that you want to perform. You can use as many processors in a pipeline as you need.

You can use different processors based on the execution mode of the pipeline: standalone or cluster. To help create or test pipelines, you can use development processors.

### Standalone Pipelines Only

In standalone pipelines, you can use the following processor:

* **Record Deduplicator** - Removes duplicate records.

### Standalone or Cluster Pipelines

In standalone or cluster pipelines, you can use the following processors:

* **Base64 Field Decoder** - Decodes Base64 encoded data to binary data.
* **Base64 Field Encoder** - Encodes binary data using Base64.
* **Couchbase Lookup** - Performs lookups in Couchbase Server to enrich records with data.
* **Data Generator** - Serializes a record into a field using the specified data format.
* **Data Parser** - Parses NetFlow or syslog data embedded in a field.
* **Delay** - Delays passing a batch to the rest of the pipeline.
* **Encrypt and Decrypt Fields** - Encrypts or decrypts fields.
* **Expression Evaluator** - Performs calculations on data. Can also add or modify record header attributes.
* Field Flattener - Flattens nested fields.
* Field Hasher - Uses an algorithm to encode sensitive data.
* Field Mapper - Maps an expression to a set of fields to alter field paths, field names, or field values.
* Field Masker - Masks sensitive string data.
* Field Merger - Merges fields in complex lists or maps.
* Field Order - Orders fields in a map or list-map root field type and outputs the fields into a list-map or list root field type.
* Field Pivoter - Pivots data in a list, map, or list-map field and creates a record for each item in the field.
* Field Remover - Removes fields from a record.
* Field Renamer - Renames fields in a record.
* Field Replacer - Replaces field values.
* Field Splitter - Splits the string values in a field into different fields.
* Field Type Converter - Converts the data types of fields.
* Field Zip - Merges list data from two fields.
* Geo IP- Returns geolocation and IP intelligence information for a specified IP address.
* Groovy Evaluator - Processes records based on custom Groovy code.
* HBase Lookup - Performs key-value lookups in HBase to enrich records with data.
* Hive Metadata - Works with the Hive Metastore destination as part of the Drift Synchronization Solution for Hive.
* HTTP Client - The HTTP Client processor sends requests to an HTTP resource URL and writes the results to a field.
* HTTP Router - Routes data to different streams based on the HTTP method and URL path in record header attributes.
* JavaScript Evaluator - Processes records based on custom JavaScript code.
* JDBC Lookup - Performs lookups in a database table through a JDBC connection.
* JDBC Tee - Writes data to a database table through a JDBC connection, and enriches records with data from generated database columns.
* JSON Generator - Serializes data from a field to a JSON-encoded string.
* JSON Parser - Parses a JSON object embedded in a string field.
* Jython Evaluator - Processes records based on custom Jython code.
* Kudu Lookup - Performs lookups in Kudu to enrich records with data.
* Log Parser - Parses log data in a field based on the specified log format.
* MLeap Evaluator - Uses a machine learning model stored in an MLeap bundle to generate evaluations, scoring, or classifications of data.
* MongoDB Lookup - Performs lookups in MongoDB to enrich records with data.
* PMML Evaluator - Uses a machine learning model stored in a PMML document to generate predictions or classifications of data.
* PostgreSQL Metadata - Tracks structural changes in source data then creates and alters PostgreSQL tables as part of the Drift Synchronization Solution for PostgreSQL.
* Redis Lookup - Performs key-value lookups in Redis to enrich records with data.
* Salesforce Lookup - Performs lookups in Salesforce to enrich records with data.
* Schema Generator - Generates a schema for each record and writes the schema to a record header attribute.
* Spark Evaluator - Processes data based on a custom Spark application.
* SQL Parser - Parses SQL queries in a string field.
* Start Job - Starts a Control Hub job.
* Start Pipeline - Starts a Data Collector, Data Collector Edge, or Transformer pipeline.
* Static Lookup - Performs key-value lookups in local memory.
* Stream Selector - Routes data to different streams based on conditions.
* TensorFlow Evaluator - Uses a TensorFlow machine learning model to generate predictions or classifications of data.
* Value Replacer (Deprecated) - Replaces existing nulls or specified values with constants or nulls.
* Whole File Transformer - Transforms Avro files to Parquet.
* Windowing Aggregator - Performs aggregations within a window of time, displays the results in Monitor mode, and writes the results to events when enabled. This processor does not update the records being evaluated.
* XML Flattener - Flattens XML data in a string field.
* XML Parser - Parses XML data in a string field.

### Development Processors

To help create or test pipelines, you can use the following development processors:

* Dev Identity
* Dev Random Error
* Dev Record Creator

