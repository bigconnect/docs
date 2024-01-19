# Cassandra

The Cassandra destination writes data to a Cassandra cluster.

When you configure the Cassandra destination, you define connection information and map incoming fields to columns in the Cassandra table. You also configure whether the destination writes each batch to Cassandra as a logged batch or an unlogged batch. You can disable batch writes and have the destination write records individually instead.

You configure whether the destination uses no authentication or username and password authentication to access the Cassandra cluster. If you install the DataStax Enterprise (DSE) Java driver, you can configure the destination to use DSE username and password authentication or Kerberos authentication.

You can also enable SSL/TLS for the connection.

### Batch Type <a href="#ariaid-title2" id="ariaid-title2"></a>

The Cassandra destination can write batches to a Cassandra cluster using one of the following batch types:

LoggedLogged batches written to Cassandra use the Cassandra distributed batch log and are atomic. This means that the destination can only write entire batches of records to Cassandra. If an error occurs with one or more records in a batch, the destination fails the entire batch. When a batch fails, all records are sent to the stage for error handling.UnloggedUnlogged batches written to Cassandra do not use the Cassandra distributed batch log and are nonatomic. This means that the destination can write partial batches of records to Cassandra. If an error occurs with one or more records in a batch, the destination sends only those failed records to the stage for error handling. The destination writes the remaining records in the batch to Cassandra.

By default, the destination uses the logged batch type.

For more information about the Cassandra distributed batch log, see the [Cassandra Query Language (CQL) documentation](https://docs.datastax.com/en/dse/6.7/cql/cql/cql\_reference/cql\_commands/cqlBatch.html).

### Authentication <a href="#ariaid-title3" id="ariaid-title3"></a>

Configure the Cassandra destination to use one of the following authentication providers to access the Cassandra cluster:

* None - Performs no authentication.
* Username/Password - Uses Cassandra username and password authentication.
* Username/Password (DSE) - Uses DataStax Enterprise username and password authentication. Requires that you install the DSE Java driver.
* Kerberos (DSE) - Uses Kerberos authentication. Requires that you install the DSE Java driver.

Before selecting one of the DSE authentication providers, install the DSE Java driver version 1.2.4 or later. For a compatibility matrix, see the [Cassandra documentation](https://docs.datastax.com/en/developer/java-driver/3.6/manual/native\_protocol/).&#x20;

#### Kerberos (DSE) Authentication <a href="#ariaid-title4" id="ariaid-title4"></a>

If you install the DSE Java driver, you can use Kerberos authentication to connect to a Cassandra cluster. When you use Kerberos authentication, Data Pipelines uses the Kerberos principal and keytab to connect to the cluster. By default, Data Pipelines uses the user account who started it to connect.

The Kerberos principal and keytab are defined in the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`. To use Kerberos authentication, configure all Kerberos properties in the Data Pipelines configuration file, install the DSE Java driver, and then enable Kerberos (DSE) authentication in the Cassandra destination.

### Cassandra Data Types <a href="#ariaid-title5" id="ariaid-title5"></a>

Due to Cassandra requirements, the data types of the incoming fields must match the data types of the corresponding Cassandra columns. When appropriate, use a Field Type Converter processor earlier in the pipeline to convert data types.

For details about the conversion of Java data types to Cassandra data types, see the Cassandra documentation.

The Cassandra destination supports the following Cassandra data types:

* ASCII
* Bigint
* Boolean
* Counter
* Decimal
* Double
* Float
* Int
* List
* Map
* Text
* Timestamp
* Timeuuid
* Uuid
* Varchar
* Varint

The following data types are not supported at this time:

* Blob
* Inet
* Set
