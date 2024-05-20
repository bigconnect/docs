# Operational Commands

### COPY

Copy data from a CSV file into a SQL table.

```
COPY FROM '/path/to/local/file.csv'
INTO tableName (columnName, columnName, ...) FORMAT CSV [CHARSET '<charset-name>']
```

#### Parameters

* `'/path/to/local/file.csv'` - actual path to your CSV file.
* `tableName` - name of the table to which the data will be copied.
* `columnName` - name of a column corresponding with the columns in the CSV file.

#### Description

`COPY` allows you to copy the content of a file in the local file system to the server and apply its data to a SQL table. Internally, `COPY` reads the file content in a binary form into data packets, and sends those packets to the server. Then, the file content is parsed and executed in a streaming mode. Use this mode if you have data dumped to a file.

#### Example

`COPY` can be executed like so:

```
COPY FROM '/path/to/local/file.csv' INTO city (
  ID, Name, CountryCode, District, Population) FORMAT CSV
```

In the above command, substitute `/path/to/local/file.csv` with the actual path to your CSV file. For instance, you can use `city.csv` which is shipped with the latest Ignite. You can find it in your `{IGNITE_HOME}/examples/src/main/resources/sql/` directory.

### SET STREAMING

Stream data in bulk from a file into a SQL table.

```
SET STREAMING [OFF|ON];
```

#### Description

Using the `SET` command, you can stream data in bulk into a SQL table in your cluster. When streaming is enabled, the JDBC/ODBC driver will pack your commands in batches and send them to the server (Ignite cluster). On the server side, the batch is converted into a stream of cache update commands which are distributed asynchronously between server nodes. Performing this asynchronously increases peak throughput because at any given time all cluster nodes are busy with data loading.

#### Usage

To stream data into your cluster, prepare a file with the `SET STREAMING ON` command followed by `INSERT` commands for data that needs to be loaded.

```
SET STREAMING ON;

INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (1,'Kabul','AFG','Kabol',1780000);
INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (2,'Qandahar','AFG','Qandahar',237500);
INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (3,'Herat','AFG','Herat',186800);
INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (4,'Mazar-e-Sharif','AFG','Balkh',127800);
INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (5,'Amsterdam','NLD','Noord-Holland',731200);
-- More INSERT commands --
```

Note that before executing the above statements, you should have the tables created in the cluster. Run `CREATE TABLE` commands, or provide the commands as part of the file that is used for inserting data, before the `SET STREAMING ON` command, like so:

```
CREATE TABLE City (
  ID INT(11),
  Name CHAR(35),
  CountryCode CHAR(3),
  District CHAR(20),
  Population INT(11),
  PRIMARY KEY (ID, CountryCode)
) WITH "template=partitioned, backups=1, affinityKey=CountryCode, CACHE_NAME=City, KEY_TYPE=demo.model.CityKey, VALUE_TYPE=demo.model.City";

SET STREAMING ON;

INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (1,'Kabul','AFG','Kabol',1780000);
INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (2,'Qandahar','AFG','Qandahar',237500);
INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (3,'Herat','AFG','Herat',186800);
INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (4,'Mazar-e-Sharif','AFG','Balkh',127800);
INSERT INTO City(ID, Name, CountryCode, District, Population) VALUES (5,'Amsterdam','NLD','Noord-Holland',731200);
-- More INSERT commands --
```

#### Known Limitations

While streaming mode allows you to load data much faster than other data loading techniques mentioned in this guide, it has some limitations:

1. Only `INSERT` commands are allowed; any attempt to execute `SELECT` or any other DML or DDL command will cause an exception.
2. Due to streaming mode’s asynchronous nature, you cannot know update counts for every statement executed; all JDBC/ODBC commands returning update counts will return 0.

#### Example

As an example, you can use the sample world.sql file that is shipped with the latest Ignite distribution. It can be found in the `{IGNITE_HOME}/examples/sql/` directory. You can use the `run` command from tools/sqlline\[SQLLine, window=\_blank], as shown below:

```
!run /apache_ignite_version/examples/sql/world.sql
```

After executing the above command and **closing the JDBC connection**, all data will be loaded into the cluster and ready to be queried.

<figure><img src="https://ignite.apache.org/docs/2.16.0/images/set-streaming.png" alt=""><figcaption></figcaption></figure>

### KILL QUERY

The `KILL QUERY` command allows you to cancel a running query. When a query is cancelled with the `KILL` command, all parts of the query running on all other nodes are terminated as well.

SQLJMXUnixWindows

```
KILL QUERY {ASYNC} 'query_id'
```

#### Parameters

* `query_id` - can be retrived via the SQL\_QUERIES view.
* `ASYNC` - is an optional parameter that returns control immediately without waiting for the cancellation to finish.

### KILL TRANSACTION

The `KILL TRANSACTION` command allows you to cancel a running transaction.

SQLJMXUnixWindows

```
KILL TRANSACTION 'xid'
```

#### Parameters

* `xid` - the transaction id that can be retrived via the TRANSACTIONS view.

### KILL SCAN

The `KILL SCAN` command allows you to cancel a running scan query.

SQLJMXUnixWindows

```
KILL SCAN 'origin_node_id' 'cache_name' query_id
```

#### Parameters

* `origin_node_id`, `cache_name`, `query_id` - can be retrived via the SCAN\_QUERIES view.

#### Example

```
KILL SCAN '6fa749ee-7cf8-4635-be10-36a1c75267a7_54321' 'cache-name' 1
```

### KILL COMPUTE

The `KILL COMPUTE` command allows you to cancel a running compute.

SQLJMXUnixWindows

```
KILL COMPUTE 'session_id'
```

#### Parameters

* `session_id` - can be retrived via the TASKS or JOBS views.

### KILL CONTINUOUS

The `KILL CONTINUOUS` command allows you to cancel a running continuous query.

SQLJMXUnixWindows

```
KILL CONTINUOUS 'origin_node_id', 'routine_id'
```

#### Parameters

* `origin_node_id` and `routine_id` - can be retrived via the CONTINUOUS\_QUERIES view.

#### Example

```
KILL CONTINUOUS '6fa749ee-7cf8-4635-be10-36a1c75267a7_54321' '6fa749ee-7cf8-4635-be10-36a1c75267a7_12345'
```

### KILL SERVICE

The `KILL SERVICE` command allows you to cancel a running service.

SQLJMXUnixWindows

```
KILL SERVICE 'name'
```

#### Parameters

* `name` - corresponds to the name you selected for the service upon the deployment time. You can always find it with the SERVICES view.

### KILL CONSISTENCY repair/check operations

The `KILL CONSISTENCY` command allows you to cancel all running consistency repair/check operations.

UnixWindows

```
./control.sh --kill CONSISTENCY
```

### KILL CLIENT operations

The `KILL CLIENT` command allows you to cancel local client (thin/jdbc/odbc) connection.

UnixWindows

```
./control.sh --kill CLIENT connection_id [--node-id node_id]
```

#### Parameters

* `connection_id` - corresponds to the connection id of the client. Specify ALL to drop all connections. Note, connection\_id are local value. Value differs on nodes for the same client. You can always find parameter values with the CLIENT\_CONNECTIONS view.
* `node_id` - corresponds to the node\_id to drop connection from.
