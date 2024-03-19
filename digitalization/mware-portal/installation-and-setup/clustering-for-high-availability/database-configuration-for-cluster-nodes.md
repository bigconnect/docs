# Database Configuration for Cluster Nodes

At minimum, nodes in a DXP cluster must be [configured to share the same database](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/example-creating-a-simple-dxp-cluster#configure-cluster-link-and-the-other-server-connections) (or database cluster) in order to function. Optimizations discussed here can further improve database performance for a DXP cluster and should be considered with consideration for your usage of DXP and expected transaction volume.

WARNING

Clustering will not work with the embedded HSQL database.

### [Read-Writer Database Configuration](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/database-configuration-for-cluster-nodes#read-writer-database-configuration) <a href="#read-writer-database-configuration" id="read-writer-database-configuration"></a>

To improve database performance, you can use a read-writer database configuration. Instead of using the same data source for read and read-write operations, this strategy uses a separate data source for each operation type. DXP’s Aspect Oriented Programming (AOP) transaction infrastructure directs read transactions to the read data source and read-write transactions to the write data source.

![Read-Writer Database Interaction](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/database-configuration-for-cluster-nodes/images/01.png)

Connections to separate read and read-write [data sources](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#JDBC) are configured using JDBC or JNDI [Portal Properties](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) (e.g., in a [`portal-ext.properties` file](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties)), as explained in the following sections. The data sources should use separate instances of the DXP database, where the read-write database instance is replicated to the read database instance.

#### [JDBC](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/database-configuration-for-cluster-nodes#jdbc) <a href="#jdbc" id="jdbc"></a>

Follow these steps to edit your `portal-ext.properties` file to connect directly to separate read and write data sources using [JDBC](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/installing-liferay/configuring-a-database):

1.  Configure JDBC connections to separate read and write data sources. See the [JDBC properties reference](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#JDBC) for more information. Here’s an example:

    ```properties
    jdbc.read.driverClassName=[place your driver name here]
    jdbc.read.url=[place the URL to your "read" database here]
    jdbc.read.username=[place your user name here]
    jdbc.read.password=[place your password here]

    jdbc.write.driverClassName=[place your driver name here]
    jdbc.write.url=[place the URL to your "read-write" database here]
    jdbc.write.username=[place your user name here]
    jdbc.write.password=[place your password here]
    ```

    NOTE

    Liferay uses HikariCP as the connection pool provider.

    For example JDBC connection values, please see [Database Templates](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/database-templates).
2.  Configure the read and write pool timeouts and size settings

    ```properties
    jdbc.read.connectionTimeout=30000
    jdbc.read.idleTimeout=600000
    jdbc.read.maximumPoolSize=100
    jdbc.read.maxLifetime=0
    jdbc.read.minimumIdle=10
    jdbc.read.registerMbeans=true

    jdbc.write.connectionTimeout=30000
    jdbc.write.idleTimeout=600000
    jdbc.write.maximumPoolSize=100
    jdbc.write.maxLifetime=0
    jdbc.write.minimumIdle=10
    jdbc.write.registerMbeans=true
    ```
3.  **(This step is only necessary for DXP 7.4 U69 and below)** Configure DXP to use the write data source (the data source whose prefix is `jdbc.write.`) to create the [Counter](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Counter) data source. A separate data source is always dedicated to the counter.

    ```properties
    counter.jdbc.prefix=jdbc.write.
    ```
4. Optionally validate the data connections to make sure bad connections are handled gracefully.
5.  **(This step is only necessary for DXP 7.3 U22, DXP 7.4 U65 and below)** Enable the read-writer database configuration by copying the default [`spring.infrastructure.configs` Portal Properties](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Spring) to your `portal-ext.properties` file and adding the following Spring configuration file paths to them.

    Add to `spring.infrastructure.configs`:

    ```log
    META-INF/dynamic-data-source-infrastructure-spring.xml
    ```

    For more information, see the [Spring configuration Portal Properties](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Spring).

#### [JNDI](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/database-configuration-for-cluster-nodes#jndi) <a href="#jndi" id="jndi"></a>

Edit your `portal-ext.properties` file following these steps to connect to your read and write data sources on your app server using JNDI:

1.  Set your read and write JNDI data source user names and passwords.

    ```properties
    jdbc.read.jndi.name=[place your "read" data source JNDI name here]

    jdbc.read.username=*[place your user name here]
    jdbc.read.password=[place your password here]

    jdbc.write.jndi.name=[place your "read-write" data source JNDI name here]

    jdbc.write.username=[place your user name here]
    jdbc.write.password=[place your password here]
    ```
2.  Configure the read and write pool timeouts and size settings

    ```properties
    jdbc.read.connectionTimeout=30000
    jdbc.read.idleTimeout=600000
    jdbc.read.maximumPoolSize=100
    jdbc.read.maxLifetime=0
    jdbc.read.minimumIdle=10
    jdbc.read.registerMbeans=true

    jdbc.write.connectionTimeout=30000
    jdbc.write.idleTimeout=600000
    jdbc.write.maximumPoolSize=100
    jdbc.write.maxLifetime=0
    jdbc.write.minimumIdle=10
    jdbc.write.registerMbeans=true
    ```
3.  **(This step is only necessary for DXP 7.4 U69 and below)** Configure DXP to use the write data source (the data source whose prefix is `jdbc.write.`) to create the [Counter](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Counter) data source. A separate data source is always dedicated to the counter.

    ```properties
    counter.jdbc.prefix=jdbc.write.
    ```
4. Optionally validate the data connections to make sure bad connections are handled gracefully.
5.  **(This step is only necessary for DXP 7.4 U65 and below or DXP 7.3 U22 and below)** Enable the read-writer database configuration by copying the default [`spring.infrastructure.configs` Portal Properties](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Spring) to your `portal-ext.properties` file and adding the following Spring configuration file paths to them.

    Add to `spring.infrastructure.configs`:

    ```log
    META-INF/dynamic-data-source-infrastructure-spring.xml
    ```

    For more information, see the [Spring configuration Portal Properties](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Spring).

DXP uses a read data source, a write data source, and a counter data source the next time it starts.

### [Database Replication](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/database-configuration-for-cluster-nodes#database-replication) <a href="#database-replication" id="database-replication"></a>

Using a database cluster improves fault tolerance and DXP performance. Database cluster instances must be stay in sync. Replication is the process of copying changed data and changed schema from one database instance to another. All [supported databases](https://help.liferay.com/hc/en-us/articles/360049238151) support replication. If you’re using a database cluster, set up the databases for replication by following the database vendor’s instructions.

\
