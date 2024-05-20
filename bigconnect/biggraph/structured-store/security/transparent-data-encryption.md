# Transparent Data Encryption

Transparent data encryption (TDE) allows users to encrypt their data at rest.

When Ignite persistence is turned on, encryption can be enabled per cache/table, in which case the following data will be encrypted:

* Data on disk
* WAL records

If you enable cache/table encryption, Ignite will generate a key (called _cache encryption key_) and will use this key to encrypt/decrypt the data in the cache. The cache encryption key is held in the system cache and cannot be accessed by users. When the cache encryption key is sent to other nodes or saved to disk (when the node goes down), it is encrypted using the _master key_. The master key must be specified by the user in the configuration.

The _same_ master key must be specified via the configuration in every server node. One way to ensure you’re using the same key is to copy the JKS file from one node to the other nodes. If you try to enable TDE using different keys, the nodes with the different key will not be able to join the cluster (will be rejected based on differing digests).

Ignite uses JDK-provided encryption algorithms: "AES/CBC/PKCS5Padding" to encrypt WAL records and "AES/CBC/NoPadding" to encrypt data pages on disk.&#x20;

### Configuration

To enable encryption in the cluster, provide a master key in the configuration of each server node. A configuration example is shown below.

```xml
<bean class="org.apache.ignite.configuration.IgniteConfiguration">
    <!-- We need to configure EncryptionSpi to enable encryption feature. -->
    <property name="encryptionSpi">
        <!-- Using EncryptionSpi implementation based on java keystore. -->
        <bean class="org.apache.ignite.spi.encryption.keystore.KeystoreEncryptionSpi">
            <!-- Path to the keystore file. -->
            <property name="keyStorePath" value="ignite_keystore.jks"/>
            <!-- Password for keystore file. -->
            <property name="keyStorePassword" value="mypassw0rd"/>
            <!-- Name of the key in keystore to be used as a master key. -->
            <property name="masterKeyName" value="ignite.master.key"/>
            <!-- Size of the cache encryption keys in bits. Can be 128, 192, or 256 bits.-->
            <property name="keySize" value="256"/>
        </bean>
    </property>
    <property name="cacheConfiguration">
        <bean class="org.apache.ignite.configuration.CacheConfiguration">
            <property name="name" value="encrypted-cache"/>
            <property name="encryptionEnabled" value="true"/>
        </bean>
    </property>
    <property name="dataStorageConfiguration">
        <bean class="org.apache.ignite.configuration.DataStorageConfiguration">
            <property name="encryptionConfiguration">
                <bean class="org.apache.ignite.configuration.EncryptionConfiguration">
                    <!-- Set re-encryption rate limit to 10.3 MB/s. -->
                    <property name="reencryptionRateLimit" value="10.3"/>
                </bean>
            </property>
        </bean>
    </property>
</bean>
```

When the master key is configured, you can enable encryption for a cache as follows:

```xml
<property name="cacheConfiguration">
    <bean class="org.apache.ignite.configuration.CacheConfiguration">
        <property name="name" value="encrypted-cache"/>
        <property name="encryptionEnabled" value="true"/>
    </bean>
</property>
```

### Master Key Generation Example

A keystore with a master key can be created using `keytool` as follows:

Master Key Generation Example

```sh
user:~/tmp:[]$ java -version
java version "1.8.0_161"
Java(TM) SE Runtime Environment (build 1.8.0_161-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.161-b12, mixed mode)

user:~/tmp:[]$ keytool -genseckey \
-alias ignite.master.key \
-keystore ./ignite_keystore.jks \
-storetype PKCS12 \
-keyalg aes \
-storepass mypassw0rd \
-keysize 256

user:~/tmp:[]$ keytool \
-storepass mypassw0rd \
-storetype PKCS12 \
-keystore ./ignite_keystore.jks \
-list

Keystore type: PKCS12
Keystore provider: SunJSSE

Your keystore contains 1 entry

ignite.master.key, 07.11.2018, SecretKeyEntry,
```
