# WebSocket Client

The WebSocket Client destination writes data to a WebSocket endpoint. Use the destination to send data to a WebSocket resource URL.&#x20;

The WebSocket Client destination opens a connection to the WebSocket endpoint for each batch of data that it writes. The destination sends each record to the WebSocket resource URL. When the batch is fully written, the destination closes the connection and then opens another connection for the next batch of data.

When you configure the WebSocket Client destination, you define the resource URL and headers to use for the requests. You can also configure SSL/TLS properties, including default transport protocols and cipher suites.

### Data Formats <a href="#ariaid-title2" id="ariaid-title2"></a>

The WebSocket Client destination writes data to WebSocket endpoints based on the data format that you select.

The WebSocket Client destination processes data formats as follows:

**Binary**

The stage writes binary data to a single field in the record.

**JSON**

The destination writes records as JSON data. You can use one of the following formats:

* Array - Each file includes a single array. In the array, each element is a JSON representation of each record.
* Multiple objects - Each file includes multiple JSON objects. Each object is a JSON representation of a record.

**Record**

The destination writes records in the Record data format.

**Text**

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.

When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.

When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.
