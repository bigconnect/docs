# UDP Source

The UDP Source origin reads messages from one or more UDP ports.

To use multiple threads for pipeline processing, use the UDP Multithreaded Source.&#x20;

UDP Source generates a record for every message. UDP Source can process [collectd](https://collectd.org/) messages, NetFlow 5 and NetFlow 9 messages, and the following types of syslog messages:

* [RFC 5424](https://tools.ietf.org/html/rfc5424)
* [RFC 3164](https://tools.ietf.org/html/rfc3164)
* Non-standard common messages, such as RFC 3339 dates with no version digit

When processing NetFlow messages, the stage generates different records based on the NetFlow version. When processing NetFlow 9, the records are generated based on the NetFlow 9 configuration properties.&#x20;

The origin can also read binary or character-based raw data.

When you configure UDP Source, you specify the ports to use and the batch size and wait time. When epoll is available, you can specify the number of receiver threads to use to increase the throughput of packets to the pipeline.

You also specify the data format for the data, then configure any related properties.

### Processing Raw Data <a href="#ariaid-title2" id="ariaid-title2"></a>

Use the Raw/Separated Data data format to enable the UDP Source origin to generate records from binary or character-based raw data.

When processing raw data, the origin can generate a record for each UDP packet that it receives. Or, if you specify a separator character, then the origin can generate multiple records from each UDP packet.

When generating multiple records, you specify the multiple value behavior: one record with only the first value, one record with all values as a list, or multiple records with one record for each value.

You can optionally specify an output field to use for the data. When not specified, the origin writes the raw data to the root field.

You might use the Raw/Separated Data data format to write raw data to a field that you later process using the Data Parser processor. This allows you to retain the raw data for another use.

### Receiver Threads <a href="#ariaid-title3" id="ariaid-title3"></a>

Receiver threads are used to pass data from the UDP source system to the origin. By default, the origin uses a single receiver thread.

You can configure the UDP Source origin to use additional receiver threads when Data Pipelines runs on a machine enabled for epoll. Epoll requires native libraries and is only available when Data Pipelines runs on recent versions of 64-bit Linux. When you enable multiple receiver threads, you increase the volume of data that can be passed to the origin at one time.

To use additional receiver threads, select the Use Native Transports (epoll) property, and then configure Number of Receiver Threads.
