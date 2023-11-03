# Encryption

Encryption of the data stored within the data grid of Hazelcast has previously required a **Hazelcast Enterprise** subscription. While this remains an option for those who would like the additional features it provides (such as WAN replication), Payara Server now includes its own encryption implementation that provides this feature to you without an additional subscription.

### Encryption Mechanism <a href="#encryption-mechanism" id="encryption-mechanism"></a>

This feature provides encryption of the data that’s being stored in-memory via the Hazelcast data grid, providing end-to-end encryption of the data across all instances in the domain. End-to-end encryption is encryption of data both at rest (in storage) and in transit (during communication).

Payara Server uses a symmetric key to perform the encryption of the data within the data grid store. This means that the same key is used by each member of the data grid to encrypt and decrypt the data.

This key is generated and protected using the master password and a key derivation function. The key itself is a 256-bit AES key, utilising an initialisation vector and a salt for each call of encryption to help prevent any statistical pattern recognition.

### Encryption Targets

The following data stores are encrypted:

* Web Session Persistence
* Stateful Session Bean Persistence
* Request Traces
* Historic Health Checks

### Setup

This section details the necessary steps to set up and enable encryption of the data grid and the various data stores.

Documentation around configuration of the data sources for encryption assumes that you’ve already generated the encryption key and enabled encryption of the data grid.

#### Generating an Encryption Key

The key to be used by Payara Server to perform the actual encryption and decryption of data can be generated using the following command:

```shell
asadmin > generate-encryption-key
```

This command generates a key using the master password – the same one used to access the key store used by Payara Server. Similar to the `change-master-password` command, this command requires you to shut down the DAS first.

The default value for the master password is `changeit`. You can also provide the master password via a password file like so:

```shell
asadmin -W /path/to/passwordfile.txt generate-encryption-key
```

That will generate a new file with the following contents:

```
AS_ADMIN_MASTERPASSWORD=changeit
```

#### Enabling Encryption in Hazelcast

Enabling encryption of the data grid itself is controlled by the `set-hazelcast-configuration` asadmin command, with the `--encryptdatagrid` parameter:

```shell
asadmin set-hazelcast-configuration --encryptdatagrid true
```

This is a domain-wide setting, and thus means that hybrid setups where encryption is enabled only on specific clusters within a domain is not supported.

#### Web Session Availability Configuration

Encrypting web session availability data requires no extra actions on top of configuring the Web Container Availability Persistence Type to _hazelcast_ (which it is by default).

```shell
asadmin set configs.config.server-config.availability-service.web-container-availability.persistence-type=hazelcast
```

#### Stateful Session Bean Configuration

Encrypting Stateful Session Beans (SFSB) availability data requires no extra actions on top of configuring the EJB Container Availability HA and/or SFSB Persistence Type to `hazelcast`:

```shell
asadmin set configs.config.server-config.availability-service.ejb-container-availability.sfsb-ha-persistence-type=hazelcast
asadmin set configs.config.server-config.availability-service.ejb-container-availability.sfsb-persistence-type=hazelcast
```

\[\[Request Tracing Service-configuration]] === Request Tracing Configuration

No additional actions necessary.

#### HealthCheck Configuration

No additional actions necessary.
