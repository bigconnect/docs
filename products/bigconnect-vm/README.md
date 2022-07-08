# BigConnect Virtual Machine

BigConect VM hides the complexity of managing containers behind an easy-to-use UI. By removing the need to use the CLI, write YAML or understand manifests, BigConect VM makes deploying apps and troubleshooting problems so easy that anyone can do it.

BigConect VM consists of two elements: the Server and the Agent. Both run as lightweight containers on your existing containerized infrastructure. The Agent should be deployed to each node in your cluster and configured to report back to the Server container.

A single BigConect VM Server will accept connections from any number of Agents, providing the ability to manage multiple clusters from one centralized interface. To do this, the Server container requires data persistence. The Agents are stateless, with data being shipped back to the Server container.

### The Agent

In standard deployments, the central BigConect VM Server instance and any environments it manages are assumed to be on the same network, that is, the Server and Agents are able to seamlessly communicate with one another. However, in configurations where the remote environments are on a completely separate network to the BigConect VM Server, say, across the internet, historically we would have been unable to centrally manage these devices.

### Security and compliance

BigConect VM runs exclusively on your servers, within your network, behind your own firewalls. As a result, we do not currently hold any SOC or PCI/DSS compliance because we do not host any of your infrastructure. You can even run BigConect VM completely disconnected (air-gapped) without any impact on functionality.
