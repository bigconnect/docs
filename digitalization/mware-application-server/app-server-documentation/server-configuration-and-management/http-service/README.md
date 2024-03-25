# HTTP Service

HTTP in MWARE AS is composed of several configuration layers:

* Transport. The default transport is `tcp`.
* An HTTP protocol. One of the default protocols is `http-listener-1` (which has the same name as the `http-listener-1` network listener).
* A network listener. One of the default listeners is `http-listener-1`.
* A virtual server. The default virtual server is `server`.
* A web server. [Apache HTTP Server ](https://httpd.apache.org/)and [NGINX](https://nginx.org/en/docs/) are both supported for reverse-proxy, load balancing, SSO, authentication and static resource caching
* Native high-availability cluster configuration for [Apache HTTP Server](https://httpd.apache.org/)

The configuration for each of these objects can be found in their respective pages:

* Transport
* Protocols
* Network Listeners
* Virtual Servers
