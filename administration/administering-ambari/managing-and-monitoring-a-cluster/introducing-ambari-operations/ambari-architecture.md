# Ambari architecture

Simple overview of the Ambari Server, Agents and Web architecture.

The Ambari Server collects data from across your cluster. Each host has a copy of the Ambari Agent, which allows the Ambari Server to control each host.

![Ambari Architecture](../../../../.gitbook/assets/image%20%2820%29.png)

Ambari Web is a client-side, JavaScript application that calls the Ambari REST API \(accessible from the Ambari Server\) to access cluster information and perform cluster operations. After authenticating to Ambari Web, the application authenticates to the Ambari Server. Communication between the browser and server occurs asynchronously using the REST API.

The Ambari Web UI periodically accesses the Ambari REST API, which resets the session timeout. Therefore, Ambari Web sessions do not timeout automatically by default. You can configure Ambari to timeout after a period of inactivity.



