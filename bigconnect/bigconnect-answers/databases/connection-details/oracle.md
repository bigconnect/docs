# Oracle

To add a database connection, click on the **gear** icon in the top right, and navigate to **Admin settings** > **Databases** > **Add a database**.

## Settings

You can edit these settings at any time. Just remember to save your changes.

#### Display name

The display name for the database in the Answers interface.&#x20;

#### Host

Your database’s IP address, or its domain name (e.g., esc.mydatabase.com).&#x20;

#### Port

The database port. E.g., 1521. Oracle system ID (SID)

Usually something like ORCL or XE. Optional if using service name.&#x20;

#### Oracle service name

Optional TNS alias. Username

The database username for the account that you want to use to connect to your database. You can set up multiple connections to the same database using different user accounts to connect to the same database, each with different sets of privileges.&#x20;

#### Password

The password for the username that you use to connect to the database.&#x20;

#### Use a secure connection (SSL)

You can use both client and server authentication (known as mutual authentication).

#### Client authentication with a keystore

To configure the server (the Oracle server) to authenticate the identity of the client (Answers), you need to configure a keystore file that includes the client’s private key.

You’ll import the client’s private key into the keystore (rather than a root CA into a truststore file). Add the following JVM options for Answers:

```
-Djavax.net.ssl.keyStore=/path/to/keystore.jks
-Djavax.net.ssl.keyStoreType=JKS \
-Djavax.net.ssl.keyStorePassword=<keyStorePassword>
```

With this done, the Oracle server will authenticate Answers using the private key when Answers tries to connect over SSL.

#### Server authentication with a truststore

To configure the client (Answers) to authenticate the identity of the server (the Oracle server), you may need to configure a truststore file that includes the server’s root CA, so that the JVM running Answers trusts its certificate chain. Refer to the Oracle documentation on using keytool to manage key and truststore files, importing certificates, etc.

For more information on setting up a truststore for AWS RDS Oracle instances, see the instructions provided by Amazon.

If you need to connect to other databases using SSL, instead of creating a new truststore, you’ll probably want to add the RDS CA to your existing truststore file (likely called `cacerts`).

#### Default result cache duration

How long to keep question results. By default, Answers will use the value you supply on the cache settings page, but if this database has other factors that influence the freshness of data, it could make sense to set a custom duration. You can also choose custom durations on individual questions or dashboards to help improve performance.

Options are:

* **Use instance default (TTL)**. TTL is time to live, meaning how long the cache remains valid before Answers should run the query again.
* **Custom**.

You can also set cache duration per questions. See Advanced caching controls.

## Downloading the Oracle JDBC Driver JAR

You can download a JDBC driver from [Oracle’s JDBC driver downloads page](https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html).

The minimum driver version should be 19c, regardless of which Java version or Oracle Database version you have.

We recommend using the `ojdbc8.jar` JAR.

## Adding the Oracle JDBC Driver JAR to the Answers plugins directory

In your Answers directory (the directory where you keep and run your answers.jar), create a directory called `plugins` (if it doesn’t already exist.

Move the JAR you just downloaded (`ojdbc8.jar`) into the plugins directory, and restart Answers. Answers will automatically make the Oracle driver available when it starts back up.

By default, the plugins directory is called `plugins`, and lives in the same directory as the Answers JAR.

For example, if you’re running Answers from a directory called `/app/`, you should move the Oracle JDBC driver JAR to `/app/plugins/`:

```
# example directory structure for running Answers with Oracle support
/app/answers.jar
/app/plugins/ojdbc8.jar
```

