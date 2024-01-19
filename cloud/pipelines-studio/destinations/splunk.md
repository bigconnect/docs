# Splunk

The Splunk destination writes data to Splunk using the Splunk HTTP Event Collector (HEC).&#x20;

The destination sends HTTP POST requests to the HEC endpoint using the JSON data format. The destination generates one HTTP request for each batch, sending multiple records at a time. Each record must contain the event data and optionally the event metadata in the format required by Splunk.

Before you configure the destination, you must complete several prerequisites including enabling HEC in Splunk and creating an HEC authentication token.

When you configure the Splunk destination, you supply the Splunk API endpoint and the HEC authentication token. You can configure the timeout, request transfer encoding, and authentication type. You can configure the destination to use the Gzip or Snappy compression format to write the data. You can optionally use an HTTP proxy and configure SSL/TLS properties.

You can also configure the destination to log request and response information.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you can write to Splunk, you must complete the following prerequisites:Enable HTTP Event Collector (HEC)By default, HEC in Splunk is disabled. [Enable HEC](http://docs.splunk.com/Documentation/Splunk/latest/Data/HECWalkthrough) as described in the Splunk documentation.Create an HTTP Event Collector (HEC) tokenTo send data to HEC, the Splunk destination must use a token to authenticate to the Splunk server on which HEC is running. [Create the HEC token](http://docs.splunk.com/Documentation/Splunk/latest/Data/HECWalkthrough) as described in the Splunk documentation.When you configure the Splunk destination in Data Pipelines, you enter this token value.

### Required Record Format <a href="#ariaid-title3" id="ariaid-title3"></a>

Splunk requires that the event data and metadata be correctly formatted in the record. If the record is formatted incorrectly, an error occurs and the destination fails to write to Splunk. When you design a pipeline with the Splunk destination, you must ensure that the record sent to the destination uses the required format.

The record must contain an /event field that contains the event data. The /event field can be a string, map, or list-map field. For more information, see [Event data](http://docs.splunk.com/Documentation/Splunk/7.1.1/Data/FormateventsforHTTPEventCollector) in the Splunk documentation.Important: The Splunk destination does not support raw events. Events must be sent in the /event field.

The record can optionally contain event metadata fields. Splunk includes several pre-defined keys that can be included in the event metadata. Any metadata key-value pairs that are not included in the event are set to values defined for the token on the Splunk server. For a list of the keys that can be included in event metadata, see [Event metadata](http://docs.splunk.com/Documentation/Splunk/7.1.1/Data/FormateventsforHTTPEventCollector) in the Splunk documentation.

For example, the following record includes three of the keys that can be included in event metadata and an /event field using the Map data type:

```
{
    "time": 1437522387,
    "host": "myserver.example.com",
    "source": "myapp",
    "event": { 
        "message": "Here is my message",
        "severity": "INFO"
    }
}
```

The following record includes five of the keys that can be included in event metadata and an /event field using the String data type:

```
{
    "time": 1426279439, // epoch time
    "host": "localhost",
    "source": "datasource",
    "sourcetype": "txt",
    "index": "main",
    "event": "Here is my event" 
}
```

### Logging Request and Response Data <a href="#ariaid-title4" id="ariaid-title4"></a>

The Splunk destination can log request and response data to the Data Pipelines log.

When enabling logging, you configure the following properties:

VerbosityThe type of data to include in logged messages:

* Headers\_Only - Includes request and response headers.
* Payload\_Text - Includes request and response headers as well as any text payloads.
* Payload\_Any - Includes request and response headers and the payload, regardless of type.

**Log Level**

The level of messages to include in the Data Pipelines log. When you select a level, higher level messages are also logged. That is, if you select the Warning log level, then Severe and Warning messages are written to the Data Pipelines log.

Note: The configured log level for Data Pipelines can limit the level of detail that is logged. For example, if you set the log level to Finest to log detailed trace information, but Data Pipelines is configured for ERROR, then the origin only writes Severe level messages.

The following table describes the stage log levels and the corresponding Data Pipelines log levels needed to enable the logging:

| Stage Log Level | Data Pipelines | Description                                |
| --------------- | -------------- | ------------------------------------------ |
| Severe          | ERROR          | Only messages indicating serious failures. |
| Warning         | WARN           | Messages warning of potential problems.    |
| Info            | INFO           | Informational messages.                    |
| Fine            | DEBUG          | Basic tracing information.                 |
| Finer           | DEBUG          | Detailed tracing information.              |
| Finest          | TRACE          | Highly detailed tracing information.       |

The name of this stage logger is `com.streamsets.http.RequestLogger`.

**Max entity size**

The maximum size of message data to write to the log. Use to limit the volume of data written to the Data Pipelines log for any single message.
