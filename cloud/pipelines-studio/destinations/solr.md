# Solr

The Solr destination writes data to a Solr node or cluster.&#x20;

When you configure the Solr destination, you configure connection information for the node or cluster. You can configure the destination to index records individually or in batches.

You configure how the destination maps fields in the record to fields in Solr. You can have the destination automatically map fields in the record to fields in the Solr schema based on name. Alternatively, you can map specific incoming fields to Solr fields. You also specify the action to take when the record is missing fields from the schema or mapped fields, and you can configure the destination to ignore missing optional fields.

You can specify whether the destination validates the connection to Solr. And you can configure write properties that determine whether the destination waits for Solr to complete all processing before continuing to write additional data.

When necessary, you can enable the destination to use Kerberos authentication.

### Index Mode <a href="#ariaid-title2" id="ariaid-title2"></a>

The index mode determines how the Solr destination indexes records when writing to Solr. Index mode also determines how the destination handles errors.

You can use the following index modes:

**Record**

The destination indexes one record at a time, and then passes the record to Solr.If an error occurs, the destination passes the record to the stage for error handling.

**Batch**

The destination indexes a batch of records at one time, and then passes the batch to Solr.If an error occurs, the destination rolls back any records that were indexed and passes the entire batch to the stage for error handling.

### Kerberos Authentication <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use Kerberos authentication to connect to a Solr node or cluster. When you use Kerberos authentication, Data Pipelines uses the Kerberos principal and keytab to connect to Solr.

The Kerberos principal and keytab are defined in the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`. To use Kerberos authentication, configure all Kerberos properties in the Data Pipelines configuration file, and then enable Kerberos in the Solr destination.
