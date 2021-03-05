# JDBC Driver

DataCharm can be accessed from Java using the Presto JDBC driver. Download [presto-jdbc-339.jar](https://repo1.maven.org/maven2/io/prestosql/presto-jdbc/339/presto-jdbc-339.jar) and add it to the classpath of your Java application.

The driver is also available from Maven Central:

```text
<dependency>
    <groupId>io.prestosql</groupId>
    <artifactId>presto-jdbc</artifactId>
    <version>339</version>
</dependency>
```

### Requirements <a id="requirements"></a>

The JDBC driver is compatible with Java 8, and can be used with applications running on Java virtual machines version 8 and higher.

### Driver Name <a id="driver-name"></a>

The driver class name is `io.prestosql.jdbc.PrestoDriver`. Most users do not need this information as drivers are loaded automatically.

### Connecting <a id="connecting"></a>

The following JDBC URL formats are supported:

```text
jdbc:presto://host:port
jdbc:presto://host:port/catalog
jdbc:presto://host:port/catalog/schema
```

For example, use the following URL to connect to Presto running on `example.net` port `8080` with the catalog `hive` and the schema `sales`:

```text
jdbc:presto://dsql-<TENANT_ID>.cloud.bigconnect.io:443/bigconnect/public
```

The above URL can be used as follows to create a connection:

```text
String url = "jdbc:presto://dsql-<TENANT_ID>.cloud.bigconnect.io:443/bigconnect/public";
Connection connection = DriverManager.getConnection(url, "<TENANT_ID>", "<PASSWORD>");
```

### Connection Parameters <a id="connection-parameters"></a>

The driver supports various parameters that may be set as URL parameters, or as properties passed to `DriverManager`. Both of the following examples are equivalent:

```text
// URL parameters
String url = "jdbc:presto://dsql-<TENANT_ID>.cloud.bigconnect.io:443/bigconnect/public";
Properties properties = new Properties();
properties.setProperty("user", "test");
properties.setProperty("password", "secret");
properties.setProperty("SSL", "true");
Connection connection = DriverManager.getConnection(url, properties);

// properties
String url = "jdbc:presto://dsql-<TENANT_ID>.cloud.bigconnect.io:443/bigconnect/public?user=test&password=secret&SSL=true";
Connection connection = DriverManager.getConnection(url);
```

These methods may be mixed; some parameters may be specified in the URL, while others are specified using properties. However, the same parameter may not be specified using both methods.

