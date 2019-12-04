# Establishing Rack Awareness

The term Rack Awareness means defining the physical rack on which a cluster host resides. Establishing Rack awareness can increase availability of data blocks and improve cluster performance. Co-locating data replication blocks on one physical rack speeds replication operations. The HDFS balancer and DataNode decommissioning are rack-aware operations. Rack awareness is not established by default, when a cluster is deployed or a new host is added, using Ambari. Instead the entire cluster is assigned to one, default rack.

You can establish rack awareness in two ways. Either you can set the rack ID using Ambari or you can set the rack ID using a custom topology script.

{% page-ref page="set-the-rack-id-for-ambari.md" %}

{% page-ref page="set-the-rack-id-on-a-host.md" %}



