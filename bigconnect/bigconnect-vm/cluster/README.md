# Cluster

This section provides an overview of your Swarm environment. You can view information about environments and nodes, view cluster information, and configure environment-specific settings.

## Cluster status

This section describes the cluster's basic configuration, including the number of nodes, the Docker API version, and the total CPU and memory available. A link to the cluster visualizer is also included.

![](<../../../.gitbook/assets/image (375).png>)

## Nodes

Lists all of the nodes in the cluster along with a summary of each node, including:

* The role of the node.
* The number of CPUs and memory available on the node.
* The Docker Engine version running on the node.
* The node's IP address.
* The status and availability of the node.

![](<../../../.gitbook/assets/image (329).png>)

Clicking on an individual node's name will take you to an overview page for that node.

## Node overview

### Host Details

This section describes the node's basic configuration, including the hostname, OS information, and total CPU and memory.

![](<../../../.gitbook/assets/image (360).png>)

### Engine Details

Information such as the Docker version and the available volume and network plugins helps you to understand more about the Docker engine running on your node.

![](<../../../.gitbook/assets/image (344).png>)

### PCI Devices and Physical Disks

These sections list the available PCI devices and physical disks on the node.&#x20;

![](<../../../.gitbook/assets/image (361).png>)

### Node Details

Here you'll find details about the node's configuration as part of the cluster. You can view the role, set the availability status of the node, view the current status and apply labels to the node.

![](<../../../.gitbook/assets/image (351).png>)

The availability status of the node can be one of the following:

* **Active**: the scheduler can assign tasks to the node.
* **Drain**: the scheduler doesn't assign new tasks to the node. The scheduler shuts down any existing tasks and schedules them on an available node. Use this mode to take a node down for maintenance.
* **Pause**: the scheduler doesn't assign new tasks to the node, but existing tasks remain running.
