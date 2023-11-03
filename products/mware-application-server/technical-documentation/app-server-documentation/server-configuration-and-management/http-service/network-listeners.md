# Network Listeners

A network listener is a listener socket that has an IP address, port number, protocol, as well as various other configuration options. A listener can listen on a specific port on all network interfaces by using the IP `0.0.0.0`, and a configured port.

A virtual server will then be used to select a combination of network listeners to use for an application. For example, MWARE AS has two default network listeners for user deployed applications: an insecure listener on port 8080 (`http-listener-1`), and a secure listener on port 8181 (`http-listener-2`).

### Configuration

Existing network listeners can be configured from the admin console or with asadmin commands. The network listener configurations can be found in the admin console under **Configurations** → **your-config** → **Network Config** → **Network Listeners**.

![Configuration Page Location](https://docs.payara.fish/community/docs/\_images/http/network-listener-config.png)

The following fields can be configured for network listeners:

***

#### Name

The unique name of the network listener. This cannot be edited from the admin console.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.name=value`

***

#### Protocol

The protocol used by the network listener. This cannot be edited from the admin console.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.protocol=${protocol-name}`

***

#### Status

The status of the network listener. This determines whether the listener will be actively listening for requests.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.status=[true/false]`

***

#### JK Enabled

Whether `mod_jk` support should be enabled for this listener. Should only be enabled if `mod_jk` is being explicitly configured on an Apache Web Server in conjunction with MWARE AS.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.jk-enabled=[true/false]`

***

**JK Configuration File**

The location of the `mod_jk` configuration file. Defaults to `${com.sun.aas.instanceRoot}/config/glassfish-jk.properties`.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.jk-configuration-file=/location/to/file`

***

#### Port

The port used by the network listener. The selected port will be used to listen for requests.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.port=value`

***

#### Address

The address used by the network listener. `0.0.0.0` corresponds to all addresses.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.address=value`

***

#### Transport

The transport used by the network listener. This cannot be edited from the admin console. Defaults to `tcp`.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.transport=${transport-name}`

***

#### Thread Pool

The thread pool used by the network listener. This determines the pool of threads used to service requests.

**Asadmin Command:**

`set configs.config.server-config.network-config.network-listeners.network-listener.${network-listener-name}.thread-pool=${thread-pool-name}`

***

### Server Name Indication - SNI

Server Name Indication, SNI for short, allows you to use multiple SSL certificates with the same IP address. When enabled, the server will look for a certificate in the configured keystore using a nickname that matches the host name requested.

SNI can be enabled by configuring a network listener. Existing network listeners can be configured from the admin console or with asadmin commands. The network listener configurations can be found in the admin console under **Configurations → \<your-config> → Network Config → Network Listeners**.

From here, click on the **SSL** tab, and make sure that the **SNI Support** checkbox is ticked.

|   | For SNI to take effect, the network listener must have security enabled by providing a default keypair and certificate, and by checking the **Security** box on the **HTTP** tab. |
| - | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Asadmin Commands

The following asadmin commands are available for administering network listeners instead of using the Admin console. The parameters are all explained in the configuration options above.

#### create-network-listener

The `create-network-listener` command creates a network listener. It requires an already created transport and protocol. For more information on creating protocols, see [protocols](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Server%20Configuration%20And%20Management/HTTP%20Service/Protocols.html). The parameters are all explained in the configuration options above.

**Format**

```
asadmin> create-network-listener [--help]
           [--address address]
           --listenerport listener-port
           [--threadpool thread-pool]
           --protocol protocol
           [--transport transport]
           [--enabled={true|false}]
           [--jkenabled={false|true}]
           [--target target]
           listener-name
```

#### restart-http-listeners

The `restart-http-listeners` commands restarts all HTTP listeners but the `admin-listener` of the targeted instance(s). It is either used with the `--all` flag to target all instances or with a particular target instance, deployment group or cluster following the `--target` parameter. When `target` and `all` parameters are both omitted the default target is the DAS.

When `--all` is used together with a target the target is ignored and the command affects all instances.

**Format**

```
asadmin> restart-http-listeners [--all] [--target target]
```

**Examples**

```
asadmin> restart-http-listeners --all
asadmin> restart-http-listeners --target FlyingFish
asadmin> restart-http-listeners FlyingFish
asadmin> restart-http-listeners
```

First line affects all instances, second line affects only instance named `FlyingFish`, third is a shorthand for the second, last is a shorthand for `restart-http-listeners server` or `restart-http-listeners --target=server`.
