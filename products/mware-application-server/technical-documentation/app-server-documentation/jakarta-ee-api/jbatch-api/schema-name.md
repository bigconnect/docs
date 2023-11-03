# Schema Name

Payara Server allows the ability to define the name of the _database schema_ that will hold all batch job tables. This can be set via the Admin Console, in the _domain.xml_ file, or using Asadmin commands.

### Defining a Schema name through the Admin Console

1. Click on the instance or cluster to move to its configuration page.
2. Select the _Batch_ tab, and from there click on the _Configuration_ sub-tab.
3. Enter your desired value in the _Database Schema Name_ field.
4. Save your changes

### Defining a Schema name using Asadmin

It’s possible to set the schema using the `set-batch-runtime-configuration` command.

The command requires you to specify the `Executor` or `DataSource` lookup name, which you can do with the _-x_ or _-d_ options respectively.

The command defaults to targeting the Admin Server instance (_server_), to target a different instance or cluster, use the _--target_ option.

To specify the schema name, use the _--schemaName_ option, or its shortcut _-n_.

An example can be seen below:

```shell
asadmin set-batch-runtime-configuration -d jdbc/__default --target cluster1 -n test
```

This command expects the administration server to be listening on port **4848**. If it is not, use the _-p_ option to specify the port, for example:

```shell
asadmin -p 5048 set-batch-runtime-configuration -d jdbc/__default --target cluster1 -n test
```

### Defining a Schema name in the domain.xml file

1. Open up the _domain.xml_ configuration file.
2.  Find the `<batch-runtime-configuration>` tag under the appropriate `<config>` tag (e.g. `<config name="server-config">` for the Admin Server), and enter the schema name like so:

    ```xml
    <batch-runtime-configuration schema-name="testSchemaName"></batch-runtime-configuration>
    ```

|   | If you’re editing the _domain.xml_ of a domain that has not been started at least once, the `batch-runtime-configuration` tag will not exist and you will have to add it in yourself |
| - | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

#### Specifying a Blank Schema Name

If you specify a blank schema name, then the schema name will depend on what database vendor is being used:

* On **MySQL**, the _test_ schema will be used
* On **H2**, **Derby**, **Oracle**, **DB2** and **PostgreSQL** the schema name will be the username of the JDBC connection pool resource associated with the JDBC resource to which JBatch is configured to use.
