# Local file connector

The local file connector allows querying the HTTP request log files stored on the local file system of each worker.

### Configuration <a href="#configuration" id="configuration"></a>

To configure the local file connector, create a catalog properties file under `etc/catalog` named, for example, `example.properties` with the following contents:

```
connector.name=localfile
```

### Configuration properties <a href="#configuration-properties" id="configuration-properties"></a>

| Property name                          | Description                                                                                |
| -------------------------------------- | ------------------------------------------------------------------------------------------ |
| `trino-logs.http-request-log.location` | Directory or file where HTTP request logs are written                                      |
| `trino-logs.http-request-log.pattern`  | If the log location is a directory, this glob is used to match file names in the directory |

### Local file connector schemas and tables <a href="#local-file-connector-schemas-and-tables" id="local-file-connector-schemas-and-tables"></a>

The local file connector provides a single schema named `logs`. You can see all the available tables by running `SHOW TABLES`:

```
SHOW TABLES FROM example.logs;
```

#### `http_request_log` <a href="#http-request-log" id="http-request-log"></a>

This table contains the HTTP request logs from each node on the cluster.

\
