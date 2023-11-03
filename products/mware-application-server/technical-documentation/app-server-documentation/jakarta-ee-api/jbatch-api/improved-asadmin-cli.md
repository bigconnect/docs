# Improved Asadmin CLI

The following is a list of the administration commands that have been introduced or changed from GlassFish in Payara Server to improve the **JBatch API**.

### `set-batch-runtime-configuration`

**Usage**

`asadmin> set-batch-runtime-configuration --datasourcelookupname="jdbc/default" --executorservicelookupname="concurrent __defaultManagedExecutorService" --schemaName="APP" --tablePrefix="BJP" --tableSuffix="BJS"`

**Aim**

Configures the batch runtime storage and execution settings.

#### Command Options

| Option                        | Shortcut | Description                                                                                 | Default | Mandatory                                             |
| ----------------------------- | -------- | ------------------------------------------------------------------------------------------- | ------- | ----------------------------------------------------- |
| `--target`                    | N/A      | The instance or cluster to configure                                                        | server  | No                                                    |
| `--datasourcelookupname`      | -d       | The JNDI name of the JDBC data source used to store job information                         | N/A     | Yes (unless `executorservicelookupname` is specified) |
| `--executorservicelookupname` | -x       | The JNDI name of the Managed Executor Service that provides threads for the jobs execution. | N/A     | Yes (unless `datasourceLookupName` is used)           |
| `--schemaName`                | -n       | The database schema that holds the batch tables.                                            | APP     | No                                                    |
| `--tablePrefix`               | N/A      | The prefix to apply to the batch table name.                                                | N/A     | No                                                    |
| `--tableSuffix`               | N/A      | Sets the suffix to apply to the batch table name.                                           | N/A     | No                                                    |

#### Example

The following command will set the data source, and the prefix and suffix for the DAS:

```shell
asadmin > set-batch-runtime-configuration --target="server"
            --datasourcelookupname="jdbc/BatchJobsDS"
            --tablePrefix="BATCHJ_" --tableSuffix="_JB"
```

### `purge-Jakarta EE API/JBatch API-repository`

**Usage**

`asadmin> purge-Jakarta EE API/JBatch API-repository <apptag>`

**Aim**

Purges the current JBatch repository of all jobs scheduled for the referenced application’s tag. The application’s tag is structured like this:

```
<Configuration Identifier>:<Application Name>__/<Application Name>
```

Where **Configuration Identifier** is the identifier of the configuration associated to the instance or cluster where the application is deployed and **Application Name** is the name of the deployed application.

#### Command Options

There are no options available.

#### Example

To remove all current jobs for an application named _Jakarta EE API/JBatch API-test_ deployed on the DAS (`server-config`) then run the following command:

```shell
asadmin > purge-Jakarta EE API/JBatch API-repository "server-config:Jakarta EE API/JBatch API-test__/Jakarta EE API/JBatch API-test"
```

|   | The command will remove all current jobs created by the referenced application whether these jobs are running or not. |
| - | --------------------------------------------------------------------------------------------------------------------- |

|   | If you are having trouble finding the application’s tag for your application, you can get it programmatically on a Java EE application by using the following code: |
| - | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

```java
import com.ibm.Jakarta EE API/JBatch API.container.servicesmanager.ServicesManager;
import com.ibm.Jakarta EE API/JBatch API.container.servicesmanager.ServicesManagerImpl;
import com.ibm.Jakarta EE API/JBatch API.container.services.IPersistenceManagerService;
...

JobOperator jobOperator = getJobOperator();
Long executionId = jobOperator.start("job-name", new Properties());

ServicesManager manager = ServicesManagerImpl.getInstance();
IPersistenceManagerService ps = manager.getPersistenceManagerService();
String applicationTag = ps.getJobCurrentTag(jobid);
```

You need to add a dependency to IBM’s **JBatch runtime** to your project to access the classes mentioned:

```xml
<dependency>
    <groupId>com.ibm.Jakarta EE API/JBatch API</groupId>
    <artifactId>com.ibm.Jakarta EE API/JBatch API-runtime-all</artifactId>
    <version>1.0</version>
    <scope>provided</scope>
</dependency>
```

\[\[clean-Jakarta EE API/JBatch API-repository]] == `clean-Jakarta EE API/JBatch API-repository`

**Usage**::\`asadmin> clean-Jakarta EE API/JBatch API-repository --status COMPLETED|ALL --days \<int.value> \<JobName>\` **Aim**:: The clean-Jakarta EE API/JBatch API-repository subcommand deletes records of job executions.

#### Command Options

| Option     | Type                                    | Description                                                                                                                                                                                                             | Default   | Mandatory |
| ---------- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | --------- |
| `--status` | <ul><li>ALL</li><li>COMPLETED</li></ul> | The status of batch jobs executions to which delete the records of. If ALL then all job executions will be deleted, regardless of status. If COMPLETED only job executions that successfully completed will be deleted. | COMPLETED | No        |
| `--days`   | Integer                                 | The number of days previous that the end time of the job execution must be to get deleted.                                                                                                                              | 1         | No        |

#### Example

The following command will delete all Batch jobs that were executed before 7 days ago.

```shell
asadmin > clean-Jakarta EE API/JBatch API-repository --status=ALL --days=7 testjob
```

### `list-batch-jobs`

**Usage**

`asadmin> list-batch-jobs --output jobname,appname,instancecount,instanceid,executionid,batchstatus,starttime,endtime,exitstatus --limit <long value> --offset <long.value> --header true|false <JobName>`

**Aim**

Lists batch jobs and information about them.

#### Command Options

| Option     | Shortcut | Description                                                                                                                                                                                                                                                                                                                                                                                                                                               | Default                                                            | Mandatory |
| ---------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | --------- |
| `--target` | N/A      | The instance or cluster to configure                                                                                                                                                                                                                                                                                                                                                                                                                      | server                                                             | No        |
| `--long`   | -l       | Shows detailed information about batch jobs                                                                                                                                                                                                                                                                                                                                                                                                               | false                                                              | No        |
| `--output` | -0       | <p>Shows specific information about batch jobs. You can specify information to display by using comma-separated list.</p><p>Available values are:</p><ul><li><code>jobname</code></li><li><code>appname</code></li><li><code>instancecount</code></li><li><code>instanceid</code></li><li><code>executionid</code></li><li><code>batchstatus</code></li><li><code>starttime</code></li><li><code>endtime</code></li><li><code>exitstatus</code></li></ul> | `jobname` and `instancecount` information are displayed by default | No        |
| `--header` | -h       | Option to choose whether column heading should be displayed or not                                                                                                                                                                                                                                                                                                                                                                                        | false                                                              | No        |
| `--limit`  | N/A      | Set number of rows of batch jobs to display                                                                                                                                                                                                                                                                                                                                                                                                               | N/A                                                                | No        |
| `--offset` | N/A      | Set number of rows of batch jobs to skip                                                                                                                                                                                                                                                                                                                                                                                                                  | N/A                                                                | No        |

#### Example

```shell
asadmin > list-batch-jobs --target server
            --long true
            --limit 20
            --offset 0
```
