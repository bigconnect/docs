# Details

This page provides information about the Docker Swarm cluster for the selected environment. The page is split into the following sections: Cluster status and Nodes, with clicking a node name taking you to the Node overview for that node.

This page is only available on Docker Swarm environments.

### Cluster status <a href="#cluster-status" id="cluster-status"></a>

This section describes the cluster's basic configuration, including the number of nodes, the Docker API version, and the total CPU and memory available. A link to the cluster visualizer is also included.

<figure><img src="https://docs.portainer.io/~gitbook/image?url=https%3A%2F%2Fcontent.gitbook.com%2Fcontent%2F3roAECKwSfowCXNw8RZR%2Fblobs%2FyydyPJuPyjvuCoGttLKv%2F2.15-swarm-clusterstatus.png&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=391fbf2f&#x26;sv=2" alt=""><figcaption></figcaption></figure>

### Nodes <a href="#nodes" id="nodes"></a>

Lists all of the nodes in the cluster along with a summary of each node, including:

* The role of the node.
* The number of CPUs and memory available on the node.
* The Docker Engine version running on the node.
* The node's IP address.
* The status and availability of the node.

<figure><img src="https://docs.portainer.io/~gitbook/image?url=https%3A%2F%2Fcontent.gitbook.com%2Fcontent%2F3roAECKwSfowCXNw8RZR%2Fblobs%2F3lR7hZCdyo8iHDpn17XF%2F2.15-swarm-nodes.png&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=ac4dd7a6&#x26;sv=2" alt=""><figcaption></figcaption></figure>

Clicking on an individual node's name will take you to an overview page for that node.

### Node overview <a href="#node-overview" id="node-overview"></a>

#### Host Details <a href="#host-details" id="host-details"></a>

This section describes the node's basic configuration, including the hostname, OS information, and total CPU and memory.

<figure><img src="https://docs.portainer.io/~gitbook/image?url=https%3A%2F%2Fcontent.gitbook.com%2Fcontent%2F3roAECKwSfowCXNw8RZR%2Fblobs%2F4PuwBifkYsgdc3cRlGxY%2F2.15-swarm-nodedetail.png&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=3a11fec6&#x26;sv=2" alt=""><figcaption></figcaption></figure>

#### Engine Details <a href="#engine-details" id="engine-details"></a>

Information such as the Docker version and the available volume and network plugins helps you to understand more about the Docker engine running on your node.

<figure><img src="https://docs.portainer.io/~gitbook/image?url=https%3A%2F%2Fcontent.gitbook.com%2Fcontent%2F3roAECKwSfowCXNw8RZR%2Fblobs%2FdgBmYLMSLG6ziejNe3Sw%2F2.15-swarm-nodedetail-engine.png&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=37a90349&#x26;sv=2" alt=""><figcaption></figcaption></figure>

#### PCI Devices and Physical Disks <a href="#pci-devices-and-physical-disks" id="pci-devices-and-physical-disks"></a>

These sections list the available PCI devices and physical disks on the node.

These sections are only visible when host management features are enabled for the cluster.

<figure><img src="https://docs.portainer.io/~gitbook/image?url=https%3A%2F%2Fcontent.gitbook.com%2Fcontent%2F3roAECKwSfowCXNw8RZR%2Fblobs%2FETYuOZTsebulEW8iAc9b%2F2.15-docker-host-pci.png&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=7dcf975d&#x26;sv=2" alt=""><figcaption></figcaption></figure>

#### Node Details <a href="#node-details" id="node-details"></a>

Here you'll find details about the node's configuration as part of the cluster. You can view the role, set the availability status of the node, view the current status and apply labels to the node.

<figure><img src="https://docs.portainer.io/~gitbook/image?url=https%3A%2F%2Fcontent.gitbook.com%2Fcontent%2F3roAECKwSfowCXNw8RZR%2Fblobs%2FdJIW8sDu1L13Uf7RSKFB%2F2.15-swarm-nodedetail-detail.png&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=735152e1&#x26;sv=2" alt=""><figcaption></figcaption></figure>
