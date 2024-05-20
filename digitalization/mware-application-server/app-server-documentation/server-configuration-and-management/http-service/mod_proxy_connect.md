# mod\_proxy\_connect

This module _requires_ the service of `mod_proxy`. It provides support for the `CONNECT` HTTP method. This method is mainly used to tunnel SSL requests through proxy servers.

Thus, in order to get the ability of handling `CONNECT` requests, `mod_proxy` and `mod_proxy_connect` have to be present in the server.

CONNECT is also used when the server needs to send an HTTPS request through a forward proxy. In this case the server acts as a CONNECT client. This functionality is part of `mod_proxy` and `mod_proxy_connect` is not needed in this case.

#### Warning

Do not enable proxying until you have [secured your server](https://httpd.apache.org/docs/2.4/mod/mod\_proxy.html#access). Open proxy servers are dangerous both to your network and to the Internet at large.

#### Topics

* [Request notes](https://httpd.apache.org/docs/2.4/mod/mod\_proxy\_connect.html#notes)

#### Directives

* [AllowCONNECT](https://httpd.apache.org/docs/2.4/mod/mod\_proxy\_connect.html#allowconnect)

#### Bugfix checklist

* [httpd changelog](https://www.apache.org/dist/httpd/CHANGES\_2.4)
* [Known issues](https://bz.apache.org/bugzilla/buglist.cgi?bug\_status=\_\_open\_\_\&list\_id=144532\&product=Apache%20httpd-2\&query\_format=specific\&order=changeddate%20DESC%2Cpriority%2Cbug\_severity\&component=mod\_proxy\_connect)
* [Report a bug](https://bz.apache.org/bugzilla/enter\_bug.cgi?product=Apache%20httpd-2\&component=mod\_proxy\_connect)

#### See also

* [`mod_proxy`](https://httpd.apache.org/docs/2.4/mod/mod\_proxy.html)
* [Comments](https://httpd.apache.org/docs/2.4/mod/mod\_proxy\_connect.html#comments\_section)

[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_proxy\_connect.html#page-header)

### Request notes

[`mod_proxy_connect`](https://httpd.apache.org/docs/2.4/mod/mod\_proxy\_connect.html) creates the following request notes for logging using the `%{VARNAME}n` format in [`LogFormat`](https://httpd.apache.org/docs/2.4/mod/mod\_log\_config.html#logformat) or [`ErrorLogFormat`](https://httpd.apache.org/docs/2.4/mod/core.html#errorlogformat):

proxy-source-portThe local port used for the connection to the backend server.[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_proxy\_connect.html#page-header)

### AllowCONNECT Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description)     | Ports that are allowed to `CONNECT` through the proxy                                                                                      |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)               | `AllowCONNECT port[-port] [port[-port]] ...`                                                                                               |
| [Default:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Default)             | `AllowCONNECT 443 563`                                                                                                                     |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)             | server config, virtual host                                                                                                                |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)               | Extension                                                                                                                                  |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)               | mod\_proxy\_connect                                                                                                                        |
| [Compatibility:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Compatibility) | Moved from [`mod_proxy`](https://httpd.apache.org/docs/2.4/mod/mod\_proxy.html) in Apache 2.3.5. Port ranges available since Apache 2.3.7. |

The `AllowCONNECT` directive specifies a list of port numbers or ranges to which the proxy `CONNECT` method may connect. Today's browsers use this method when a `https` connection is requested and proxy tunneling over HTTP is in effect.

By default, only the default https port (`443`) and the default snews port (`563`) are enabled. Use the `AllowCONNECT` directive to override this default and allow connections to the listed ports only.
