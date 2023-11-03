# Datagrid in Applications

This section details how to use the underlying Data Grid Hazelcast instance in MWARE AS within application code.

### Accessing the Hazelcast instance via JNDI

By default, the JNDI name of the hazelcast instance is `payara/Hazelcast`, though this can be altered using the `set-hazelcast-configuration` command. You will need to import the following packages into your Java class:

```java
import com.hazelcast.core.HazelcastInstance;
import javax.naming.Context;
import javax.naming.InitialContext;
```

To import the Hazelcast package, you will need to set add the Public API artefact as a dependency in the project’s _pom.xml_ file (in case of Maven projects), or have the Hazelcast JAR set as a project dependency (if using a non-Maven based project). Here’s the Maven dependency:

```xml
<dependency>
  <groupId>fish.payara.api</groupId>
  <artifactId>payara-api</artifactId>
  <version>${page-version}</version>
</dependency>
```

The Hazelcast JAR (for non-Maven projects), can either be downloaded from the [Hazelcast website](http://hazelcast.org/download/), or you can make use of the JAR that is packaged in MWARE AS. The JAR packaged with Payara can be found at `<Payara Installation Directory>/glassfish/modules/hazelcast.jar`.

The following code sample will initialize a `HazelcastInstance` variable in the instance embedded in Payara:

```java
Context ctx = new InitialContext();
HazelcastInstance instance = (HazelcastInstance) ctx.lookup("payara/Hazelcast");
```

### Using Hazelcast for Web and EJB Container Persistence

Hazelcast can be used as the persistence provider for both the Web and EJB containers.

#### Setting Hazelcast as the Persistence provider through the Admin Console

To set up Hazelcast for persistence:

1. Select the instance’s configuration from the page tree:
2.  Select _"Availability Service"_ to view the _"Availability Service"_ page:

    ![Availability Service](https://docs.payara.fish/community/docs/\_images/hazelcast/hazelcast-admin-console-select-availability-service.png)

**Setting Web Persistence**

1.  Open the "Web Container Availability" tab, and select "Hazelcast" from the Persistence Type drop-down menu:

    ![Web Persistence](https://docs.payara.fish/community/docs/\_images/hazelcast/hazelcast-admin-console-availability-enable-web-persistence.png)
2. Save the changes.

**Setting EJB Persistence**

1.  Open the "EJB Container Availability" tab and select "Hazelcast" from the Persistence Type drop-down menu:

    ![EJB Persistence](https://docs.payara.fish/community/docs/\_images/hazelcast/hazelcast-admin-console-availability-enable-ejb-persistence.png)
2. Save the changes.

#### Setting Hazelcast as the Persistence provider using Asadmin

To configure the persistence provider with the _asadmin_ CLI, you have to use the _set_ command.

**For the Web Container**

To set Hazelcast as the persistence provider of the Web Container, run the following command:

```shell
asadmin> set ${cluster-config}.availability-service.web-container-availability.persistence-type=hazelcast
```

**For the EJB Container**

To set Hazelcast as the persistence provider of the EJB Container, run the following command:

```shell
asadmin> set ${cluster-config}.availability-service.ejb-container-availability.sfsb-ha-persistence-type=hazelcast
```
