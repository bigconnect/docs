# NetFlow Data Processing

You can use Data Collector to process NetFlow 5 and NetFlow 9 data.

When processing NetFlow 5 data, Data Collector processes flow records based on information in the packet header. Data Collector expects multiple packets with header and flow records sent on the same connection, with no bytes in between. As a result, when processing NetFlow 5 messages, you have no data-related properties to configure.When processing template-based NetFlow 9 messages, Data Collector generates records based on cached templates, information in the packet header, and NetFlow 9 configuration properties in the stage. The NetFlow 9 properties display in different locations depending on the type of stage that you use:

* For origins that process messages directly from the network, such as the UDP Source origin, you configure the NetFlow 9 properties on a NetFlow 9 tab.
* For most origins and processors that process other types of data, such as JSON or protobuf, you configure NetFlow 9 properties on a Data Formats tab after you select Datagram or NetFlow as the data format.
* For the TCP Server, you specify the NetFlow TCP mode, and then configure NetFlow 9 properties on a NetFlow 9 tab.

When processing NetFlow 5 messages, the stage ignores any configured NetFlow 9 properties.

## Caching NetFlow 9 Templates

Processing NetFlow 9 data requires caching the templates used to process the messages. When you configure NetFlow 9 properties, you can specify the maximum number of templates to cache and how long to allow an unused template to remain in the cache. You can also configure the stage to allow an unlimited number of templates in the cache for an unlimited amount of time.When you configure caching limitations, templates can be ejected from the cache under the following conditions:

* When the cache is full and a new template appears.
* When a template exceeds the specified idle period.

Configure NetFlow 9 caching properties to allow the stage to retain templates for processing in a logical way. When a record requires the use of a template that is not in the cache, the record is passed to the stage for error handling.

For example, say you use the UDP Source origin to process NetFlow 9 data from five servers. Each server sends data using a different template, so to process data from these five servers, you can set the cache size to five templates. But to allow for additional servers that might be added later, you might set the template cache to a higher number.

Most servers resend templates periodically, so you might take this refresh interval into account when you configure the cache timeout.

For example, say your server resends templates every three minutes. If you set the cache timeout for two minutes, then a template that hasn't been used in two minutes gets evicted. If the server sends a packet that requires the evicted template, the stage generates an error record because the template is not available. If you set the cache timeout for four minutes and an unlimited cache size, then the templates from all servers remain in the cache until replaced by a new version of the template.

{% hint style="info" %}
Data Collector keeps the cached templates in memory
{% endhint %}

## NetFlow 5 Generated Records

When processing NetFlow 5 records, Data Collector ignores any configured NetFlow 9 configuration properties.

The generated NetFlow 5 records include processed data as fields in the record, with no additional metadata, as follows:

```javascript
​{
     ​"tcp_flags" : 27,
     ​"last" : 1503089880145,
     ​"length" : 360,
     ​"raw_first" : 87028333,
     ​"flowseq" : 0,
     ​"count" : 7,
     ​"proto" : 6,
     ​"dstaddr" : 1539135649,
     ​"seconds" : 1503002821,
     ​"id" : "27a647b5-9e3a-11e7-8db3-874a63bd401c",
     ​"engineid" : 0,
     ​"srcaddr_s" : "172.17.0.4",
     ​"sender" : "/0:0:0:0:0:0:0:1",
     ​"srcas" : 0,
     ​"readerId" : "/0:0:0:0:0:0:0:0:9999",
     ​"src_mask" : 0,
     ​"nexthop" : 0,
     ​"snmpinput" : 0,
     ​"dPkts" : 11214,
     ​"raw_sampling" : 0,
     ​"timestamp" : 1503002821000,
     ​"enginetype" : 0,
     ​"samplingint" : 0,
     ​"dstaddr_s" : "91.189.88.161",
     ​"samplingmode" : 0,
     ​"srcaddr" : -1408172028,
     ​"first" : 1503089849333,
     ​"raw_last" : 87059145,
     ​"dstport" : 80,
     ​"nexthop_s" : "0.0.0.0",
     ​"version" : 5,
     ​"uptime" : 0,
     ​"dOctets" : 452409,
     ​"nanos" : 0,
     ​"dst_mask" : 0,
     ​"packetid" : "b58f5750-7ccd-1000-8080-808080808080",
     ​"srcport" : 51156,
     ​"snmponput" : 0,
     ​"tos" : 0,
     "dstas" : 0
  }
```

## NetFlow 9 Generated Records

NetFlow 9 records are generated based on the Record Generation Mode that you select for the NetFlow 9 stage properties. You can include "interpreted" or processed values, raw data, or both in NetFlow 9 records.

NetFlow 9 records can include the following fields:

<table>
  <thead>
    <tr>
      <th style="text-align:left">NetFlow 9 Field Name</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Included...</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">flowKind</td>
      <td style="text-align:left">
        <p>Indicates the type of flow to be processed:</p>
        <ul>
          <li>FLOWSET for data from a flowset.</li>
          <li>OPTIONS for data from an options flow.</li>
        </ul>
      </td>
      <td style="text-align:left">In all NetFlow 9 records.</td>
    </tr>
    <tr>
      <td style="text-align:left">values</td>
      <td style="text-align:left">A map field with field names and values as processed by the stage based
        on the template specified in the packet header.</td>
      <td style="text-align:left">In NetFlow 9 records when you configure the Record Generation Mode property
        to include &#x201C;interpreted&#x201D; data in the record.</td>
    </tr>
    <tr>
      <td style="text-align:left">packetHeader</td>
      <td style="text-align:left">A map field containing information about the packet. Typically includes
        information such as the source ID and the number of records in the packet.</td>
      <td
      style="text-align:left">In all NetFlow 9 records.</td>
    </tr>
    <tr>
      <td style="text-align:left">rawValues</td>
      <td style="text-align:left">A map field with the fields defined by the associated template and the
        raw, unprocessed, bytes for those fields.</td>
      <td style="text-align:left">In NetFlow 9 records when you configure the Record Generation Mode property
        to include raw data in the record.</td>
    </tr>
  </tbody>
</table>

### Sample Raw and Interpreted Record

When you set the Record Generation Mode property to Raw and Interpreted Data, the resulting record includes all of the possible NetFlow 9 fields, as follows:

```javascript
{
      "flowKind" : "FLOWSET",
      "values" : {
         "ICMP_TYPE" : 0,
         "L4_DST_PORT" : 9995,
         "TCP_FLAGS" : 0,
         "L4_SRC_PORT" : 52767,
         "INPUT_SNMP" : 0,
         "FIRST_SWITCHED" : 86400042,
         "PROTOCOL" : 17,
         "IN_BYTES" : 34964,
         "OUTPUT_SNMP" : 0,
         "LAST_SWITCHED" : 86940154,
         "IPV4_SRC_ADDR" : "127.0.0.1",
         "SRC_AS" : 0,
         "IN_PKTS" : 29,
         "IPV4_DST_ADDR" : "127.0.0.1",
         "DST_AS" : 0,
         "SRC_TOS" : 0,
         "FORWARDING_STATUS" : 0
      },
      "packetHeader" : {
         "flowRecordCount" : 8,
         "sourceIdRaw" : "AAAAAQ==",
         "version" : 9,
         "sequenceNumber" : 0,
         "unixSeconds" : 1503002821,
         "sourceId" : 1,
         "sysUptimeMs" : 0
      },
      "rawValues" : {
         "OUTPUT_SNMP" : "AAA=",
         "IN_BYTES" : "AACIlA==",
         "LAST_SWITCHED" : "BS6Z+g==",
         "IPV4_SRC_ADDR" : "fwAAAQ==",
         "SRC_AS" : "AAA=",
         "IPV4_DST_ADDR" : "fwAAAQ==",
         "IN_PKTS" : "AAAAHQ==",
         "DST_AS" : "AAA=",
         "FORWARDING_STATUS" : "AA==",
         "SRC_TOS" : "AA==",
         "ICMP_TYPE" : "AAA=",
         "TCP_FLAGS" : "AA==",
         "L4_DST_PORT" : "Jws=",
         "L4_SRC_PORT" : "zh8=",
         "INPUT_SNMP" : "AAA=",
         "FIRST_SWITCHED" : "BSZcKg==",
         "PROTOCOL" : "EQ=="
      }
   }
```

### Sample Interpreted Record

When you set the Record Generation Mode property to Interpreted Only, the resulting record omits the rawValues field from the record, as follows:

```javascript
{
      "flowKind" : "FLOWSET",
      "values" : {
         "ICMP_TYPE" : 0,
         "L4_DST_PORT" : 9995,
         "TCP_FLAGS" : 0,
         "L4_SRC_PORT" : 52767,
         "INPUT_SNMP" : 0,
         "FIRST_SWITCHED" : 86400042,
         "PROTOCOL" : 17,
         "IN_BYTES" : 34964,
         "OUTPUT_SNMP" : 0,
         "LAST_SWITCHED" : 86940154,
         "IPV4_SRC_ADDR" : "127.0.0.1",
         "SRC_AS" : 0,
         "IN_PKTS" : 29,
         "IPV4_DST_ADDR" : "127.0.0.1",
         "DST_AS" : 0,
         "SRC_TOS" : 0,
         "FORWARDING_STATUS" : 0
      },
      "packetHeader" : {
         "flowRecordCount" : 8,
         "sourceIdRaw" : "AAAAAQ==",
         "version" : 9,
         "sequenceNumber" : 0,
         "unixSeconds" : 1503002821,
         "sourceId" : 1,
         "sysUptimeMs" : 0
      },
   }
```

### Sample Raw Record

When you set the Record Generation Mode property to Raw Only, the resulting record omits the values field that contains processed data, as follows:

```javascript
{
      "flowKind" : "FLOWSET",
       "packetHeader" : {
         "flowRecordCount" : 8,
         "sourceIdRaw" : "AAAAAQ==",
         "version" : 9,
         "sequenceNumber" : 0,
         "unixSeconds" : 1503002821,
         "sourceId" : 1,
         "sysUptimeMs" : 0
      },
      "rawValues" : {
         "OUTPUT_SNMP" : "AAA=",
         "IN_BYTES" : "AACIlA==",
         "LAST_SWITCHED" : "BS6Z+g==",
         "IPV4_SRC_ADDR" : "fwAAAQ==",
         "SRC_AS" : "AAA=",
         "IPV4_DST_ADDR" : "fwAAAQ==",
         "IN_PKTS" : "AAAAHQ==",
         "DST_AS" : "AAA=",
         "FORWARDING_STATUS" : "AA==",
         "SRC_TOS" : "AA==",
         "ICMP_TYPE" : "AAA=",
         "TCP_FLAGS" : "AA==",
         "L4_DST_PORT" : "Jws=",
         "L4_SRC_PORT" : "zh8=",
         "INPUT_SNMP" : "AAA=",
         "FIRST_SWITCHED" : "BSZcKg==",
         "PROTOCOL" : "EQ=="
      }
   }
```

