# Redis Lookup

The Redis Lookup processor performs key-value lookups in Redis and passes the lookup values to fields.

Use the Redis Lookup to enrich records with additional data. For example, you can configure the processor to use a city field as the key to look up zip code values in Redis, and pass the values to a new zip\_code output field.

When you configure the Redis Lookup processor, you specify whether the processor performs a bulk lookup of all keys in a batch, or performs an individual lookup of each key in a record. You use an expression to define the key to look up in Redis, and specify the output field to write the lookup values to. You also specify the data type of the Redis value.

You can configure the processor to locally cache the key-value pairs to improve performance.

You also specify properties to connect to Redis, including the URI of the Redis server.

### Data Types <a href="#ariaid-title2" id="ariaid-title2"></a>

When you define the key to look up in Redis, you specify the data type of the Redis value. The Redis Lookup processor converts the Redis data type to a Data Pipelines data type.

The following table lists the supported Redis data types and the corresponding Data Pipelines data type:

| Redis Data Type | Data Pipelines Data Type                                                                                                                                                                                                                     |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| String          | String                                                                                                                                                                                                                                       |
| List            | <p>List.</p><p>Converts the data to an indexed list of values, for example:</p><pre><code>0: &#x3C;value>
1: &#x3C;value>
2: &#x3C;value>
</code></pre>                                                                                      |
| Hash            | <p>Map.</p><p>Converts the data to a map of the Redis field names and values, for example:</p><pre><code>/&#x3C;first field name>: &#x3C;value>
/&#x3C;second field name>: &#x3C;value>
/&#x3C;third field name>: &#x3C;value>
</code></pre> |
| Set             | <p>List.</p><p>Converts the data to an indexed list of values, for example:</p><pre><code>0: &#x3C;value>
1: &#x3C;value>
2: &#x3C;value>
</code></pre>                                                                                      |

Note: The remaining Redis data types are not supported.

### Lookup Cache <a href="#ariaid-title3" id="ariaid-title3"></a>

To improve pipeline performance, you can configure the Redis Lookup processor to locally cache the key-value pairs returned from Redis.

The processor caches key-value pairs until the cache reaches the maximum size or the expiration time. When the first limit is reached, the processor evicts key-value pairs from the cache.

You can configure the following ways to evict key-value pairs from the cache:

**Size-based eviction**

Configure the maximum number of key-value pairs that the processor caches. When the maximum number is reached, the processor evicts the oldest key-value pairs from the cache.

**Time-based eviction**

Configure the amount of time that a key-value pair can remain in the cache without being written to or accessed. When the expiration time is reached, the processor evicts the key from the cache. The eviction policy determines whether the processor measures the expiration time since the last write of the value or since the last access of the value.

For example, you set the eviction policy to expire after the last access and set the expiration time to 60 seconds. After the processor does not access a key-value pair for 60 seconds, the processor evicts the key-value pair from the cache.

When you stop the pipeline, the processor clears the cache.

### Configuring a Redis Lookup Processor <a href="#ariaid-title4" id="ariaid-title4"></a>

Configure a Redis Lookup processor to perform key-value lookups in Redis.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Lookup tab, configure the following properties:

    | Lookup Property          | Description                                                                                                                                                                                                                                                                                                                                                                                          |
    | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Key Expression           | <p>Key to look up in Redis. Enter a key name or enter an expression that defines the key.For example, enter the following expression to use the data in the city field as the lookup key:</p><pre><code>${record:value('/city')}
    </code></pre>                                                                                                                                                       |
    | Output Field             | Name of the field in the record to pass the lookup value. You can specify an existing field or a new field. If the field does not exist, Redis Lookup creates the field.                                                                                                                                                                                                                             |
    | Data Type                | <p>Data type of the value in Redis.</p><p>Default is String.</p>                                                                                                                                                                                                                                                                                                                                     |
    | Mode                     | <p>Mode used to perform the lookups:</p><ul><li>Per Batch - Performs a bulk lookup of all keys in a batch. The processor performs a single lookup for each batch.</li><li>Per Key in Each Record - Performs individual lookups of each key in each record. If you configure multiple key expressions, the processor performs multiple lookups for each record.</li></ul><p>Default is Per Batch.</p> |
    | Enable Local Caching     | Specifies whether to locally cache the returned key-value pairs.                                                                                                                                                                                                                                                                                                                                     |
    | Maximum Entries to Cache | <p>Maximum number of key-value pairs to cache. When the maximum number is reached, the processor evicts the oldest key-value pairs from the cache.</p><p>Default is -1, which means unlimited.</p>                                                                                                                                                                                                   |
    | Eviction Policy Type     | <p>Policy used to evict key-value pairs from the local cache when the expiration time has passed:</p><ul><li>Expire After Last Access - Measures the expiration time since the key-value pair was last accessed by a read or a write.</li><li>Expire After Last Write - Measures the expiration time since the key-value pair was created, or since the value was last replaced.</li></ul>           |
    | Expiration Time          | <p>Amount of time that a key-value pair can remain in the local cache without being accessed or written to.</p><p>Default is 1 second.</p>                                                                                                                                                                                                                                                           |
    | Time Unit                | <p>Unit of time for the expiration time.</p><p>Default is seconds.</p>                                                                                                                                                                                                                                                                                                                               |
3.  On the Redis tab, configure the following properties:

    | Redis Property           | Description                                                                                                                                                                                                                                                                                                                                                                                                            |
    | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | URI                      | <p>URI of the Redis server. Use the following format:</p><pre><code>redis://&#x3C;host name>:&#x3C;port number>/&#x3C;database>
    </code></pre><p>You can omit the database if the server uses the default database.</p><p>You can optionally include your password to log in to the Redis server. For example:</p><pre><code>redis://:&#x3C;password>@&#x3C;host name>:&#x3C;port number>/&#x3C;database>
    </code></pre> |
    | Connection Timeout (sec) | <p>Maximum time in seconds to wait for a connection.</p><p>Default is 60 seconds.</p>                                                                                                                                                                                                                                                                                                                                  |
