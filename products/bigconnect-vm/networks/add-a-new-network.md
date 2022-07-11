# Add a new network

From the menu select **Networks** then click **Add network**. Define the new network, using the table below as a guide.

* **Name**: Give the network a descriptive name.&#x20;
* **Driver**: Define the you will use.
* **Driver options**: Set in place any options related to your network driver, if required.&#x20;
* **IPv4 Network configuration**: Define IPv4 range, subnet, gateway and exclude IP. If no information is entered here, Docker will automatically assign an IPv4 range.&#x20;
* **IPv6 Network configuration**: Define IPv6 range, subnet, gateway and exclude IP. If no information is entered here, Docker will automatically assign an IPv6 range.&#x20;
* **Labels**: Add labels to the network.&#x20;
* **Isolated network**: Toggle this option on to isolate any containers created in this network to this network only, with no inbound or outbound connectivity.&#x20;
* **Enable manual container attachment**: Toggle this option on to allow users to attach the network to running containers.&#x20;
* **Deployment**: On multi-node clusters, select the node where the network will be created.

![](<../../../.gitbook/assets/image (770).png>)

When you're finished, click **Create the network**.
