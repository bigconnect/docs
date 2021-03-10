# Networking

Networks is a free feature for the BigConnect Cloud, which allows you to create private IPv4 networks between your BigConnect Cloud Servers. For example, you could create a database server which is only available on the private network instead of binding it to the public network, where it would be accessible from everywhere. 

The traffic on the private network interfaces is free and will not be charged. Since Networks is a layer 3 feature, you can only use the IP addresses allocated by the backend. Networks support IPv4 only. 

Networks provide private layer 3 links between Cloud Servers using dedicated network interfaces. They can be used to conveniently construct multi-tier architectures spanning multiple locations.

You can use the Cloud Console to manage the IP addresses in Networks. Whenever you attach a server to a network, our system will automatically assign an IPv4 address within your private network to it. Or you can choose a specific IP address within your private network if you prefer.

## IP addresses

You can create Networks for all RFC1918 private IP ranges which are:

* 10.0.0.0/8
* 172.16.0.0/12
* 192.168.0.0/16

## Limits

* Traffic between Cloud Servers inside a Network is private and isolated, but not automatically encrypted. We recommend you use TLS or similar protocols to encrypt sensitive traffic.
* The first IP address of your Network IP range is reserved
* The network and broadcast IP addresses of a network are reserved. For example in 10.0.0.0/24 you cannot use 10.0.0.0 and 10.0.0.255.
* The special private IP Address 172.31.1.1 is reserved. This IP address is being used as a default gateway of your servers public network interface.
* You can attach up to 100 servers to a Network
* Every server can have up to 5 alias IPs in addition to its private main IP
* You can attach a Cloud Server to up to three Networks at the same time.





