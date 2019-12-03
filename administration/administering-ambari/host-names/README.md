# Host names

The Ambari Server relies on the host names of Ambari Agent to communicate to operators which hosts belong members of the cluster. You can configure custom or private host names.

It is common in large cluster deployments to use DNS aliases for specific hosts, so that configuration files mentioning those hosts do not need to be changed when the services on that host are moved to another physical machine. For example, if you have multiple deployed applications that write to HDFS, using a DNS alias instead of a physical host name to refer to the HDFS NameNode allows you to move the NameNode to other physical machines without having to change those deployed application’s HDFS client configuration.

In Ambari, individual hosts can be configured to use a public host name when referencing individual hosts in configuration files or in **Quick Links**.   
  
For example, if you have a physical host with a FQDN of `revo1.bigconnect.local`, and you have a DNS CNAME that also points to that host using `nn1.bigconnect.local`, it is possible to configure Ambari to use `nn1.hortonworks.local` for **Quick Links** associated with that host, and whenever `nn1.bigconnect.local` is used in configuration, Ambari will understand that it is associated with the `revo1.bigconnect.local` host. That way if you need to move the NameNode to `revo4.bigconnect.local`, you can configure that new host to use `nn1.bigconnect.local` for its public host name without having to make client configuration changes.

{% hint style="info" %}
You still have to modify the specific configuration properties that reference `revo1.bigconnect.local` and update them with the alias which you have chosen to use, `nn1.hortonworks.local` in this case, in order for this feature to work as expected.
{% endhint %}

{% page-ref page="custom-and-private-host-names.md" %}

{% page-ref page="configure-a-public-host-name.md" %}

{% page-ref page="configure-a-custom-host-name.md" %}

{% page-ref page="public-host-name-limitations.md" %}

{% page-ref page="verifying-public-host-name-configuration.md" %}

{% page-ref page="changing-host-names.md" %}

