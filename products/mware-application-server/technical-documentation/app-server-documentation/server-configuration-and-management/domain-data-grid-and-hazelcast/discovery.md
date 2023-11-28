---
description: Domain Data Grid Discovery Modes
---

# Discovery

The Domain Data Grid has the concept of auto-discovery. When a MWARE AS instance starts it must discover other Data Grid nodes so that it can join the grid. Once an instance has connected to a MWARE AS Instance that is already a member of the grid then it can become a member of the grid.

### Discovery Modes

The following are the discovery mechanisms or modes that are included by default in MWARE AS.

#### Domain Discovery Mode

The Domain discovery mode is the default discovery mode in MWARE AS. In this mode when a MWARE AS instance starts it uses information encoded into the `domain.xml` describing the node IP Address and ports of the other servers in the domain and the DAS to connect to an existing member of the Data Grid. For a MWARE AS instance the first server it will try to connect to is the DAS if that fails it will then try each of the other instances in turn. When the DAS starts it tries to connect to each instance in turn.

#### TCP-IP Discovery Mode

The TCP-IP discovery mode is an alternative discovery mode. In this mode the user specifies a comma separated list of IP addresses and ports that a MWARE AS instance should try to find members of the data grid at upon boot.

#### Multicast Discovery Mode

The Multicast discovery mode is an alternative discovery mode. In this mode the user specifies the multicast group and multicast port used to find other members of the Data Grid. When a MWARE AS instance configured with multicast discovery starts it broadcasts a message on the multicast group and port to discover other members of the data grid.

#### DNS Discovery Mode

The DNS discovery mode is an alternative discovery mode, closely related to the TCPIP discovery mode. In this mode the user specifies a comma separated list of DNS names and ports that a MWARE AS should try to find members of the data grid at upon boot.

#### Kubernetes Discovery Mode

The Kubernetes discovery mode is an alternative discovery mode intended for use when running in a Kubernetes environment. In this mode, the user specifies the Kubernetes service name and namespace that MWARE AS should look to find other data grid members in - MWARE AS will then contact the Kubernetes master upon boot to obtain the list of IP addresses of any other running instances under the specified namespace and service name.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: default-cluster
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: view
subjects:
- kind: ServiceAccount
  name: default
  namespace: default
```

### Configuration in the Administration Console

The domain discovery mode can be configured in the administration console under the **Data Grid** menu option. The discovery mode that is set gets to be applied in a domain-wide manner.

![Domain Discovery Mode](https://docs.payara.fish/community/docs/\_images/domain-datagrid/discovery-mode.png)

### Configuration via Asadmin CLI

The discovery mode can be set via the asadmin CLI, depending on the specific mode to be selected, by running the following commands

#### Set TCPIP Mode

When setting TCPIP mode the list of IP addresses and ports of all Data Grid members must be specified:

```shell
asadmin> set-hazelcast-configuration --clustermode tcpip --tcpipmembers 192.168.0.104:4900,192.168.0.105:5900
```

#### Set Domain Mode

```shell
asadmin> set-hazelcast-configuration --clustermode domain
```

#### Set Multicast Mode

When setting multicast mode the multicast group and multicast port must be also specified.

```shell
asadmin> set-hazelcast-configuration --clustermode multicast --multicastgroup 224.2.2.4 --multicastport 55000
```

#### Set DNS Mode

When setting DNS mode the list of DNS names and ports of all Data Grid members must be specified.

```shell
asadmin> set-hazelcast-configuration --clustermode dns --dnsmembers localhost:4900,www.example.com:5900
```

#### Set Kubernetes Mode

When setting Kubernetes mode the service name and namespace must be specified using the `--kubernetesservicename` and `--kubernetesnamespace` options respectively.

```shell
asadmin> set-hazelcast-configuration --clustermode kubernetes --kubernetesServiceName payara --kubernetesNamespace default
```
