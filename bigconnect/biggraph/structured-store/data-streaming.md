# Data Streaming

Ignite provides a Data Streaming API that can be used to inject large amounts of data into an Ignite cluster. The main goal of streaming is efficient, quick data loading. The data that is added to the streamer is automatically organized and distributed between the nodes in partition-aware and parallel manner.

<figure><img src="https://ignite.apache.org/docs/2.15.0/images/data_streaming.png" alt=""><figcaption></figcaption></figure>

The Data Streaming API is designed to be scalable and provides _at-least-once_ delivery semantics for the data streamed into Ignite, meaning each entry is processed at least once.

### Usage

A data streamer is associated with a specific cache and provides an interface for streaming data into the cache.

In a typical scenario, you obtain a data streamer and use one of its methods to stream data into the cache, and Ignite takes care of the rest.

You can obtain the data streamer for a specific cache as follows:

```java
// Get the data streamer reference and stream data.
try (IgniteDataStreamer<Integer, String> stmr = ignite.dataStreamer("myCache")) {
    // Stream entries.
    for (int i = 0; i < 100000; i++)
        stmr.addData(i, Integer.toString(i));
}
System.out.println("dataStreamerExample output:" + cache.get(99999));
```

In the Java version of Ignite, a data streamer is an implementation of the `IgniteDataStreamer` interface. `IgniteDataStreamer` provides a number of `addData(…​)` methods for adding key-value pairs to caches.

The streamer can accept data to load by multiple threads.

A good use of streaming is data preloading.

### Limitations

DataStreamer doesn’t guarantee:

* By default, data consistency until successfully finished;
* Immediate data loading. Data can be kept for a while before loading;
* Data order. Data records may be loaded into a cache in a different order compared to load into the streamer;
* By default, working with external storages.

If  'allowOverwrite' property is 'false' (default), consider:

* You should not have the same keys repeating in the data being streamed;
* Streamer cancelation or streamer node failure can cause data inconsistency;
* If loading into a persistent cache, concurrently created snapshot may contain inconsistent data and might not be restored entirely.

Most important behavior of Ignite Data Streamer is defined by stream receiver and 'allowOverwite' setting.

### Streamer receivers.

Ignite DataStreamer is an orchestrator and doesn’t write data itself. [StreamerReceiver](https://ignite.apache.org/releases/2.15.0/javadoc/org/apache/ignite/stream/StreameReceiver.html) does. The default receiver is designed for fastest load and fewer network requests. With this receiver, streamer focuses on parallel transfer of backup and primary records.

You can set your own receiver. See Stream Transformer and Stream Visitor. The logic implemented in a stream receiver is executed on the node where data is to be stored.

```java
try (IgniteDataStreamer<Integer, String> stmr = ignite.dataStreamer("myCache")) {

    stmr.allowOverwrite(true);

    stmr.receiver((StreamReceiver<Integer, String>) (cache, entries) -> entries.forEach(entry -> {

        // do something with the entry

        cache.put(entry.getKey(), entry.getValue());
    }));
}
```

Changing receiver to non-default changes data distribution algorithm. With non-default receiver streamer sends data batches only to primary node receiver. And primary node needs another requests to send backup writes.

### Overwritting data <a href="#overwritting" id="overwritting"></a>

By default, existing keys aren’t overwritten. You can change that behavior by setting the `allowOverwrite` property of the data streamer to `true`. Since the default receiver does not overwrite data, other provided one is automatically chosen. Any non-default receiver is considered as not-overwriting. And `allowOverwrite` property says `true`. However, your own receiver may use `putIfAbsent` for instance.

#### Stream Transformer

A stream transformer is a convenient implementation of a stream receiver, that updates the data in the stream. Stream transformers take advantage of the colocation feature and update the data on the node where it is going to be stored.

In the example below, we use a stream transformer to increment a counter for each distinct word found in the text stream.

```java
String[] text = { "hello", "world", "hello", "Ignite" };
CacheConfiguration<String, Long> cfg = new CacheConfiguration<>("wordCountCache");

IgniteCache<String, Long> stmCache = ignite.getOrCreateCache(cfg);

try (IgniteDataStreamer<String, Long> stmr = ignite.dataStreamer(stmCache.getName())) {
    // Allow data updates.
    stmr.allowOverwrite(true);

    // Configure data transformation to count instances of the same word.
    stmr.receiver(StreamTransformer.from((e, arg) -> {
        // Get current count.
        Long val = e.getValue();

        // Increment count by 1.
        e.setValue(val == null ? 1L : val + 1);

        return null;
    }));

    // Stream words into the streamer cache.
    for (String word : text)
        stmr.addData(word, 1L);

}
```

#### Stream Visitor

A stream visitor is another implementation of a stream receiver, which visits every key-value pair in the stream.

In the example below, we have 2 caches: "marketData", and "instruments". We receive market data ticks and put them into the streamer for the "marketData" cache. The stream visitor for the "marketData" streamer is invoked on the cluster member mapped to the particular market symbol. Upon receiving individual market ticks it updates the "instrument" cache with the latest market price.

Note, that we do not update the "marketData" cache at all, leaving it empty. We simply use it for colocated processing of the market data within the cluster directly on the node where the data is stored.

```java
static class Instrument {
    final String symbol;
    Double latest;
    Double high;
    Double low;

    public Instrument(String symbol) {
        this.symbol = symbol;
    }

}

static Map<String, Double> getMarketData() {
    //populate market data somehow
    return new HashMap<>();
}

@Test
void streamVisitorExample() {
    try (Ignite ignite = Ignition.start()) {
        CacheConfiguration<String, Double> mrktDataCfg = new CacheConfiguration<>("marketData");
        CacheConfiguration<String, Instrument> instCfg = new CacheConfiguration<>("instruments");

        // Cache for market data ticks streamed into the system.
        IgniteCache<String, Double> mrktData = ignite.getOrCreateCache(mrktDataCfg);

        // Cache for financial instruments.
        IgniteCache<String, Instrument> instCache = ignite.getOrCreateCache(instCfg);

        try (IgniteDataStreamer<String, Double> mktStmr = ignite.dataStreamer("marketData")) {
            // Note that we do not populate the 'marketData' cache (it remains empty).
            // Instead we update the 'instruments' cache based on the latest market price.
            mktStmr.receiver(StreamVisitor.from((cache, e) -> {
                String symbol = e.getKey();
                Double tick = e.getValue();

                Instrument inst = instCache.get(symbol);

                if (inst == null)
                    inst = new Instrument(symbol);

                // Update instrument price based on the latest market tick.
                inst.high = Math.max(inst.high, tick);
                inst.low = Math.min(inst.low, tick);
                inst.latest = tick;

                // Update the instrument cache.
                instCache.put(symbol, inst);
            }));

            // Stream market data into the cluster.
            Map<String, Double> marketData = getMarketData();
            for (Map.Entry<String, Double> tick : marketData.entrySet())
                mktStmr.addData(tick);
        }
    }
}
```

### Configuring Data Streamer Thread Pool Size

The data streamer thread pool is dedicated to process batches coming from the data streamers.

The default pool size is `max(8, total number of cores)`. Use `IgniteConfiguration.setDataStreamerThreadPoolSize(…​)` to change the pool size.
