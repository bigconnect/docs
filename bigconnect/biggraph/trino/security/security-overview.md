# Security overview

After the initial installation of your cluster, security is the next major concern for successfully operating Trino. This overview provides an introduction to different aspects of configuring security for your Trino cluster.

### Aspects of configuring security <a href="#aspects-of-configuring-security" id="aspects-of-configuring-security"></a>

The default installation of Trino has no security features enabled. Security can be enabled for different parts of the Trino architecture:

* Securing client access to the cluster
* Securing inside the cluster
* Securing cluster access to data sources

### Suggested configuration workflow <a href="#suggested-configuration-workflow" id="suggested-configuration-workflow"></a>

To configure security for a new Trino cluster, follow this best practice order of steps. Do not skip or combine steps.

1.  **Enable** TLS/HTTPS

    * Work with your security team.
    * Use a load balancer or proxy to terminate HTTPS, if possible.
    * Use a globally trusted TLS certificate.

    Verify this step is working correctly.
2.  **Configure** a a shared secret

    Verify this step is working correctly.
3.  **Enable authentication**

    * Start with password file authentication to get up and running.
    * Then configure your preferred authentication provider, such as LDAP.
    * Avoid the complexity of Kerberos for client authentication, if possible.

    Verify this step is working correctly.
4.  **Enable authorization and access control**

    * Start with file-based rules.
    * Then configure another access control method as required.

    Verify this step is working correctly.

Configure one step at a time. Always restart the Trino server after each change, and verify the results before proceeding.

### Securing client access to the cluster <a href="#securing-client-access-to-the-cluster" id="securing-client-access-to-the-cluster"></a>

Trino clients include the Trino CLI, the Web UI, the JDBC driver, Python, Go, or other clients, and any applications using these tools.

All access to the Trino cluster is managed by the coordinator. Thus, securing access to the cluster means securing access to the coordinator.

There are three aspects to consider:

* Encryption: protecting the integrity of client to server communication in transit.
* Authentication: identifying users and user name management.
* Authorization and access control: validating each user’s access rights.

#### Encryption <a href="#encryption" id="encryption"></a>

The Trino server uses the standard HTTPS protocol and TLS encryption, formerly known as SSL.

#### Authentication <a href="#authentication" id="authentication"></a>

Trino supports several authentication providers. When setting up a new cluster, start with simple password file authentication before configuring another provider.

* Password file authentication
* LDAP authentication
* Salesforce authentication
* OAuth 2.0 authentication
* Certificate authentication
* JSON Web Token (JWT) authentication
* Kerberos authentication

**User name management#**

Trino provides ways to map the user and group names from authentication providers to Trino user names.

* User mapping applies to all authentication systems, and allows for regular expression rules to be specified that map complex user names from other systems (`alice@example.com`) to simple user names (`alice`).
* File group provider provides a way to assign a set of user names to a group name to ease access control.

#### Authorization and access control <a href="#authorization-and-access-control" id="authorization-and-access-control"></a>

Trino’s default method of access control allows all operations for all authenticated users.

To implement access control, use:

* File-based system access control, where you configure JSON files that specify fine-grained user access restrictions at the catalog, schema, or table level.
* Open Policy Agent access control, where you use Open Policy Agent to make access control decisions on a fined-grained level.

In addition, Trino provides an API that allows you to create a custom access control method, or to extend an existing one.

Access control can limit access to columns of a table. The default behavior of a query to all columns with a `SELECT *` statement is to show an error denying access to any inaccessible columns.

You can change this behavior to silently hide inaccessible columns with the global property `hide-inaccessible-columns` configured in Config properties:

```
hide-inaccessible-columns = true
```

### Securing inside the cluster <a href="#securing-inside-the-cluster" id="securing-inside-the-cluster"></a>

You can secure the internal communication between coordinator and workers inside the clusters.

Secrets in properties files, such as passwords in catalog files, can be secured with secrets management.

### Securing cluster access to data sources <a href="#securing-cluster-access-to-data-sources" id="securing-cluster-access-to-data-sources"></a>

Communication between the Trino cluster and data sources is configured for each catalog. Each catalog uses a connector, which supports a variety of security-related configurations.

More information is available with the documentation for individual connectors.

Secrets management can be used for the catalog properties files content.
