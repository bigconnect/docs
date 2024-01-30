# Enabling HTTPS

To secure communication within BigConnect Pipelines, enable HTTPS for the following components:\
\
**BigConnect Pipelines**\
\
Enable HTTPS for BigConnect Pipelines to secure the communication to the BigConnect Pipelines UI and REST API and to use the BigConnect Pipelines as an authoring BigConnect Pipelines in the Control Hub Pipeline Designer.\
\
For Control Hub cloud, an authoring BigConnect Pipelines must use the HTTPS protocol because Control Hub cloud also uses the HTTPS protocol. For Control Hub on-premises, an authoring BigConnect Pipelines must use the same protocol as the Control Hub on-premises installation.\
\
**Cluster pipelines**\
\
If you run cluster pipelines, enable HTTPS for cluster pipelines to secure the communication between the gateway and worker nodes in the cluster.\
\
**Pipeline stages that connect to external systems**\
\
During pipeline development, developers can enable specific stages to use SSL/TLS to secure the communication with an external system. For example, if designing a pipeline that writes to a Cassandra cluster enabled for HTTPS, the developer must configure the Cassandra destination to use SSL/TLS to connect to Cassandra.\
\
For information about enabling HTTPS for pipeline stages, see SSL/TLS Encryption.

By default, BigConnect Pipelines and cluster pipelines use the HTTP protocol. StreamSets recommends using HTTPS in a production environment. HTTPS requires SSL/TLS certificates.
