# UDP Multithreaded Source

The UDP Multithreaded Source origin reads messages from one or more UDP ports. The origin can create multiple worker threads to enable parallel processing in a multithreaded pipeline.&#x20;

UDP Multithreaded Source generates a record for every message. UDP Multithreaded Source can process [collectd](https://collectd.org/) messages, NetFlow 5 and NetFlow 9 messages, and the following types of syslog messages:

* [RFC 5424](https://tools.ietf.org/html/rfc5424)
* [RFC 3164](https://tools.ietf.org/html/rfc3164)
* Non-standard common messages, such as RFC 3339 dates with no version digit

When processing NetFlow messages, the stage generates different records based on the NetFlow version. When processing NetFlow 9, the records are generated based on the NetFlow 9 configuration properties.

The origin can also read binary or character-based raw data.

When you configure UDP Multithreaded Source, you specify the ports to use and the batch size and wait time. You specify the number of worker threads to use in multithreaded processing and you can specify the packet queue size. When epoll is available on the Data Pipelines machine, you can also specify the number of receiver threads to use to increase the throughput of packets to the pipeline.

You specify the data format for the data, then configure any related properties.

### Processing Raw Data <a href="#ariaid-title2" id="ariaid-title2"></a>

Use the Raw/Separated Data data format to enable the UDP Multithreaded Source origin to generate records from binary or character-based raw data.

When processing raw data, the origin can generate a record for each UDP packet that it receives. Or, if you specify a separator character, then the origin can generate multiple records from each UDP packet.

When generating multiple records, you specify the multiple value behavior: one record with only the first value, one record with all values as a list, or multiple records with one record for each value.

You can optionally specify an output field to use for the data. When not specified, the origin writes the raw data to the root field.

You might use the Raw/Separated Data data format to write raw data to a field that you later process using the Data Parser processor. This allows you to retain the raw data for another use.

### Receiver and Worker Threads <a href="#ariaid-title3" id="ariaid-title3"></a>

The UDP Multithreaded Source origin uses both of the following types of threads:Receiver threadsUsed to pass data from the operating system socket to the origin's packet queue. By default, the origin uses a single receiver thread.

You can configure the origin to use multiple receiver threads when Data Pipelines runs on a machine enabled for epoll. Epoll requires native libraries and is only available when Data Pipelines runs on recent versions of 64-bit Linux.

When you enable multiple receiver threads, you increase the rate that data can be passed to the origin, but at the cost of a standard increase of overhead for thread management.

To use additional receiver threads, select the Use Native Transports (epoll) property, and then configure Number of Receiver Threads.

Worker threadsUsed to perform multithreaded pipeline processing. By default, the origin uses a single thread for pipeline processing. You can increase the number of threads to use to perform parallel processing of larger volumes of data.&#x20;

To use additional worker threads for parallel processing, increase the Number of Worker Threads property.

### Packet Queue <a href="#ariaid-title4" id="ariaid-title4"></a>

The UDP Multithreaded Source origin uses a packet queue to hold incoming data in memory until the data can be incorporated in a batch and passed through the pipeline. When the packet queue is full, incoming packets are dropped. The number of packets that are dropped is noted in stage metrics.

When you configure the origin, you can specify the maximum number of packets to allow in the queue. The default is 200,000. Because the packet queue uses Data Pipelines heap memory, when increasing the size of the queue, you should consider increasing the Data Pipelines heap size as well.

### Multithreaded Pipelines <a href="#ariaid-title5" id="ariaid-title5"></a>

The UDP Multithreaded Source origin performs parallel processing and enables the creation of a multithreaded pipeline.

When you enable multithreaded processing, the UDP Multithreaded Source origin uses multiple concurrent threads for pipeline processing based on the Number of Worker Threads property. When you start the pipeline, the origin creates the number of threads specified in the property.

As packets arrive from the specified UDP ports, they enter the packet queue. There is a single instance of the packet queue per pipeline. All receiver threads (which can be more than one, when using epoll) place packets onto the queue. At the same time, each worker thread removes packets from the queue, parses them according to the specified data format, and processes the rest of the pipeline using a pipeline runner.

A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin. Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

For example, say you enable multithreaded processing and set the Number of Worker Threads property to 5. When you start the pipeline, the origin creates five threads, and Data Pipelines creates a matching number of pipeline runners. The origin adds incoming data to the packet queue, creates batches of data from the queue and then passes the batches to the pipeline runners for processing.

Each pipeline runner performs the processing associated with the rest of the pipeline. After a batch is written to pipeline destinations, the pipeline runner becomes available for another batch of data. Each batch is processed and written as quickly as possible, independent from other batches processed by other pipeline runners, so batches may be written differently from the read-order.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

### Metrics for Performance Tuning <a href="#ariaid-title6" id="ariaid-title6"></a>

The UDP Multithreaded Source origin provides packet queue metrics that you can use to tune pipeline performance.

The origin provides the following packet queue metrics:

* Dropped Packets - The number of packets that were dropped because the packet queue was full.
* Queue Size - The current size of the packet queue.
* Queued Packets - The total number of packets that have passed through the packet queue for processing.

These metrics can help you determine how to improve pipeline performance. For example, if you have a high volume of dropped packets and the queue size seems to be maxed out as you monitor the pipeline, you might increase the number of worker threads for the pipeline to allow for greater throughput. Or, if you have relatively high bursts of data volume and find packets getting dropped during those bursts, consider increasing the packet queue size to better accommodate them.

If the queue size is not maxed out, but the number of queued packets does not seem as high as you expect, you might be dropping packets on the operating system side. When epoll is available - that is, when Data Pipelines runs on recent versions of 64-bit Linux - increasing the number of receiver threads can increase the volume of packets that are passed to the origin.
