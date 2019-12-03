# Custom and private host names

Example scenarios for using custom and public host names.

Ambari Agents can be configured to register with the Ambari Server using custom host names and public host names. Ambari uses the host name of the agent to name and refer to that host in Ambari Web, for example in the **Hosts** list. The public host name, if configured, is used as an alias for the host when referenced in configuration and is used in **Quick Links** URLs.

To determine whether to use a custom host name or public host name, consider the following scenarios:

| Scenario | Configuration |
| :--- | :--- |
| If you have a host with the host name `revo1.bigconnect.local`, but you want it to show up in Ambari web UI as `c1r1.bigconnect.local`, you should: | Configure a custom host name |
| If you have a host with the host name `revo1.bigconnect.local` and want to use a DNS CNAME of `nn1.bigconnect.local` to be used for Quick Links and as a configuration alias, you should: | Configure a public host name |

