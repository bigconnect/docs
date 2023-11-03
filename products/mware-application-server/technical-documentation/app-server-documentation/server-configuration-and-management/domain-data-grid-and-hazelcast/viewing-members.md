# Viewing Members

This section details how to visualize the members of the Domain Data Grid in MWARE AS.

### Viewing Data Grid Members from the Admin Console <a href="#viewing-members-admin-console" id="viewing-members-admin-console"></a>

To view data grid members from the admin console:

#### Viewing from the DAS

Selected the **Data Grid** menu option from the domain menu in the sidebar and select the _Data Grid Instances_ tab:

![Domain Cluster Members](https://docs.payara.fish/community/docs/\_images/hazelcast/hazelcast-admin-console-view-cluster-members-from-domain.png)

### Viewing Cluster Members via asadmin

To view the current cluster members from the command line, run one of the following `asadmin` commands:

```shell
asadmin> list-hazelcast-members
\{ server-MicroShoal-/192.168.1.148:4900-this \}

asadmin> list-hazelcast-cluster-members

Instance Name  Instance Group  Instance Type  Host Name  HTTP Ports  HTTPS Ports  Admin Port  Hazelcast Port  Lite Member  Deployed Applications  Last Hearbeat
server         server-config   DAS            127.0.1.1  8080        8181         4848        5900            false        __admingui             2019-03-05 09:25:42
```
