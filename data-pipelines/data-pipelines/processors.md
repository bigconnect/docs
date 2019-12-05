# Processors

A processor stage represents a type of data processing that you want to perform. You can use as many processors in a pipeline as you need.

You can use different processors based on the execution mode of the pipeline: standalone, cluster, or edge. To help create or test pipelines, you can use development processors.

### Standalone Pipelines Only

In standalone pipelines, you can use the following processor:

* [Record Deduplicator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/RDeduplicator.html#concept_z3m_v52_zq) - Removes duplicate records.

### Standalone or Cluster Pipelines

In standalone or cluster pipelines, you can use the following processors:

* [Base64 Field Decoder](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Base64Decoder.html#concept_ujj_spy_kv) - Decodes Base64 encoded data to binary data.
* [Base64 Field Encoder](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Base64Encoder.html#concept_wtr_mpy_kv) - Encodes binary data using Base64.
* [Control Hub API](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/ControlHubAPI.html#concept_akz_zsr_2jb) - Calls a Control Hub API.
* [Couchbase Lookup](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/CouchbaseLookup.html#concept_rxk_1dq_2fb) - Performs lookups in Couchbase Server to enrich records with data.
* [Databricks ML Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/DatabricksML.html#concept_nlz_k3v_gfb) - Uses a machine learning model exported with Databricks ML Model Export to generate evaluations, scoring, or classifications of data.
* [Data Generator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/DataGenerator.html#concept_hw1_gq4_3fb) - Serializes a record into a field using the specified data format.
* [Data Parser](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/DataParser.html#concept_xw3_4xk_r1b) - Parses NetFlow or syslog data embedded in a field.
* [Delay](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Delay.html#concept_ez5_pvf_wbb) - Delays passing a batch to the rest of the pipeline.
* [Encrypt and Decrypt Fields](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/EncryptDecrypt.html#concept_zs3_vfk_hfb) - Encrypts or decrypts fields.
* [Expression Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Expression.html#concept_zm2_pp3_wq) - Performs calculations on data. Can also add or modify record header attributes.
* [Field Flattener](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldFlattener.html#concept_njn_3kk_fx) - Flattens nested fields.
* [Field Hasher](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldHasher.html#concept_ivv_c3k_wq) - Uses an algorithm to encode sensitive data.
* [Field Mapper](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldMapper.html#concept_q5y_tdq_xgb) - Maps an expression to a set of fields to alter field paths, field names, or field values.
* [Field Masker](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldMasker.html#concept_hjc_t4k_wq) - Masks sensitive string data.
* [Field Merger](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldMerger.html#concept_pgm_tsl_gt) - Merges fields in complex lists or maps.
* [Field Order](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldOrder.html#concept_krp_5fv_vy) - Orders fields in a map or list-map root field type and outputs the fields into a list-map or list root field type.
* [Field Pivoter](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/ListPivoter.html#concept_ekg_313_qw) - Pivots data in a list, map, or list-map field and creates a record for each item in the field.
* [Field Remover](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldRemover.html#concept_jdd_blr_wq) - Removes fields from a record.
* [Field Renamer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldRenamer.html#concept_vyv_zsg_ht) - Renames fields in a record.
* [Field Replacer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldReplacer.html#concept_rw4_2d3_4cb) - Replaces field values.
* [Field Splitter](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldSplitter.html#concept_vlj_vph_yq) - Splits the string values in a field into different fields.
* [Field Type Converter](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldTypeConverter.html#concept_is3_zkp_wq) - Converts the data types of fields.
* [Field Zip](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldZip.html#concept_o3b_t1k_yx) - Merges list data from two fields.
* [Geo IP](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/GeoIP.html#concept_fch_fc3_ms)- Returns geolocation and IP intelligence information for a specified IP address.
* [Groovy Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Groovy.html#concept_ldh_sct_gv) - Processes records based on custom Groovy code.
* [HBase Lookup](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/HBaseLookup.html#concept_mnj_zhq_bw) - Performs key-value lookups in HBase to enrich records with data.
* [Hive Metadata](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/HiveMetadata.html#concept_rz5_nft_zv) - Works with the Hive Metastore destination as part of the [Drift Synchronization Solution for Hive](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Hive_Drift_Solution/HiveDriftSolution_title.html#concept_phk_bdf_2w).
* [HTTP Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/HTTPClient.html#concept_ghx_ypr_fw) - The HTTP Client processor sends requests to an HTTP resource URL and writes the results to a field.
* [HTTP Router](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/HTTPRouter.html#concept_ghx_ypr_fw) - Routes data to different streams based on the HTTP method and URL path in record header attributes.
* [JavaScript Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/JavaScript.html#concept_n2p_jgf_lr) - Processes records based on custom JavaScript code.
* [JDBC Lookup](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/JDBCLookup.html#concept_ysc_ccy_hw) - Performs lookups in a database table through a JDBC connection.
* [JDBC Tee](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/JDBCTee.html#concept_qbx_lcy_hw) - Writes data to a database table through a JDBC connection, and enriches records with data from generated database columns.
* [JSON Generator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/JSONGenerator.html#concept_jmg_hw1_h1b) - Serializes data from a field to a JSON-encoded string.
* [JSON Parser](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/JSONParser.html#concept_bs1_4t3_yq) - Parses a JSON object embedded in a string field.
* [Jython Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Jython.html#concept_a1h_lkf_lr) - Processes records based on custom Jython code.
* [Kudu Lookup](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/KuduLookup.html#concept_a1x_3wl_p1b) - Performs lookups in Kudu to enrich records with data.
* [Log Parser](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/LogParser.html#concept_ulm_qdq_fs) - Parses log data in a field based on the specified log format.
* [MLeap Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/MLeap.html#concept_wnr_wlv_gfb) - Uses a machine learning model stored in an MLeap bundle to generate evaluations, scoring, or classifications of data.
* [MongoDB Lookup](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/MongoDBLookup.html#concept_rrp_t4w_2fb) - Performs lookups in MongoDB to enrich records with data.
* [PMML Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/PMML.html#concept_r3s_3fv_gfb) - Uses a machine learning model stored in a PMML document to generate predictions or classifications of data.
* [PostgreSQL Metadata](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/PostgreSQLMetadata.html#concept_lcp_ssh_qcb) - Tracks structural changes in source data then creates and alters PostgreSQL tables as part of the [Drift Synchronization Solution for PostgreSQL](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/JDBC_DriftSolution/JDBC_DriftSyncSolution_title.html#concept_ljq_knr_4cb).
* [Redis Lookup](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/RedisLookup.html#concept_ng3_lpr_pv) - Performs key-value lookups in Redis to enrich records with data.
* [Salesforce Lookup](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/SalesforceLookup.html#concept_k23_3rk_yx) - Performs lookups in Salesforce to enrich records with data.
* [Schema Generator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/SchemaGenerator.html#concept_rfz_ks3_x1b) - Generates a schema for each record and writes the schema to a record header attribute.
* [Spark Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Spark.html#concept_cpx_1lm_zx) - Processes data based on a custom Spark application.
* [SQL Parser](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/SQLParser.html#concept_zh2_kfj_tdb) - Parses SQL queries in a string field.
* [Start Job](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/StartJob.html#concept_irv_l5r_2jb) - Starts a Control Hub job.
* [Start Pipeline](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/StartPipeline.html#concept_bbc_cxr_2jb) - Starts a Data Collector, Data Collector Edge, or Transformer pipeline.
* [Static Lookup](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/StaticLookup.html#concept_aqz_t4r_pv) - Performs key-value lookups in local memory.
* [Stream Selector](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/StreamSelector.html#concept_tqv_t5r_wq) - Routes data to different streams based on conditions.
* [TensorFlow Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/TensorFlow.html#concept_otg_csh_z2b) - Uses a TensorFlow machine learning model to generate predictions or classifications of data.
* [Value Replacer \(Deprecated\)](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/ValueReplacer.html#concept_o5k_dmf_zq) - Replaces existing nulls or specified values with constants or nulls.
* [Whole File Transformer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/WholeFileTransformer.html#concept_nwg_rx4_l2b) - Transforms Avro files to Parquet.
* [Windowing Aggregator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Aggregator.html#concept_ofb_svm_5bb) - Performs aggregations within a window of time, displays the results in Monitor mode, and writes the results to events when enabled. This processor does not update the records being evaluated.
* [XML Flattener](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/XMLFlattener.html#concept_ck4_255_sv) - Flattens XML data in a string field.
* [XML Parser](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/XMLParser.html#concept_dtt_q5q_k5) - Parses XML data in a string field.

### Edge Pipelines

In edge pipelines, you can use the following processors:

* [Expression Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/Expression.html#concept_zm2_pp3_wq) - Performs calculations on data. Can also add or modify record header attributes.
* [Field Remover](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/FieldRemover.html#concept_jdd_blr_wq) - Removes fields from a record.
* [JavaScript Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/JavaScript.html#concept_n2p_jgf_lr) - Processes records based on custom JavaScript code.
* [Stream Selector](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/StreamSelector.html#concept_tqv_t5r_wq) - Routes data to different streams based on conditions.
* [TensorFlow Evaluator](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Processors/TensorFlow.html#concept_otg_csh_z2b) - Uses a TensorFlow machine learning model to generate predictions or classifications of data.

### Development Processors

To help create or test pipelines, you can use the following development processors:

* Dev Identity
* Dev Random Error
* Dev Record Creator



