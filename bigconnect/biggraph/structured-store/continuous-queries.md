# Continuous Queries

A continuous query is a query that monitors data modifications occurring in a cache. Once a continuous query is started, you get notified of all the data changes that fall into your query filter.

All update events are propagated to the local listener that must be registered in the query. Continuous query implementation guarantees exactly once delivery of an event to the local listener.

You can also specify a remote filter to narrow down the range of entries that are monitored for updates.

### Local Listener

When a cache gets modified (an entry is inserted, updated, or deleted), an event is sent to the continuous query’s local listener so that your application can react accordingly. The local listener is executed on the node that initiated the query.

Note that the continuous query throws an exception if started without a local listener.

```java
IgniteCache<Integer, String> cache = ignite.getOrCreateCache("myCache");

ContinuousQuery<Integer, String> query = new ContinuousQuery<>();

query.setLocalListener(new CacheEntryUpdatedListener<Integer, String>() {

    @Override
    public void onUpdated(Iterable<CacheEntryEvent<? extends Integer, ? extends String>> events)
        throws CacheEntryListenerException {
        // react to the update events here
    }
});

cache.query(query);
```

### Initial Query

You can specify an initial query that is executed before the continuous query gets registered in the cluster and before you start to receive updates. To specify an initial query, use the `ContinuousQuery.setInitialQuery(…​)` method.

Just like scan queries, a continuous query is executed via the `query()` method that returns a cursor. When an initial query is set, you can use that cursor to iterate over the results of the initial query.

```java
IgniteCache<Integer, String> cache = ignite.getOrCreateCache("myCache");

ContinuousQuery<Integer, String> query = new ContinuousQuery<>();

// Setting an optional initial query.
// The query will return entries for the keys greater than 10.
query.setInitialQuery(new ScanQuery<>((k, v) -> k > 10));

//mandatory local listener
query.setLocalListener(events -> {
});

try (QueryCursor<Cache.Entry<Integer, String>> cursor = cache.query(query)) {
    // Iterating over the entries returned by the initial query 
    for (Cache.Entry<Integer, String> e : cursor)
        System.out.println("key=" + e.getKey() + ", val=" + e.getValue());
}
```

### Remote Filter

This filter is executed for each updated key and evaluates whether the update should be propagated to the query’s local listener. If the filter returns `true`, then the local listener is notified about the update.

For redundancy reasons, the filter is executed for both primary and backup versions (if backups are configured) of the key. Because of this, a remote filter can be used as a remote listener for update events.

```java
ContinuousQuery<Integer, String> qry = new ContinuousQuery<>();

qry.setLocalListener(events ->
    events.forEach(event -> System.out.format("Entry: key=[%s] value=[%s]\n", event.getKey(), event.getValue()))
);

qry.setRemoteFilterFactory(new Factory<CacheEntryEventFilter<Integer, String>>() {
    @Override
    public CacheEntryEventFilter<Integer, String> create() {
        return new CacheEntryEventFilter<Integer, String>() {
            @Override
            public boolean evaluate(CacheEntryEvent<? extends Integer, ? extends String> e) {
                System.out.format("the value for key [%s] was updated from [%s] to [%s]\n", e.getKey(), e.getOldValue(), e.getValue());
                return true;
            }
        };
    }
});
```

### Remote Transformer

By default, continuous queries send the whole updated object to the local listener. This can lead to excessive network usage, especially if the object is very large. Moreover, applications often need only a subset of fields of the object.

To address these cases, you can use a continuous query with a transformer. A transformer is a function that is executed on remote nodes for every updated object and sends back only the results of the transformation.

```java
IgniteCache<Integer, Person> cache = ignite.getOrCreateCache("myCache");

// Create a new continuous query with a transformer.
ContinuousQueryWithTransformer<Integer, Person, String> qry = new ContinuousQueryWithTransformer<>();

// Factory to create transformers.
Factory factory = FactoryBuilder.factoryOf(
    // Return one field of a complex object.
    // Only this field will be sent over to the local listener.
    (IgniteClosure<CacheEntryEvent, String>)
        event -> ((Person)event.getValue()).getName()
);

qry.setRemoteTransformerFactory(factory);

// Listener that will receive transformed data.
qry.setLocalListener(names -> {
    for (String name : names)
        System.out.println("New person name: " + name);
});
```

### Events Delivery Guarantees

Continuous queries ensure the exactly-once semantic for the delivery of events to the clients' local listeners.

Both primary and backup nodes maintain an update queue that holds events that are processed by continuous queries on the server side but yet to be delivered to the clients. Suppose a primary node crashes or the cluster topology changes for any reason. In that case, every backup node flushes the content of its update queue to the client, making sure that every event is delivered to the client’s local listener.

Ignite manages a special per-partition update counter that helps to avoid duplicate notifications. Once an entry in some partition is updated, a counter for this partition is incremented on both primary and backup nodes. The value of this counter is also sent along with the event notification to the client. Thus, the client can skip already-processed events. Once the client confirms that an event is received, the primary and backup nodes remove the record for this event from their backup queues.
