# REST API

Ignite provides an HTTP REST client that can communicate with the cluster over HTTP and HTTPS protocols using the REST approach. REST APIs can be used to perform different operations like read/write from/to cache, execute tasks, get various metrics, and more.

Internally, Ignite uses Jetty to provide HTTP server features. See [Configuration](https://ignite.apache.org/docs/latest/restapi#configuration) section below for details on how to configure jetty.

### Getting Started

To enable HTTP connectivity, make sure that the `ignite-rest-http` module is enabled. If you use the binary distribution, copy the `ignite-rest-http` module from `IGNITE_HOME/libs/optional/` to the `IGNITE_HOME/libs` folder.&#x20;

Explicit configuration is not required; the connector starts up automatically and listens on port `8080`. You can check if it works with curl:

```
curl 'http://localhost:8080/ignite?cmd=version'
```

Request parameters may be provided as either a part of URL or in a form data:

```
curl 'http://localhost:8080/ignite?cmd=put&cacheName=myCache' -X POST -H 'Content-Type: application/x-www-form-urlencoded' -d 'key=testKey&val=testValue'
```

#### Configuration

You can change HTTP server parameters as follows:

```xml
<bean class="org.apache.ignite.configuration.IgniteConfiguration" id="ignite.cfg">
    <property name="connectorConfiguration">
        <bean class="org.apache.ignite.configuration.ConnectorConfiguration">
            <property name="jettyPath" value="jetty.xml"/>
        </bean>
    </property>
</bean>
```

The following table describes the properties of `ConnectorConfiguration` that are related to the http server:

| Parameter Name              | Description                                                                                                                                                                                                                                                                                                                                                                                                                            | Optional | Default Value |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------- |
| `setSecretKey(String)`      | Defines secret key used for client authentication. When provided, client request must contain `HTTP header X-Signature` with the string "\[1]:\[2]", where \[1] is timestamp in milliseconds and \[2] is the Base64 encoded SHA1 hash of the secret key.                                                                                                                                                                               | Yes      | `null`        |
| `setPortRange(int)`         | Port range for Jetty server. If the port provided in Jetty configuration or `IGNITE_JETTY_PORT` system property is already in use, Ignite iteratively increments port by 1 and tries to bind once again until provided port range is exceeded.                                                                                                                                                                                         | Yes      | `100`         |
| `setJettyPath(String)`      | Path to Jetty configuration file. Should be either absolute or relative to `IGNITE_HOME`. If the path is not set, Ignite starts a Jetty server with a simple HTTP connector. This connector uses `IGNITE_JETTY_HOST` and `IGNITE_JETTY_PORT` system properties as `host` and `port` respectively. If `IGNITE_JETTY_HOST` is not provided, `localhost` is used as default. If `IGNITE_JETTY_PORT` is not provided, port `8080` is used. | Yes      | `null`        |
| `setMessageInterceptor(…​)` | The interceptor transforms all objects exchanged via REST protocol. For example, if you use custom serialisation on client you can write an interceptor to transform binary representations received from the client to Java objects and later access them from Java code directly.                                                                                                                                                    | Yes      | `null`        |

**Example Jetty XML Configuration**

Path to this configuration should be set to `ConnectorConfiguration.setJettyPath(String)` as explained above.

```xml
<?xml version="1.0"?>
<!DOCTYPE Configure PUBLIC "-//Jetty//Configure//EN" "http://www.eclipse.org/jetty/configure.dtd">
<Configure id="Server" class="org.eclipse.jetty.server.Server">
    <Arg name="threadpool">
        <!-- Default queued blocking thread pool -->
        <New class="org.eclipse.jetty.util.thread.QueuedThreadPool">
            <Set name="minThreads">20</Set>
            <Set name="maxThreads">200</Set>
        </New>
    </Arg>
    <New id="httpCfg" class="org.eclipse.jetty.server.HttpConfiguration">
        <Set name="secureScheme">https</Set>
        <Set name="securePort">8443</Set>
        <Set name="sendServerVersion">true</Set>
        <Set name="sendDateHeader">true</Set>
    </New>
    <Call name="addConnector">
        <Arg>
            <New class="org.eclipse.jetty.server.ServerConnector">
                <Arg name="server"><Ref refid="Server"/></Arg>
                <Arg name="factories">
                    <Array type="org.eclipse.jetty.server.ConnectionFactory">
                        <Item>
                            <New class="org.eclipse.jetty.server.HttpConnectionFactory">
                                <Ref refid="httpCfg"/>
                            </New>
                        </Item>
                    </Array>
                </Arg>
                <Set name="host">
                  <SystemProperty name="IGNITE_JETTY_HOST" default="localhost"/>
                </Set>
                <Set name="port">
                  <SystemProperty name="IGNITE_JETTY_PORT" default="8080"/>
                </Set>
                <Set name="idleTimeout">30000</Set>
                <Set name="reuseAddress">true</Set>
            </New>
        </Arg>
    </Call>
    <Set name="handler">
        <New id="Handlers" class="org.eclipse.jetty.server.handler.HandlerCollection">
            <Set name="handlers">
                <Array type="org.eclipse.jetty.server.Handler">
                    <Item>
                        <New id="Contexts" class="org.eclipse.jetty.server.handler.ContextHandlerCollection"/>
                    </Item>
                </Array>
            </Set>
        </New>
    </Set>
    <Set name="stopAtShutdown">false</Set>
</Configure>
```

#### Security

When authentication is configured in the cluster, all applications that use REST API request authentication by providing security credentials. The authentication request returns a session token that can be used with any command within that session.

There are two ways to request authorization:

1.  Use the authenticate command with `ignite.login=[user]&ignite.password=[password]` parameters.

    ```
    https://[host]:[port]/ignite?cmd=authenticate&ignite.login=[user]&ignite.password=[password]
    ```
2.  Use any REST command with `ignite.login=[user]&ignite.password=[password]` parameters in the path of your connection string. In our example below, we use the `version` command:

    ```
    http://[host]:[port]/ignite?cmd=version&ignite.login=[user]&ignite.password=[password]
    ```

    In both examples above, replace `[host]`, `[port]`, `[user]`, and `[password]` with actual values.

Executing any one of the above strings in a browser returns a response with a session token which looks like this:

```
{"successStatus":0,"error":null,"sessionToken":"EF6013FF590348CE91DEAE9870183BEF","response":true}
```

Once you obtain the session token, use the sessionToken parameter with your connection string as shown in the example below:

```
http://[host]:[port]/ignite?cmd=top&sessionToken=[sessionToken]
```

In the above connection string, replace `[host]`, `[port]`, and `[sessionToken]` with actual values.

### Data Types

By default, the REST API exchanges query parameters in the `String` format. The cluster works with the parameters as with `String` objects.

If a type of a parameter is different from `String`, you can use the `keyType` or `valueType` to specify the real type of the argument. The REST API supports both Java Types and Custom Types.

#### Java Types

| REST KeyType/ValueType | Corresponding Java Type                                                                                                                                                                                                                                                                                              |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `boolean`              | `java.lang.Boolean`                                                                                                                                                                                                                                                                                                  |
| `byte`                 | `java.lang.Byte`                                                                                                                                                                                                                                                                                                     |
| `short`                | `java.lang.Short`                                                                                                                                                                                                                                                                                                    |
| `integer`              | `java.lang.Integer`                                                                                                                                                                                                                                                                                                  |
| `long`                 | `java.lang.Long`                                                                                                                                                                                                                                                                                                     |
| `float`                | `java.lang.Float`                                                                                                                                                                                                                                                                                                    |
| `double`               | `java.lang.Double`                                                                                                                                                                                                                                                                                                   |
| `date`                 | <p><code>java.sql.Date</code></p><p>The date value should be in the format as specified in the <code>valueOf(String)</code> method in the <a href="https://docs.oracle.com/javase/8/docs/api/java/sql/Date.html#valueOf-java.lang.String-">Java documentation</a></p><p>Example: 2018-01-01</p>                      |
| `time`                 | <p><code>java.sql.Time</code></p><p>The time value should be in the format as specified in the <code>valueOf(String)</code> method in the <a href="https://docs.oracle.com/javase/8/docs/api/java/sql/Date.html#valueOf-java.lang.String-">Java documentation</a></p><p>Example: 01:01:01</p>                        |
| `timestamp`            | <p><code>java.sql.Timestamp</code></p><p>The timestamp value should be in the format as specified in the <code>valueOf(String)</code> method in the <a href="https://docs.oracle.com/javase/8/docs/api/java/sql/Date.html#valueOf-java.lang.String-">Java documentation</a></p><p>Example: 2018-02-18%2001:01:01</p> |
| `uuid`                 | `java.util.UUID`                                                                                                                                                                                                                                                                                                     |
| `IgniteUuid`           | `org.apache.ignite.lang.IgniteUuid`                                                                                                                                                                                                                                                                                  |

The following example shows a `put` command with `keyType=int` and `valueType=date`:

```
http://[host]:[port]/ignite?cmd=put&key=1&val=2018-01-01&cacheName=myCache&keyType=int&valueType=date
```

Similarly, the `get` command with `keyType=int` and `valueType=date` would be:

```
http://[host]:[port]/ignite?cmd=get&key=1&cacheName=myCache&keyType=int&valueType=date
```

#### Custom Types

The JSON format is used to exchange complex custom objects via the Ignite REST protocol.

For example, let’s assume you have a `Person` class, and below is the JSON representation of an object instance that you need to send to the cluster:

```
 {
  "uid": "7e51118b",
  "name": "John Doe",
  "orgId": 5678901,
  "married": false,
  "salary": 156.1
 }
```

Next, you use this REST request to put the object in the cluster by setting the `valueType` parameter to `Person` and the `val` parameter to the value of the JSON object:

```
http://[host]:[port]/ignite?cacheName=testCache&cmd=put&keyType=int&key=1&valueType=Person
&val=%7B%0A+++++%22uid%22%3A+%227e51118b%22%2C%0A+++++%22name%22%3A+%22John+Doe%22%2C%0A+++++%22orgId%22%3A+5678901%2C%0A+++++%22married%22%3A+false%2C%0A+++++%22salary%22%3A+156.1%0A++%7D&
```

Once a server receives the request, it converts the object from the JSON into the internal [binary object](https://ignite.apache.org/docs/data-modeling/data-modeling#binary-object-format) format following the conversion procedure below:

* If the `Person` class exists and available on the server’s classpath, the JSON object is resolved to an instance of the `Person` class.
*   If the `Person` class is not available on the server’s classpath, but there is a `QueryEntity` object that defines the `Person`, then the JSON object is resolved to a binary object of that `Person` type:

    | Query entity                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | <pre><code>&#x3C;bean class="org.apache.ignite.cache.QueryEntity">
    &#x3C;property name="keyType" value="java.lang.Integer"/>
    &#x3C;property name="valueType" value="Person"/>
    &#x3C;property name="fields">
    &#x3C;map>
    &#x3C;entry key="uid"     value="java.util.UUID"/>
    &#x3C;entry key="name"    value="java.lang.String"/>
    &#x3C;entry key="orgId"   value="java.lang.Long"/>
    &#x3C;entry key="married" value="java.lang.Boolean"/>
    &#x3C;entry key="salary"  value="java.lang.Float"/>
    &#x3C;/map>
    &#x3C;/property>
    &#x3C;/bean>
    </code></pre> |

    | Binary Object (Person)                                                                                                                                                     |
    | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | <pre><code>"uid": "7e51118b",  // UUID
    "name": "John Doe", // string
    "orgId": 5678901,   // long
    "married": false,   // boolean
    "salary": 156.1     // float
    </code></pre> |
*   Otherwise, the JSON object’s field types are resolved following the regular JSON convention:

    ```
    "uid": "7e51118b",   // string
    "name": "John Doe",  // string
    "orgId": 5678901,    // int
    "married": false,    // boolean
    "salary": 156.1      // double
    ```

The same conversion rules apply when you have a custom key type set via the `keyType` parameter of the Ignite REST protocol.

### Returned Value

The HTTP REST request returns a JSON object which has a similar structure for each command:

| Field            | Type         | Description                                                                                                                                                                                                                                                        | Example                                |
| ---------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- |
| `affinityNodeId` | `string`     | Affinity node ID.                                                                                                                                                                                                                                                  | `2bd7b049-3fa0-4c44-9a6d-b5c7a597ce37` |
| `error`          | `string`     | This field contains description of error if server could not handle the request.                                                                                                                                                                                   | Specifically for each command.         |
| `sessionToken`   | `string`     | When authentication is enabled on the server, this field contains a session token that can be used with any command within that session. If authentication is off, this field contains `null`. When authentication is enabled - `EF6013FF590348CE91DEAE9870183BEF` | Otherwise, `null`.                     |
| `response`       | `jsonObject` | This field contains the result of the command.                                                                                                                                                                                                                     | Specifically for each command.         |
| `successStatus`  | `integer`    | <p>Exit status code. It might have the following values:</p><p><code>success = 0</code></p><p><code>failed = 1</code></p><p><code>authorization failed = 2</code></p><p><code>security check failed = 3</code></p>                                                 | `0`                                    |

### REST API Reference

#### Version

Returns the Ignite version.

**Request:**

```
http://host:port/ignite?cmd=version
```

**Response:**

```
{
  "error": "",
  "response": "1.0.0",
  "successStatus": 0
}
```

#### Cluster State

Returns the current state of the cluster.

**Request:**

```
http://host:port/ignite?cmd=state
```

**Response:**

Returns `true` if the cluster is active. Returns `false` if the cluster in inactive.

```
{
  "successStatus":0,
  "error":null,
  "sessionToken":null,
  "response": "ACTIVE_READ_ONLY"
}
```

#### Change Cluster State

The `setstate` command changes the cluster state.

**Request:**

```
http://host:port/ignite?cmd=setstate&state={new_state}
```

| Parameter | Type                                                                                                                                                                                                                                                                                                                                  | Description                                                                                                                                                                                                         |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `state`   | String                                                                                                                                                                                                                                                                                                                                | <p>New cluster state. One of the values:</p><ul><li><code>ACTIVE</code>: active state,</li><li><code>ACTIVE_READ_ONLY</code>: read only state,</li><li><code>INACTIVE</code>: the cluster is deactivated.</li></ul> |
|           | Deactivation deallocates all memory resources, including your application data, on all cluster nodes and disables public cluster API. If you have in-memory caches that are not backed up by a persistent storage, you will lose the data and will have to repopulate these caches. The non-persistent system caches are cleared too. |                                                                                                                                                                                                                     |

**Response:**

```
{
  "successStatus":0,
  "error":null,
  "sessionToken":null,
  "response":"setstate done"
}
```

#### Increment

Adds and gets current value of given atomic long.

**Request:**

```
http://host:port/ignite?cmd=incr&cacheName={cacheName}&key={incrKey}&init={initialValue}&delta={delta}
```

| Parameter   | Type   | Optional | Description                                         | Example          |
| ----------- | ------ | -------- | --------------------------------------------------- | ---------------- |
| `cacheName` | string | Yes      | Cache name. If not provided, default cache is used. | partitionedCache |
| `key`       | string |          | The name of atomic long.                            | counter          |
| `init`      | long   | Yes      | Initial value.                                      | 15               |
| `delta`     | long   |          | Number to be added.                                 | 42               |

**Response:**

The response contains the value after the operation.

```
{
  "affinityNodeId": "e05839d5-6648-43e7-a23b-78d7db9390d5",
  "error": "",
  "response": 42,
  "successStatus": 0
}
```

#### Decrement

Subtracts and gets current value of given atomic long.

**Request:**

```
http://host:port/ignite?cmd=decr&cacheName={cacheName}&key={key}&init={init_value}&delta={delta}
```

| Parameter   | Type   | Optional | Description                                                         | Example          |
| ----------- | ------ | -------- | ------------------------------------------------------------------- | ---------------- |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache ("default") is used. | partitionedCache |
| `key`       | string |          | The name of atomic long.                                            | counter          |
| `init`      | long   | Yes      | Initial value.                                                      | `15`             |
| `delta`     | long   |          | Number to be subtracted.                                            | `42`             |

**Response:**

The response contains the value after the operation.

```
{
  "affinityNodeId": "e05839d5-6648-43e7-a23b-78d7db9390d5",
  "error": "",
  "response": -42,
  "successStatus": 0
}
```

#### Cache Metrics

Shows metrics for a cache.

**Request:**

```
http://host:port/ignite?cmd=cache&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "",
  "error": "",
  "response": {
    "hits": 0,
    "misses": 0,
    "reads": 0,
    "writes": 2
  },
  "successStatus": 0
}
```

| Field      | Type       | Description                                                                      | Example                                                                                                                                 |
| ---------- | ---------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `response` | jsonObject | The JSON object contains cache metrics such as create time, count reads and etc. | `{ "createTime": 1415179251551, "hits": 0, "misses": 0, "readTime":1415179251551, "reads": 0,"writeTime": 1415179252198, "writes": 2 }` |

#### Cache Size

Gets the number of all entries cached across all nodes.

**Request:**

```
http://host:port/ignite?cmd=size&cacheName={cacheName}
```

| Parameter   | Type   | Optional | Description                                             | Example          |
| ----------- | ------ | -------- | ------------------------------------------------------- | ---------------- |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache |

**Response:**

```
{
  "affinityNodeId": "",
  "error": "",
  "response": 1,
  "successStatus": 0
}
```

| Field      | Type   | Description                                    | Example |
| ---------- | ------ | ---------------------------------------------- | ------- |
| `response` | number | Number of all entries cached across all nodes. | 5       |

#### Cache Metadata

Gets metadata for a cache.

**Request:**

```
http://host:port/ignite?cmd=metadata&cacheName={cacheName}
```

| Parameter   | Type   | Optional | Description                                                            | Example          |
| ----------- | ------ | -------- | ---------------------------------------------------------------------- | ---------------- |
| `cacheName` | String | Yes      | Cache name. If not provided, metadata for all user caches is returned. | partitionedCache |

**Response:**

```
{
  "error": "",
  "response": {
    "cacheName": "partitionedCache",
    "types": [
      "Person"
    ],
    "keyClasses": {
      "Person": "java.lang.Integer"
    },
    "valClasses": {
      "Person": "org.apache.ignite.Person"
    },
    "fields": {
      "Person": {
        "_KEY": "java.lang.Integer",
        "_VAL": "org.apache.ignite.Person",
        "ID": "java.lang.Integer",
        "FIRSTNAME": "java.lang.String",
        "LASTNAME": "java.lang.String",
        "SALARY": "double"
      }
    },
    "indexes": {
      "Person": [
        {
          "name": "ID_IDX",
          "fields": [
            "id"
          ],
          "descendings": [],
          "unique": false
        },
        {
          "name": "SALARY_IDX",
          "fields": [
            "salary"
          ],
          "descendings": [],
          "unique": false
        }
      ]
    }
  },
  "sessionToken": "",
  "successStatus": 0
}
```

#### Compare-And-Swap

Stores a given key-value pair in a cache only if the previous value is equal to the expected value passed in.

**Request:**

```
http://host:port/ignite?cmd=cas&key=casKey&val=newValue&val2=expectedValue&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key to store in cache.                                  | name                                 |
| `val`       | string |          | Value associated with the given key.                    | Jack                                 |
| `val2`      | string |          | Expected value.                                         | Bob                                  |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

The response returns `true` if the value was replaced, `false` otherwise.

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

#### Append

Appends a line for value which is associated with key.

**Request:**

```
http://host:port/ignite?cmd=append&key={appendKey}&val={_suffix}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key to store in cache.                                  | name                                 |
| `val`       | string |          | Value to be appended to the current value.              | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                    | Example |
| ---------- | ------- | ---------------------------------------------- | ------- |
| `response` | boolean | `true` if replace happened, `false` otherwise. | true    |

#### Prepend

Adds prefix to the value that is associated with a given key.

**Request:**

```
http://host:port/ignite?cmd=prepend&key={key}&val={value}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | myCache                              |
| `key`       | string |          | Key to store in cache.                                  | name                                 |
| `val`       | string |          | The string to be prepended to the current value.        | Name\_                               |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                    | Example |
| ---------- | ------- | ---------------------------------------------- | ------- |
| `response` | boolean | `true` if replace happened, `false` otherwise. | true    |

#### Replace

Stores a given key-value pair in a cache if the cache already contains the key.

**Request:**

```
http://host:port/ignite?cmd=rep&key=repKey&val=newValue&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                                                                                                                                                                                | Example                              |
| ----------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used.                                                                                                                                                    | partitionedCache                     |
| `key`       | string |          | Key to store in cache.                                                                                                                                                                                     | name                                 |
| `val`       | string |          | Value associated with the given key.                                                                                                                                                                       | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.                                                                                                                                                          | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |
| `exp`       | long   | Yes      | Expiration time in milliseconds for the entry. When the parameter is set, the operation is executed with [ModifiedExpiryPolicy](https://ignite.apache.org/docs/latest/configuring-caches/expiry-policies). | 60000                                |

**Response:**

The response contains `true` if the value was replaced, `false` otherwise.

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

#### Get

Retrieves the value mapped to a specified key from a cache.

**Request:**

```
http://host:port/ignite?cmd=get&key={getKey}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type               | Optional | Description                                             | Example                              |
| ----------- | ------------------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string             | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string             |          | Key whose associated value is to be returned.           | testKey                              |
| `keyType`   | Java built-in type | Yes      | See Data Types for more details.                        |                                      |
| `destId`    | string             | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

#### Get All

Retrieves values mapped to the specified keys from a given cache.

**Request:**

```
http://host:port/ignite?cmd=getall&k1={getKey1}&k2={getKey2}&k3={getKey3}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `k1…​kN`    | string |          | Keys whose associated values are to be returned.        | key1, key2, …​, keyN                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "",
  "error": "",
  "response": {
    "key1": "value1",
    "key2": "value2"
  },
  "successStatus": 0
}
```

|   | <h4 id="get-output-as-array">Get output as array</h4><p>To obtain the output as an array, use the <code>IGNITE_REST_GETALL_AS_ARRAY=true</code> system property. Once the property is set, the <code>getall</code> command provides the response in the following format:</p><p><code>{“successStatus”:0,“affinityNodeId”:null,“error”:null,“sessionToken”:null,“response”:[{“key”:“key1”,“value”:“value1”},{“key”:“key2”,“value”:“value2”}]}</code></p> |
| - | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Get and Remove

Removes the given key mapping from cache and returns the previous value.

**Request:**

```
http://host:port/ignite?cmd=getrmv&cacheName={cacheName}&destId={nodeId}&key={key}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key whose mapping is to be removed from the cache.      | name                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": value,
  "successStatus": 0
}
```

| Field      | Type       | Description        | Example           |
| ---------- | ---------- | ------------------ | ----------------- |
| `response` | jsonObject | Value for the key. | `{"name": "bob"}` |

#### Get and Put

Stores a given key-value pair in a cache and returns the existing value if there is one.

**Request:**

```
http://host:port/ignite?cmd=getput&key=getKey&val=newVal&cacheName={cacheName}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key to be associated with value.                        | name                                 |
| `val`       | string |          | Value to be associated with key.                        | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

The response contains the previous value for the key.

```
{
  "affinityNodeId": "2bd7b049-3fa0-4c44-9a6d-b5c7a597ce37",
  "error": "",
  "response": {"name": "bob"},
  "successStatus": 0
}
```

#### Get and Put If Absent

Stores given key-value pair in cache only if cache had no previous mapping for it. If cache previously contained value for the given key, then this value is returned.

**Request:**

```
http://host:port/ignite?cmd=getputifabs&key=getKey&val=newVal&cacheName={cacheName}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key to be associated with value.                        | name                                 |
| `val`       | string |          | Value to be associated with key.                        | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "2bd7b049-3fa0-4c44-9a6d-b5c7a597ce37",
  "error": "",
  "response": "value",
  "successStatus": 0
}
```

| Field      | Type       | Description                       | Example           |
| ---------- | ---------- | --------------------------------- | ----------------- |
| `response` | jsonObject | Previous value for the given key. | `{"name": "bob"}` |

#### Get and Replace

Stores a given key-value pair in cache only if there is a previous mapping for it.

**Request:**

```
http://host:port/ignite?cmd=getrep&key={key}&val={val}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key to store in cache.                                  | name                                 |
| `val`       | string |          | Value associated with the given key.                    | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

The response contains the previous value associated with the specified key.

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": oldValue,
  "successStatus": 0
}
```

| Field      | Type       | Description                                           | Example           |
| ---------- | ---------- | ----------------------------------------------------- | ----------------- |
| `response` | jsonObject | The previous value associated with the specified key. | `{"name": "Bob"}` |

#### Replace Value

Replaces the entry for a key only if currently mapped to a given value.

**Request:**

```
http://host:port/ignite?cmd=repval&key={key}&val={newValue}&val2={oldVal}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key to store in cache.                                  | name                                 |
| `val`       | string |          | Value associated with the given key.                    | Jack                                 |
| `val2`      | string |          | Value expected to be associated with the specified key. | oldValue                             |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                    | Example |
| ---------- | ------- | ---------------------------------------------- | ------- |
| `response` | boolean | `true` if replace happened, `false` otherwise. | true    |

#### Remove

Removes the given key mapping from cache.

**Request:**

```
http://host:port/ignite?cmd=rmv&key={rmvKey}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                              | Example                              |
| ----------- | ------ | -------- | -------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used.  | partitionedCache                     |
| `key`       | string |          | Key - for which the mapping is to be removed from cache. | name                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.        | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                    | Example |
| ---------- | ------- | ---------------------------------------------- | ------- |
| `response` | boolean | `true` if replace happened, `false` otherwise. | true    |

#### Remove All

Removes given key mappings from a cache.

**Request:**

```
http://host:port/ignite?cmd=rmvall&k1={rmKey1}&k2={rmKey2}&k3={rmKey3}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `k1…​kN`    | string |          | Keys whose mappings are to be removed from the cache.   | name                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                    | Example |
| ---------- | ------- | ---------------------------------------------- | ------- |
| `response` | boolean | `true` if replace happened, `false` otherwise. | true    |

#### Remove Value

Removes the mapping for a key only if currently mapped to the given value.

**Request:**

```
http://host:port/ignite?cmd=rmvval&key={rmvKey}&val={rmvVal}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key whose mapping is to be removed from the cache.      | name                                 |
| `val`       | string |          | Value expected to be associated with the specified key. | oldValue                             |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                           | Example |
| ---------- | ------- | ------------------------------------- | ------- |
| `response` | boolean | `false` if there was no matching key. | true    |

#### Add

Stores a given key-value pair in a cache if the cache does not contain the key.

**Request:**

```
http://host:port/ignite?cmd=add&key=newKey&val=newValue&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                                                                                                                                                                                | Example                              |
| ----------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used.                                                                                                                                                    | partitionedCache                     |
| `key`       | string |          | Key to be associated with the value.                                                                                                                                                                       | name                                 |
| `val`       | string |          | Value to be associated with the key.                                                                                                                                                                       | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.                                                                                                                                                          | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |
| `exp`       | long   | Yes      | Expiration time in milliseconds for the entry. When the parameter is set, the operation is executed with [ModifiedExpiryPolicy](https://ignite.apache.org/docs/latest/configuring-caches/expiry-policies). | 60000                                |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                             | Example |
| ---------- | ------- | ------------------------------------------------------- | ------- |
| `response` | boolean | `true` if value was stored in cache, `false` otherwise. | true    |

#### Put

Stores a given key-value pair in a cache.

**Request:**

```
http://host:port/ignite?cmd=put&key=newKey&val=newValue&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                                                                                                                                                                                | Example                              |
| ----------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used.                                                                                                                                                    | partitionedCache                     |
| `key`       | string |          | Key to be associated with values.                                                                                                                                                                          | name                                 |
| `val`       | string |          | Value to be associated with keys.                                                                                                                                                                          | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.                                                                                                                                                          | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |
| `exp`       | long   | Yes      | Expiration time in milliseconds for the entry. When the parameter is set, the operation is executed with [ModifiedExpiryPolicy](https://ignite.apache.org/docs/latest/configuring-caches/expiry-policies). | 60000                                |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                             | Example |
| ---------- | ------- | ------------------------------------------------------- | ------- |
| `response` | boolean | `true` if value was stored in cache, `false` otherwise. | true    |

#### Put all

Stores the given key-value pairs in cache.

**Request:**

```
http://host:port/ignite?cmd=putall&k1={putKey1}&k2={putKey2}&k3={putKey3}&v1={value1}&v2={value2}&v3={value3}&cacheName={cacheName}&destId={nodeId}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `k1…​kN`    | string |          | Keys to be associated with values.                      | name                                 |
| `v1…​vN`    | string |          | Values to be associated with keys.                      | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "1bcbac4b-3517-43ee-98d0-874b103ecf30",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                                   | Example |
| ---------- | ------- | ------------------------------------------------------------- | ------- |
| `response` | boolean | `true` if the values were stored in cache, `false` otherwise. | true    |

#### Put If Absent

Stores a given key-value pair in a cache if the cache does not contain the given key.

**Request:**

```
http://host:port/ignite?cmd=putifabs&key={getKey}&val={newVal}&cacheName={cacheName}
```

| Parameter   | Type   | Optional | Description                                                                                                                                                                                                | Example                              |
| ----------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used.                                                                                                                                                    | partitionedCache                     |
| `key`       | string |          | Key to be associated with value.                                                                                                                                                                           | name                                 |
| `val`       | string |          | Value to be associated with key.                                                                                                                                                                           | Jack                                 |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.                                                                                                                                                          | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |
| `exp`       | long   | Yes      | Expiration time in milliseconds for the entry. When the parameter is set, the operation is executed with [ModifiedExpiryPolicy](https://ignite.apache.org/docs/latest/configuring-caches/expiry-policies). | 60000                                |

**Response:**

The response field contains `true` if the entry was put, `false` otherwise.

```
{
  "affinityNodeId": "2bd7b049-3fa0-4c44-9a6d-b5c7a597ce37",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

#### Contains Key

Determines if cache contains an entry for the specified key.

**Request:**

```
http://host:port/ignite?cmd=conkey&key={getKey}&cacheName={cacheName}
```

| Parameter   | Type   | Optional | Description                                             | Example                              |
| ----------- | ------ | -------- | ------------------------------------------------------- | ------------------------------------ |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                     |
| `key`       | string |          | Key whose presence in this cache is to be tested.       | testKey                              |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "affinityNodeId": "2bd7b049-3fa0-4c44-9a6d-b5c7a597ce37",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                                  | Example |
| ---------- | ------- | ------------------------------------------------------------ | ------- |
| `response` | boolean | `true` if this map contains a mapping for the specified key. | true    |

#### Contains keys

Determines if cache contains any entries for the specified keys.

**Request:**

```
http://host:port/ignite?cmd=conkeys&k1={getKey1}&k2={getKey2}&cacheName={cacheName}
```

| Parameter   | Type   | Optional | Description                                             | Example                                |
| ----------- | ------ | -------- | ------------------------------------------------------- | -------------------------------------- |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache                       |
| `k1…​kN`    | string |          | Key whose presence in this cache is to be tested.       | key1, key2, …​, keyN                   |
| `destId`    | string | Yes      | Node ID for which the metrics are to be returned.       | `8daab5ea-af83-4d91-99b6-77ed2ca06647` |

**Response:**

```
{
  "affinityNodeId": "2bd7b049-3fa0-4c44-9a6d-b5c7a597ce37",
  "error": "",
  "response": true,
  "successStatus": 0
}
```

| Field      | Type    | Description                                                     | Example |
| ---------- | ------- | --------------------------------------------------------------- | ------- |
| `response` | boolean | `true` if this cache contains a mapping for the specified keys. | true    |

#### Get or Create Cache

Creates a cache with the given name if it does not exist.

**Request:**

```
http://host:port/ignite?cmd=getorcreate&cacheName={cacheName}
```

| Parameter                  | Type   | Optional | Description                                                                                                                                                                                                                                                  |
| -------------------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `cacheName`                | String | Yes      | Cache name. If not provided, the default cache is used.                                                                                                                                                                                                      |
| `backups`                  | int    | Yes      | Number of backups for cache data. Default is 0.                                                                                                                                                                                                              |
| `dataRegion`               | String | Yes      | Name of the data region the cache should belong to.                                                                                                                                                                                                          |
| `templateName`             | String | Yes      | Name of the cache template registered in Ignite to use as a configuration for the distributed cache. See the [Cache Template](https://ignite.apache.org/docs/latest/configuring-caches/configuration-overview#cache-templates) section for more information. |
| `cacheGroup`               | String | Yes      | Name of the group the cache should belong to.                                                                                                                                                                                                                |
| `writeSynchronizationMode` | String | Yes      | <p>Sets the write synchronization mode for the given cache:</p><ul><li><code>FULL_SYNC</code></li><li><code>FULL_ASYNC</code></li><li><code>PRIMARY_SYNC</code></li></ul>                                                                                    |

**Response:**

```
{
  "error": "",
  "response": null,
  "successStatus": 0
}
```

#### Destroy cache

Destroys cache with given name.

**Request:**

```
http://host:port/ignite?cmd=destcache&cacheName={cacheName}
```

| Parameter   | Type   | Optional | Description                                             | Example          |
| ----------- | ------ | -------- | ------------------------------------------------------- | ---------------- |
| `cacheName` | string | Yes      | Cache name. If not provided, the default cache is used. | partitionedCache |

**Response:**

```
{
  "error": "",
  "response": null,
  "successStatus": 0
}
```

#### Node

Gets information about a node.

**Request:**

```
http://host:port/ignite?cmd=node&attr={includeAttributes}&mtr={includeMetrics}&id={nodeId}&caches={includeCaches}
```

| Parameter | Type    | Optional | Description                                                                                                                                                                                                                                              | Example                              |
| --------- | ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `mtr`     | boolean | Yes      | Response includes metrics, if this parameter is `true`.                                                                                                                                                                                                  | true                                 |
| `attr`    | boolean | Yes      | Response includes attributes, if this parameter is `true`.                                                                                                                                                                                               | true                                 |
| `ip`      | string  |          | This parameter is optional, if id parameter is passed. Response is returned for node which has the IP.                                                                                                                                                   | 192.168.0.1                          |
| `id`      | string  |          | This parameter is optional, if ip parameter is passed. Response is returned for node which has the node ID.                                                                                                                                              | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |
| `caches`  | boolean | Yes      | <p>When set to <code>true</code> the cache information returned by node includes: name, mode, and SQL Schema.</p><p>When set to <code>false</code> the node command does not return any cache information.</p><p>Default value is <code>true</code>.</p> | true                                 |

**Response:**

```
{
  "error": "",
  "response": {
    "attributes": null,
    "caches": {},
    "consistentId": "127.0.0.1:47500",
    "defaultCacheMode": "REPLICATED",
    "metrics": null,
    "nodeId": "2d0d6510-6fed-4fa3-b813-20f83ac4a1a9",
    "replicaCount": 128,
    "tcpAddresses": ["127.0.0.1"],
    "tcpHostNames": [""],
    "tcpPort": 11211
  },
  "successStatus": 0
}
```

#### Log

Shows server logs.

**Request:**

```
http://host:port/ignite?cmd=log&from={from}&to={to}&path={pathToLogFile}
```

| Parameter | Type    | Optional | Description                                                            | Example                 |
| --------- | ------- | -------- | ---------------------------------------------------------------------- | ----------------------- |
| `from`    | integer | Yes      | Number of line to start from. Parameter is mandatory if to is passed.  | `0`                     |
| `path`    | string  | Yes      | The path to log file. If not provided the a default one is used.       | `/log/cache_server.log` |
| `to`      | integer | Yes      | Number to line to finish on. Parameter is mandatory if from is passed. | `1000`                  |

**Response:**

```
{
  "error": "",
  "response": ["[14:01:56,626][INFO ][test-runner][GridDiscoveryManager] Topology snapshot [ver=1, nodes=1, CPUs=8, heap=1.8GB]"],
  "successStatus": 0
}
```

#### Topology

Gets the information about cluster topology.

**Request:**

```
http://host:port/ignite?cmd=top&attr=true&mtr=true&id=c981d2a1-878b-4c67-96f6-70f93a4cd241
```

| Parameter | Type    | Optional | Description                                                                                                            | Example                              |
| --------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `mtr`     | boolean | Yes      | Response will include metrics, if this parameter is `true`.                                                            | true                                 |
| `attr`    | boolean | Yes      | Response will include attributes, if this parameter is `true`.                                                         | true                                 |
| `ip`      | string  | Yes      | This parameter is optional, if the `id` parameter is passed. Response will be returned for node which has the IP.      | 192.168.0.1                          |
| `id`      | string  | Yes      | This parameter is optional, if the `ip` parameter is passed. Response will be returned for node which has the node ID. | 8daab5ea-af83-4d91-99b6-77ed2ca06647 |

**Response:**

```
{
  "error": "",
  "response": [
    {
      "attributes": {
        ...
      },
      "caches": [
        {
          name: "",
          mode: "PARTITIONED"
        },
        {
          name: "partitionedCache",
          mode: "PARTITIONED",
          sqlSchema: "partitionedCache"
        }
      ],
      "consistentId": "127.0.0.1:47500",
      "metrics": {
        ...
      },
      "nodeId": "96baebd6-dedc-4a68-84fd-f804ee1ed995",
      "replicaCount": 128,
      "tcpAddresses": ["127.0.0.1"],
      "tcpHostNames": [""],
      "tcpPort": 11211
   },
   {
     "attributes": {
       ...
     },
		 "caches": [
       {
         name: "",
         mode: "REPLICATED"
       }
     ],
     "consistentId": "127.0.0.1:47501",
     "metrics": {
       ...
     },
     "nodeId": "2bd7b049-3fa0-4c44-9a6d-b5c7a597ce37",
     "replicaCount": 128,
     "tcpAddresses": ["127.0.0.1"],
     "tcpHostNames": [""],
     "tcpPort": 11212
   }
  ],
  "successStatus": 0
}
```

#### Execute a Task

Executes a given task in the cluster.

**Request:**

```
http://host:port/ignite?cmd=exe&name=taskName&p1=param1&p2=param2&async=true
```

| Parameter | Type    | Optional | Description                                              | Example    |
| --------- | ------- | -------- | -------------------------------------------------------- | ---------- |
| `name`    | string  |          | Name of the task to execute.                             | `summ`     |
| `p1…​pN`  | string  | Yes      | Argument of task execution.                              | arg1…​argN |
| `async`   | boolean | Yes      | Determines whether the task is performed asynchronously. | `true`     |

**Response:**

The response contains an error message, unique identifier of the task, the status and result of computation.

```
{
  "error": "",
  "response": {
    "error": "",
    "finished": true,
    "id": "~ee2d1688-2605-4613-8a57-6615a8cbcd1b",
    "result": 4
  },
  "successStatus": 0
}
```

#### Result of a Task

Returns the computation result for a given task.

**Request:**

```
http://host:port/ignite?cmd=res&id={taskId}
```

| Parameter | Type   | Optional | Description                                    | Example                                                                                |
| --------- | ------ | -------- | ---------------------------------------------- | -------------------------------------------------------------------------------------- |
| `id`      | string |          | ID of the task whose result is to be returned. | 69ad0c48941-4689aae0-6b0e-4d52-8758-ce8fe26f497d\~4689aae0-6b0e-4d52-8758-ce8fe26f497d |

**Response:**

The response contains information about errors (if any), ID of the task, and the status and result of computation.

```
{
  "error": "",
  "response": {
    "error": "",
    "finished": true,
    "id": "69ad0c48941-4689aae0-6b0e-4d52-8758-ce8fe26f497d~4689aae0-6b0e-4d52-8758-ce8fe26f497d",
    "result": 4
  },
  "successStatus": 0
}
```

#### SQL Query Execute

Runs SQL query over cache.

**Request:**

```
http://host:port/ignite?cmd=qryexe&type={type}&pageSize={pageSize}&cacheName={cacheName}&arg1=1000&arg2=2000&qry={query}
```

| Parameter    | Type    | Optional | Description                                                                                                                              | Example                                |
| ------------ | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| `type`       | string  |          | Type for the query                                                                                                                       | String                                 |
| `pageSize`   | number  |          | Page size for the query.                                                                                                                 | 3                                      |
| `cacheName`  | string  | Yes      | Cache name. If not provided, the default cache is used.                                                                                  | testCache                              |
| `arg1…​argN` | string  |          | Query arguments                                                                                                                          | 1000,2000                              |
| `qry`        | strings |          | Encoding sql query                                                                                                                       | `salary+%3E+%3F+and+salary+%3C%3D+%3F` |
| `keepBinary` | boolean | Yes      | do not deserialize [binary objects](https://ignite.apache.org/docs/data-modeling/data-modeling#binary-object-format), `false` by default | `true`                                 |

**Response:**

The response object contains the items returned by the query, a flag indicating the last page, and `queryId`.

```
{
  "error":"",
  "response":{
    "fieldsMetadata":[],
    "items":[
      {"key":3,"value":{"name":"Jane","id":3,"salary":2000}},
      {"key":0,"value":{"name":"John","id":0,"salary":2000}}],
    "last":true,
    "queryId":0},
  "successStatus":0
}
```

#### SQL Fields Query Execute

Runs SQL fields query over cache.

**Request:**

```
http://host:port/ignite?cmd=qryfldexe&pageSize=10&cacheName={cacheName}&qry={qry}
```

| Parameter    | Type    | Optional | Description                                                                                                                              | Example                                    |
| ------------ | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| `pageSize`   | number  |          | Page size for the query.                                                                                                                 | 3                                          |
| `cacheName`  | string  | Yes      | Cache name. If not provided, the default cache is used.                                                                                  | testCache                                  |
| `arg1…​argN` | string  |          | Query arguments.                                                                                                                         | 1000,2000                                  |
| `qry`        | strings |          | Encoding sql fields query.                                                                                                               | `select+firstName%2C+lastName+from+Person` |
| `keepBinary` | boolean | Yes      | do not deserialize [binary objects](https://ignite.apache.org/docs/data-modeling/data-modeling#binary-object-format), `false` by default | `true`                                     |

**Response:**

The response object contains the items returned by the query, fields query metadata, a flag indicating the last page, and `queryId`.

```
{
  "error": "",
  "response": {
    "fieldsMetadata": [
      {
        "fieldName": "FIRSTNAME",
        "fieldTypeName": "java.lang.String",
        "schemaName": "person",
        "typeName": "PERSON"
      },
      {
        "fieldName": "LASTNAME",
        "fieldTypeName": "java.lang.String",
        "schemaName": "person",
        "typeName": "PERSON"
      }
    ],
    "items": [["Jane", "Doe" ], ["John", "Doe"]],
    "last": true,
    "queryId": 0
  },
  "successStatus": 0
}
```

#### SQL Scan Query Execute

Runs a scan query over a cache.

**Request:**

```
http://host:port/ignite?cmd=qryscanexe&pageSize={pageSize}&cacheName={cacheName}&className={className}
```

| Parameter    | Type    | Optional | Description                                                                                                                              | Example                                     |
| ------------ | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| `pageSize`   | Number  |          | Page size for the query                                                                                                                  | 3                                           |
| `cacheName`  | String  | Yes      | Cache name. If not provided, the default cache is used.                                                                                  | testCache                                   |
| `className`  | String  | Yes      | Predicate class name for scan query. Class should implement `IgniteBiPredicate` interface.                                               | `org.apache.ignite.filters.PersonPredicate` |
| `keepBinary` | boolean | Yes      | do not deserialize [binary objects](https://ignite.apache.org/docs/data-modeling/data-modeling#binary-object-format), `false` by default | `true`                                      |

**Response:**

The response object contains the items returned by the scan query, fields query metadata, a flag indicating last page, and `queryId`.

```
{
  "error": "",
  "response": {
    "fieldsMetadata": [
      {
        "fieldName": "key",
        "fieldTypeName": "",
        "schemaName": "",
        "typeName": ""
      },
      {
        "fieldName": "value",
        "fieldTypeName": "",
        "schemaName": "",
        "typeName": ""
      }
    ],
    "items": [
      {
        "key": 1,
        "value": {
          "firstName": "Jane",
          "id": 1,
          "lastName": "Doe",
          "salary": 1000
        }
      },
      {
        "key": 3,
        "value": {
          "firstName": "Jane",
          "id": 3,
          "lastName": "Smith",
          "salary": 2000
        }
      }
    ],
    "last": true,
    "queryId": 0
  },
  "successStatus": 0
}
```

#### SQL Query Fetch

Gets next page for the query.

**Request:**

```
http://host:port/ignite?cmd=qryfetch&pageSize={pageSize}&qryId={queryId}
```

| Parameter  | Type   | Optional | Description                                                                                                  | Example |
| ---------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------ | ------- |
| `pageSize` | number |          | Page size for the query.                                                                                     | 3       |
| `qryId`    | number |          | Query id that is returned from the `Sql query execute`, `sql fields query execute`, or `sql fetch` commands. | 0       |

**Response:**

The response object contains the items returned by the query, a flag indicating the last page, and `queryId`.

```
{
  "error":"",
  "response":{
    "fieldsMetadata":[],
    "items":[["Jane","Doe"],["John","Doe"]],
    "last":true,
    "queryId":0
  },
  "successStatus":0
}
```

#### SQL Query Close

Closes query resources.

**Request:**

```
http://host:port/ignite?cmd=qrycls&qryId={queryId}
```

| Parameter | Type   | Optional | Description                                                                                                  | Example |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------ | ------- |
| `qryId`   | number |          | Query id that is returned from the `SQL query execute`, `SQL fields query execute`, or `SQL fetch` commands. | 0       |

**Response:**

The command returns 'true' if the query was closed successfully.

```
{
  "error":"",
  "response":true,
  "successStatus":0
}
```

#### Probe

Returns whether Ignite kernal as started.

**Request:**

```
http://host:port/ignite?cmd=probe
```

**Response:**

Returns HTTP Status code 200 if kernal has started, and 503 otherwise

```
{
  "error": "",
  "response": "grid has started",
  "successStatus": 0
}
```
