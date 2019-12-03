# Public host name limitations

Public host name capability is limited to the host. You cannot define public host names for components.

The public host name capability is currently available on the host level, and not the host-component level. For example, if you have a host which has a NameNode and ResourceManager on it, that host's public host name will be used in the configuration for both the NameNode and the ResourceManager. It is not possible to specify a public host name for only the NameNode or only the ResourceManager. It is only possible to specify the public host name for the physical host that is running both components.

