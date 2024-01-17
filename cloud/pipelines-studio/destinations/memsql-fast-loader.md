# MemSQL Fast Loader

The MemSQL Fast Loader destination uses a JDBC connection to insert data into a MemSQL or MySQL database table with a LOAD statement.&#x20;

In pipelines that insert data into MemSQL or MySQL, you can use the MemSQL Fast Loader destination rather than the JDBC Producer destination for faster performance. In pipelines that update or delete data, however, you must use a JDBC Producer destination. The MemSQL Fast Loader destination does not process CDC records; the destination applies error handling to records with a CDC operation in a header attribute.

To configure a MemSQL Fast Loader destination, you specify connection information, table name, and optionally define field mappings and other properties that your driver requires. By default, the MemSQL Fast Loader destination writes data to the table based on the matching field names. You can override the default field mappings by defining specific mappings. To use a JDBC version older than 4.0, you can specify the driver class name and define a health check query.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before using the MemSQL Fast Loader destination, complete the following prerequisites:

* Install the MemSQL stage library.
* To use MemSQL Fast Loader with either a MySQL or MemSQL database, install the JDBC driver for MySQL.
* To use MemSQL Fast Loader with a MySQL database, you must enable local data loading in MySQL. See the MySQL topic, Security Issues with LOAD DATA LOCAL.

**Supported Versions**

The following table lists the versions of the MemSQL Enterprise stage library to use with specific Data Pipelines versions:

| Data Pipelines Version         | Supported Stage Library Version |
| ------------------------------ | ------------------------------- |
| Data Pipelines 3.8.x and later | MemSQL Enterprise Library 1.0.1 |

#### Installing the JDBC Driver for MemSQL Fast Loader <a href="#ariaid-title7" id="ariaid-title7"></a>

The MemSQL Fast Loader destination requires that you install the JDBC driver for MySQL. Install the JDBC driver for MySQL whether you are using MemSQL or MySQL. The destination cannot access a MemSQL or MySQL database until you install this driver.

1. Download the JDBC driver for MySQL.You can download the driver on the [Download Connector/J](https://dev.mysql.com/downloads/connector/j/) page on the MySQL website.
2. Install the driver as an external library for the MemSQL Enterprise stage library.

### &#x20;<a href="#ariaid-title8" id="ariaid-title8"></a>

\
