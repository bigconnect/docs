# Domain Data Grid And Hazelcast

### Concepts <a href="#concepts" id="concepts"></a>

The Domain Data Grid provides an in-memory data structure that is distributed amongst all the MWARE AS instances within a MWARE AS Domain. The Data Grid is highly available and highly scalable and enables in-memory data storage and sharing between all MWARE AS instances in a domain. The Domain Data Grid is used as the backing store for web session data, Stateful Session Bean state, JCache caches, Clustered Singletons, and Single Sign On state as well as internal MWARE AS domain data like performance metrics and traces. In addition Persistent EJB Timers can also be configured to be stored in the grid. The Domain Data Grid also enables light-weight messaging between server instances and is the foundation of the CDI Clustered Event Bus.

The Domain Data Grid greatly simplifies traditional clustering as by default it is always on, always available and usually requires no special configuration.

The Domain Data Grid has been designed to work out of the box in a variety of network topologies and usually requires zero configuration in LAN, Amazon AWS, Google Cloud Platform and Microsoft Azure.

#### High Availability

The Domain Data Grid achieves high availability for data storage by maintaining a replica on another MWARE AS Instance of any data placed into the Domain Data Grid. This replica will be placed, as a preference, on an instance that is not running on the same host as the primary data item. If the instance holding the primary copy of a piece of data fails then the backup copy will become the primary and another instance will be selected to hold a back up of the data.

#### Scalability

The Domain Data Grid provides high scalability for data placed into the Data Grid. As a primary and a secondary copy of any data item placed into the grid the more instances that are added to the MWARE AS Domain the more JVM heap is available for storage of data in the grid. The Domain Data Grid can scale to 10s and 100s of instances allowing large data sets to be stored in memory. Read and Write performance for data in the grid is not dependent on the number of MWARE AS instances in the domain providing scalability to grids containing large numbers of MWARE AS instances.

#### Elasticity

The Domain Data Grid does require configuring with a specific number of MWARE AS instances. MWARE AS instances can be added and removed and the Data Grid will shrink and grow as required. This meets the needs of cloud and container based architectures which can scale up or scale down the number of MWARE AS instances handling requests.
