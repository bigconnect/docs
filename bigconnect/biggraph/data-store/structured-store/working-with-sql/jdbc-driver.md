# JDBC Driver

gnite is shipped with JDBC drivers that allow processing of distributed data using standard SQL statements like `SELECT`, `INSERT`, `UPDATE` or `DELETE` directly from the JDBC side.

Presently, there are two drivers supported by Ignite: the lightweight and easy to use JDBC Thin Driver described in this document and JDBC Client Driver that interacts with the cluster by means of a client node.

## JDBC Thin Driver

The JDBC Thin driver is a default, lightweight driver provided by Ignite. To start using the driver, just add `ignite-core-2.16.0.jar` to your application’s classpath.

The driver connects to one of the cluster nodes and forwards all the queries to it for final execution. The node handles the query distribution and the result’s aggregations. Then the result is sent back to the client application.

The JDBC connection string may be formatted with one of two patterns: `URL query` or `semicolon`:

Connection String Syntax

```java
// URL query pattern
jdbc:ignite:thin://<hostAndPortRange0>[,<hostAndPortRange1>]...[,<hostAndPortRangeN>][/schema][?<params>]

hostAndPortRange := host[:port_from[..port_to]]

params := param1=value1[&param2=value2]...[&paramN=valueN]

// Semicolon pattern
jdbc:ignite:thin://<hostAndPortRange0>[,<hostAndPortRange1>]...[,<hostAndPortRangeN>][;schema=<schema_name>][;param1=value1]...[;paramN=valueN]
```

* `host` is required and defines the host of the cluster node to connect to.
* `port_from` is the beginning of the port range to use to open the connection. 10800 is used by default if this parameter is omitted.
* `port_to` is optional. It is set to the `port_from` value by default if this parameter is omitted.
* `schema` is the schema name to access. PUBLIC is used by default. This name should correspond to the SQL ANSI-99 standard. Non-quoted identifiers are not case sensitive. Quoted identifiers are case sensitive. When semicolon format is used, the schema may be defined as a parameter with name schema.
* `<params>` are optional.

The name of the driver’s class is `org.apache.ignite.IgniteJdbcThinDriver`. For instance, this is how you can open a JDBC connection to the cluster node listening on IP address 192.168.0.50:

```java
// Register JDBC driver.
Class.forName("org.apache.ignite.IgniteJdbcThinDriver");

// Open the JDBC connection.
Connection conn = DriverManager.getConnection("jdbc:ignite:thin://127.0.0.1");
```

#### Connection String Examples

* `jdbc:ignite:thin://myHost` - connect to myHost on the port 10800 with all defaults.
* `jdbc:ignite:thin://myHost:11900` - connect to myHost on custom port 11900 with all defaults.
* `jdbc:ignite:thin://myHost:11900;user=ignite;password=ignite` - connect to myHost on custom port 11900 with user credentials for authentication.
* `jdbc:ignite:thin://myHost:11900;distributedJoins=true&autoCloseServerCursor=true` - connect to myHost on custom port 11900 with enabled distributed joins and autoCloseServerCursor optimization.
* `jdbc:ignite:thin://myHost:11900/myschema;` - connect to myHost on custom port 11900 and access to MYSCHEMA.
* `jdbc:ignite:thin://myHost:11900/"MySchema";lazy=false` - connect to myHost on custom port 11900 with disabled lazy query execution and access to MySchema (schema name is case sensitive).

#### Multiple Endpoints

You can enable automatic failover if a current connection is broken by setting multiple connection endpoints in the connection string. The JDBC Driver randomly picks an address from the list to connect to. If the connection fails, the JDBC Driver selects another address from the list until the connection is restored. The Driver stops reconnecting and throws an exception if all the endpoints are unreachable.

The example below shows how to pass three addresses via the connection string:

```java
// Register JDBC Driver.
Class.forName("org.apache.ignite.IgniteJdbcThinDriver");

// Open the JDBC connection passing several connection endpoints.
Connection conn = DriverManager
        .getConnection("jdbc:ignite:thin://192.168.0.50:101,192.188.5.40:101,192.168.10.230:101");
```

#### Partition Awareness

Partition awareness is a feature that makes the JDBC driver "aware" of the partition distribution in the cluster. It allows the driver to pick the nodes that own the data that is being queried and send the query directly to those nodes (if the addresses of the nodes are provided in the driver’s configuration). Partition awareness can increase average performance of queries that use the affinity key.

Without partition awareness, the JDBC driver connects to a single node, and all queries are executed through that node. If the data is hosted on a different node, the query has to be rerouted within the cluster, which adds an additional network hop. Partition awareness eliminates that hop by sending the query to the right node.

To make use of the partition awareness feature, provide the addresses of all the server nodes in the connection properties. The driver will route requests to the nodes that store the data requested by the query.

To enable partition awareness, add the `partitionAwareness=true` parameter to the connection string and provide the endpoints of multiple server nodes:

```java
Class.forName("org.apache.ignite.IgniteJdbcThinDriver");

Connection conn = DriverManager
        .getConnection("jdbc:ignite:thin://192.168.0.50,192.188.5.40,192.168.10.230?partitionAwareness=true");
```

#### Using SSL

You can configure the JDBC Thin Driver to use SSL to secure communication with the cluster. SSL must be configured both on the cluster side and in the JDBC Driver.

To enable SSL in the JDBC Driver, pass the `sslMode=require` parameter in the connection string and provide the key store and trust store parameters:

```java
Class.forName("org.apache.ignite.IgniteJdbcThinDriver");

String keyStore = "keystore/node.jks";
String keyStorePassword = "123456";

String trustStore = "keystore/trust.jks";
String trustStorePassword = "123456";

try (Connection conn = DriverManager.getConnection("jdbc:ignite:thin://127.0.0.1?sslMode=require"
        + "&sslClientCertificateKeyStoreUrl=" + keyStore + "&sslClientCertificateKeyStorePassword="
        + keyStorePassword + "&sslTrustCertificateKeyStoreUrl=" + trustStore
        + "&sslTrustCertificateKeyStorePassword=" + trustStorePassword)) {

    ResultSet rs = conn.createStatement().executeQuery("select 10");
    rs.next();
    System.out.println(rs.getInt(1));
} catch (Exception e) {
    e.printStackTrace();
}
```

## Ignite DataSource

The DataSource object is used as a deployed object that can be located by logical name via the JNDI naming service. JDBC Driver’s `org.apache.ignite.IgniteJdbcThinDataSource` implements a JDBC DataSource interface allowing you to utilize the DataSource interface instead.

In addition to generic DataSource properties, `IgniteJdbcThinDataSource` supports all the Ignite-specific properties that can be passed into a JDBC connection string. For instance, the `distributedJoins` property can be (re)set via the `IgniteJdbcThinDataSource#setDistributedJoins()` method.

Refer to the [JavaDocs](https://ignite.apache.org/releases/2.16.0/javadoc/org/apache/ignite/IgniteJdbcThinDataSource.html) for more details.

## Examples

To start processing the data located in the cluster, you need to create a JDBC Connection object via one of the methods below:

```java
// Open the JDBC connection via DriverManager.
Connection conn = DriverManager.getConnection("jdbc:ignite:thin://192.168.0.50");
```

or

```java
// Or open connection via DataSource.
IgniteJdbcThinDataSource ids = new IgniteJdbcThinDataSource();
ids.setUrl("jdbc:ignite:thin://127.0.0.1");
ids.setDistributedJoins(true);

Connection conn = ids.getConnection();
```

Then you can execute SQL SELECT queries as follows:

```java
// Query people with specific age using prepared statement.
PreparedStatement stmt = conn.prepareStatement("select name, age from Person where age = ?");

stmt.setInt(1, 30);

ResultSet rs = stmt.executeQuery();

while (rs.next()) {
    String name = rs.getString("name");
    int age = rs.getInt("age");
    // ...
}
```

You can also modify the data via DML statements.

#### INSERT

```java
// Insert a Person with a Long key.
PreparedStatement stmt = conn
        .prepareStatement("INSERT INTO Person(_key, name, age) VALUES(CAST(? as BIGINT), ?, ?)");

stmt.setInt(1, 1);
stmt.setString(2, "John Smith");
stmt.setInt(3, 25);

stmt.execute();
```

#### MERGE

```java
// Merge a Person with a Long key.
PreparedStatement stmt = conn
        .prepareStatement("MERGE INTO Person(_key, name, age) VALUES(CAST(? as BIGINT), ?, ?)");

stmt.setInt(1, 1);
stmt.setString(2, "John Smith");
stmt.setInt(3, 25);

stmt.executeUpdate();
```

#### UPDATE

```java
// Update a Person.
conn.createStatement().
  executeUpdate("UPDATE Person SET age = age + 1 WHERE age = 25");
```

#### DELETE

```java
conn.createStatement().execute("DELETE FROM Person WHERE age = 25");
```

## Streaming

JDBC Driver allows streaming data in bulk using the `SET` command.

## Error Codes

The JDBC drivers pass error codes in the `java.sql.SQLException` class, used to facilitate exception handling on the application side. To get an error code, use the `java.sql.SQLException.getSQLState()` method. It returns a string containing the ANSI SQLSTATE error code defined:

```java
PreparedStatement ps;

try {
    ps = conn.prepareStatement("INSERT INTO Person(id, name, age) values (1, 'John', 'unparseableString')");
} catch (SQLException e) {
    switch (e.getSQLState()) {
    case "0700B":
        System.out.println("Conversion failure");
        break;

    case "42000":
        System.out.println("Parsing error");
        break;

    default:
        System.out.println("Unprocessed error: " + e.getSQLState());
        break;
    }
}
```

The table below lists all the [ANSI SQLSTATE](https://en.wikipedia.org/wiki/SQLSTATE) error codes currently supported by Ignite. Note that the list may be extended in the future.

| Code  | Description                                                                                                                                                      |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0700B | Conversion failure (for example, a string expression cannot be parsed as a number or a date).                                                                    |
| 0700E | Invalid transaction isolation level.                                                                                                                             |
| 08001 | The driver failed to open a connection to the cluster.                                                                                                           |
| 08003 | The connection is in the closed state. Happened unexpectedly.                                                                                                    |
| 08004 | The connection was rejected by the cluster.                                                                                                                      |
| 08006 | I/O error during communication.                                                                                                                                  |
| 22004 | Null value not allowed.                                                                                                                                          |
| 22023 | Unsupported parameter type.                                                                                                                                      |
| 23000 | Data integrity constraint violation.                                                                                                                             |
| 24000 | Invalid result set state.                                                                                                                                        |
| 0A000 | Requested operation is not supported.                                                                                                                            |
| 40001 | Concurrent update conflict.                                                                                                                                      |
| 42000 | Query parsing exception.                                                                                                                                         |
| 50000 | Internal error. The code is not defined by ANSI and refers to an Ignite specific error. Refer to the `java.sql.SQLException` error message for more information. |
