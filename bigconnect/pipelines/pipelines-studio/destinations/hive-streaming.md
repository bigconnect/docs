# Hive Streaming

The Hive Streaming destination writes data to Hive tables stored in the ORC (Optimized Row Columnar) file format.&#x20;

Before you use the destination, verify that your Hadoop implementation supports Hive Streaming.

When configuring Hive Streaming, you specify the Hive metastore and a bucketed table stored in the ORC file format. You define the location of the Hive and Hadoop configuration files and optionally specify additional required properties. By default, the destination creates new partitions as needed.

Hive Streaming writes data to the table based on the matching field names. You can defining custom field mappings that override the default field mappings.

### Hive Properties and Configuration Files <a href="#ariaid-title2" id="ariaid-title2"></a>

You can configure Hive Streaming to use Hive and Hadoop configuration files and additional properties:Configuration filesThe following configuration files are required for the Hive Streaming destination:

* core-site.xml
* hdfs-site.xml
* hive-site.xml

To use the configuration files:

1. Store the files or a symlink to the files in the Data Pipelines resources directory or elsewhere in a path local to the Data Pipelines.
2. If the files are stored in the resources directory, specify a relative path to the files in the stage. If the files are stored outside of the resources directory, specify an absolute path to the files.Note: For a Cloudera Manager installation, Data Pipelines automatically creates a symlink to the files named `hive-conf`. Enter `hive-conf` for the location of the files in the stage.

Individual propertiesYou can configure individual Hive properties in the destination. To add a Hive property, specify the exact property name and the value. The destination does not validate the property names or values.Note: Individual properties override properties defined in the configuration files.
