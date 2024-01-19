# Install External Libraries

Install external libraries to make them available to BigConnect Pipelines stages.

You can install external libraries for the following stages:

Before you use the following stages, install JDBC drivers for the implementation that you want to use:

* JDBC Multitable Consumer origin
* JDBC Query Consumer origin
* MySQL Binary Log origin
* Oracle CDC Client origin
* SAP HANA Query Consumer origin
* Teradata Consumer origin
* JDBC Lookup processor
* JDBC Tee processor
* SQL Parser processor, when using the database to resolve the schema
* JDBC Producer destination
* MemSQL Fast Loader destination
* JDBC Query executor

For example, to use the JDBC Query Consumer origin or the JDBC Producer destination with Oracle, install the Oracle JDBC drivers.\


* Before you use the Hadoop FS origin to read from non-HDFS systems, install all required file system application JAR files. See the file system documentation for details about the files to install.
* Before you use the Spark Evaluator processor, install the Spark application JAR file and any dependencies other than the streamsets-datacollector-api, streamsets-datacollector-spark-api, and spark-core libraries.
* You can install external Java libraries to call external Java code from the scripting processors: Groovy, Java, and Jython Evaluator.
* You can install the DataStax Enterprise (DSE) Java driver to configure the Cassandra destination to use DSE username and password authentication or Kerberos authentication.
* Before you use the Google Bigtable destination, install the BoringSSL library.
* Before you use the JMS Consumer origin or the JMS Producer destination, install the JMS drivers for the implementation that you are using.
* You can install the Impala JDBC driver for use with the Hive Query executor. For more information, see Installing the Impala Driver.

When installing an external library, you install it into the stage library that includes the stage. For example, to use an external Java library with the Groovy Evaluator processor, you install the Java library as an external library for the Groovy stage library.

To use an external library with multiple stage libraries, install the external library into each stage library associated with the stages. For example, if you want to use a MySQL JDBC driver with the JDBC Lookup processor and the MySQL Binary Log origin, you install the driver as an external library for the JDBC stage library and for the MySQL Binary Log stage library.

By default, external libraries are installed to the `$SDC_DIST/streamsets-libs-extras` directory. BigConnect Pipelines recommends configuring BigConnect Pipelines to use an external directory to enable use of the libraries after BigConnect Pipelines upgrades.

You can install external libraries any of the following ways:

* Install from Package Manager
* Install from stage properties
* Install manually
