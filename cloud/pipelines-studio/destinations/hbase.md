# HBase

The HBase destination writes data to an HBase cluster. The destination can write data to an existing HBase table as text, binary data, or JSON strings. You can define the data format for each column written to HBase.

When you configure the HBase destination, you specify the HBase configuration properties, including the ZooKeeper Quorum, parent znode, and the existing table name. You specify the row key for the table, and then map fields from the pipeline to HBase columns.

When necessary, you can enable Kerberos authentication and specify an HBase user. You can also configure a time basis and add additional HBase configuration properties.

### Field Mappings <a href="#ariaid-title2" id="ariaid-title2"></a>

When you configure the HBase destination, you map fields from records to HBase columns.

You can map fields to columns in the following ways:

**Explicit field mappings**

By default, the HBase destination uses explicit field mappings. You select the fields from records to map to HBase columns. Specify the HBase columns using the following format: `<column-family>:<qualifier>`. You then define the storage type for the column in HBase.When you use explicit field mappings, you can configure the destination to ignore missing field paths. If the destination encounters a mapped field path that doesn’t exist in the record, the destination ignores the missing field path and writes the remaining fields in the record to HBase.

**Implicit field mappings**

When you configure the HBase destination to use implicit field mappings, the destination writes data based on the matching field names. You can use implicit field mappings when the field paths use the following format:

```
<column-family>:<qualifier>
```

For example, if a field path is "cf:a", the destination can implicitly map the field to the HBase table with the column family "cf" and the qualifier "a".

When you use implicit field mappings, you can configure the destination to ignore invalid columns. If the destination encounters a field path that cannot be mapped to a valid HBase column, the destination ignores the invalid column and writes the remaining fields in the record to HBase.

**Both implicit and explicit field mappings**

You can configure the destination to use implicit field mappings and then you can override the mappings by defining explicit mappings for specific fields.For example, a record might contain some field paths that use the `<column-family>:<qualifier>` format and other field paths that don’t use the required format. You can add explicit field mappings for the field paths that do not use the required format. Or, you can use explicit field mappings for fields that use the required format, but need to be written to a different column.

### Kerberos Authentication <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use Kerberos authentication to connect to HBase. When you use Kerberos authentication, Data Pipelines uses the Kerberos principal and keytab to connect to HBase. By default, Data Pipelines uses the user account who started it to connect.

The Kerberos principal and keytab are defined in the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`. To use Kerberos authentication, configure all Kerberos properties in the Data Pipelines configuration file.

### Using an HBase User <a href="#ariaid-title4" id="ariaid-title4"></a>

Data Pipelines can either use the currently logged in Data Pipelines user or a user configured in the destination to write to HBase.

A Data Pipelines configuration property can be set that requires using the currently logged in Data Pipelines user. When this property is not set, you can specify a user in the origin.&#x20;

Note that the destination uses a different user account to connect to HBase. By default, Data Pipelines uses the user account who started it to connect to external systems. When using Kerberos, Data Pipelines uses the Kerberos principal.

To configure a user in the destination to write to HBase, perform the following tasks:

1.  On HBase, configure the user as a proxy user and authorize the user to impersonate the HBase user.

    For more information, see the HBase documentation.
2. In the HBase destination, enter the HBase user name.

### Time Basis <a href="#ariaid-title5" id="ariaid-title5"></a>

The time basis determines the timestamp value added for each column written to HBase.

You can use the following times as the time basis:

**Processing Time**

When you use processing time as the time basis, the destination uses the Data Pipelines processing time as the timestamp value. The processing time is calculated once per batch.To use the processing time as the time basis, use the following expression: `${time:now()}`.

**Record Time**

When you use the time associated with a record as the time basis, you specify a Date or Datetime field in the record. The destination uses the field value as the timestamp value.To use a time associated with the record, use an expression that calls a field and resolves to a date or datetime value, such as `${record:value("/Timestamp")}`.

**System Time**

When you leave the Time Basis field empty, the destination uses the timestamp value automatically generated by HBase when the column is written to HBase.This is the default time basis.

### HDFS Properties and Configuration File <a href="#ariaid-title6" id="ariaid-title6"></a>

You can configure the HBase destination to use individual HDFS properties or HDFS configuration files:

HBase configuration fileYou can use the following HDFS configuration file with the HBase configuration file:

* hbase-site.xml

To use HDFS configuration files:

1. Store the files or a symlink to the files in the Data Pipelines resources directory.
2. In the HBase destination, specify the location of the files.Note: For a Cloudera Manager installation, Data Pipelines automatically creates a symlink to the files named `hbase-conf`. Enter `hbase-conf` for the location of the files in the HBase destination.

Individual propertiesYou can configure individual HBase properties in the HBase destination. To add an HBase property, you specify the exact property name and the value. The HBase destination does not validate the property names or values.Note: Individual properties override properties defined in the HBase configuration file.
