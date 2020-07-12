# Datagram Data Format

When reading datagram data, origins generate a record for every message.The origins can process [collectd](https://collectd.org/) messages, NetFlow 5 and NetFlow 9 messages, and the following types of syslog messages:

* [RFC 5424](https://tools.ietf.org/html/rfc5424)
* [RFC 3164](https://tools.ietf.org/html/rfc3164)
* Non-standard common messages, such as RFC 3339 dates with no version digit

When processing NetFlow messages, the stage generates different records based on the NetFlow version. When processing NetFlow 9, the records are generated based on the NetFlow 9 configuration properties. 

