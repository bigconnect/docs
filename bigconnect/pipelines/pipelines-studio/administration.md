# Administration

### Viewing  Configuration Properties <a href="#ariaid-title4" id="ariaid-title4"></a>

To view configuration properties, click Administration > Configuration.

### Viewing Data Pipelines Directories <a href="#ariaid-title5" id="ariaid-title5"></a>

You can view the directories that the Data Pipelines uses. You might check the directories being used to access a file in the directory or to increase the amount of available space for a directory.

Data Pipelines directories are defined in environment variables.&#x20;

To view Data Pipelines directories, click Administration > SDC Directories.

The following table describes the Data Pipelines directories that display:

| Directory                     | Includes                                                                                                                                                                         | Environment Variable              |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| Runtime                       | Base directory for Data Pipelines executables and related files.                                                                                                                 | SDC\_DIST                         |
| Configuration                 | <p>The Data Pipelines configuration file, <code>sdc.properties</code>, and related realm properties files and keystore files.</p><p>Also includes the logj4 properties file.</p> | SDC\_CONF                         |
| Data                          | Pipeline configuration and run details.                                                                                                                                          | SDC\_DATA                         |
| Log                           | Data Pipelines log file, `sdc.log`.                                                                                                                                              | SDC\_LOG                          |
| Resources                     | Directory for runtime resource files.                                                                                                                                            | SDC\_RESOURCES                    |
| SDC Libraries Extra Directory | Directory to store external libraries.                                                                                                                                           | STREAMSETS\_LIBRARIES\_EXTRA\_DIR |

### Viewing Data Pipelines Metrics <a href="#ariaid-title6" id="ariaid-title6"></a>

You can view metrics about Data Pipelines, such as the CPU usage or the number of pipeline runners in the thread pool.

1. To view Data Pipelines metrics, click Administration > Metrics.\
   The Data Pipelines Metrics page displays all metrics by default.
2. To modify the metrics that display on the page, click the More icon, and then click Settings.
3. Remove any metric charts that you don't want to display, and then click Save.

### Viewing Data Pipelines Logs <a href="#ariaid-title7" id="ariaid-title7"></a>

You can view and download log data. When you download log data, you can select the file to download.

1. To view log data for the Data Pipelines, click Administration > Logs.\
   The Data Pipelines UI displays roughly 50,000 characters of the most recent log information.
2. To stop the automatic refresh of log data, click Stop Auto Refresh.Or, click Start Auto Refresh to view the latest data.
3. To view earlier events, click Load Previous Logs.
4. To download the latest log file, click Download. To download a specific log file, click Download > \<file name>.The most recent information is in the file with the highest number.

#### Data Pipelines Log Format <a href="#ariaid-title8" id="ariaid-title8"></a>

Data Pipelines uses the Apache Log4j library to write log data. Each log entry includes a timestamp and message along with additional information relevant for the message.

In the Data Pipelines UI, each log entry has the following information:

* Timestamp
* Pipeline
* Severity
* Message
* Category
* User
* Runner
* Thread

For example:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/LogFormat-UIexample.png)

In the downloaded log file, the log entry has the same information, presented in a different order, plus the stage that encountered the message. The downloaded file shows the information in the following order:

* Timestamp
* User
* Pipeline
* Runner
* Thread
* Stage
* Severity
* Category
* Message

For example:

```
2019-03-19 09:34:26,236 [user:admin] [pipeline:Test/TestPipeline65f67dde-faad-426d-ac47-8a2cd707f224] [runner:] [thread:webserver-430] [stage:] INFO  StandaloneAndClusterRunnerProviderImpl - Pipeline execution mode is: STANDALONE 
```

For this message, the stage and runner are not relevant, and therefore not included in the log entry.

The information included in the downloaded file is set by the `log4j.appender.stdout.layout.ConversionPattern` property in the log configuration file, $SDC\_CONF/sdc-log4j.properties. The default configuration sets this property to:

```
%d{ISO8601} [user:%X{s-user}] [pipeline:%X{s-entity}] [runner:%X{s-runner}] [thread:%t] [stage:%X{s-stage}] %-5p %c{1} - %m%n
```

To customize the log format, see the [Log4j documentation](http://logging.apache.org/log4j/1.2/manual.html). Data Pipelines provides the following custom objects:

* `%X{s-entity}` - Pipeline name and ID
* `%X{s-runner}` - Runner ID
* `%X{s-stage}` - Stage name
* `%X{s-user}` - User who initiated the operation

#### Modifying the Log Level <a href="#ariaid-title9" id="ariaid-title9"></a>

If the Data Pipelines logs do not provide enough troubleshooting information, you can modify the log level to display messages at another severity level.

By default, Data Pipelines logs messages at the INFO severity level. You can configure the following log levels:

* TRACE
* DEBUG
* INFO (Default)
* WARN
* ERROR
* FATAL

1. Click Administration > Logs.
2. Click Log Config.Data Pipelines displays the contents of the log configuration file, `$SDC_CONF/sdc-log4j.properties`.
3.  Change the default value of INFO for the following line in the file:

    ```
    log4j.logger.com.streamsets=INFO
    ```

    For example, to set the log level to DEBUG, modify the line as follows:

    ```
    log4j.logger.com.streamsets=DEBUG
    ```
4. Click Save.The changes that you make to the log level take effect immediately - you do not need to restart Data Pipelines. You can also change the log file by directly editing the log configuration file, `$SDC_CONF/sdc-log4j.properties`.Note: For a Cloudera Manager installation, use Cloudera Manger to modify the log level. In Cloudera Manager, select the StreamSets service, then click Configuration. Click Category > Logs, and then modify the value of the Data Pipelines Logging Threshold property.

When you’ve finished troubleshooting, set the log level back to INFO to avoid having verbose log files.

### Shutting Down Data Pipelines <a href="#ariaid-title10" id="ariaid-title10"></a>

You can shut down and then manually launch Data Pipelines to apply changes to the Data Pipelines configuration file, environment configuration file, or user logins.

Use one of the following methods to shut down Data Pipelines:

User interfaceTo use the Data Pipelines UI for shutdown:

1. Click Administration > Shut Down.
2. When a confirmation dialog box appears, click Yes.

Command line when started as a serviceTo use the command line for shutdown when Data Pipelines is started as a service, use the required command for your operating system:

* For CentOS 6, Oracle Linux 6, Red Hat Enterprise Linux 6, or Ubuntu 14.04 LTS, use: `service sdc stop`
* For CentOS 7, Oracle Linux 7, Red Hat Enterprise Linux 7, or Ubuntu 16.04 LTS, use: `systemctl stop sdc`

Command line when started manuallyTo use the command line for shutdown when Data Pipelines is started manually, use the Data Pipelines process ID in the following command:

```
kill -15 <process ID>
```

### Viewing Users and Groups <a href="#ariaid-title12" id="ariaid-title12"></a>

If you use file-based authentication, you can view all user accounts granted access to this Data Pipelines instance, including the roles and groups assigned to each user.

To view users and groups, click Administration > Users and Groups. Data Pipelines displays a read-only view of the users, groups, and roles.

You configure users, groups, and roles for file-based authentication in the associated `realm.properties` file located in the Data Pipelines configuration directory, `$SDC_CONF`. For more information, see Configuring File-Based Authentication.

### Health Inspector <a href="#ariaid-title18" id="ariaid-title18"></a>

The Data Pipelines Health Inspector provides a snapshot of how Data Pipelines is functioning. When you run Health Inspector, it performs checks for common misconfigurations and errors. You can use the Health Inspector to quickly check the health of your Data Pipelines.

Health Inspector provides only Data Pipelines-level details. For pipeline-level details, monitor the pipeline or review the Data Pipelines log.

The Health Inspector provides the following categories of information:

* Data Pipelines configuration - Displays the settings for certain Data Pipelines configuration properties, such as the maximum number of pipeline errors allowed in production.
* Java Virtual Machine (JVM) process - Displays the settings for certain JVM configuration properties, such as the maximum amount of memory allotted to the JVM. Also generates related usage statistics, such as the percentage of the JVM memory currently used by Data Pipelines.
* Machine - Displays important details about available resources on the Data Pipelines machine, such as the available space in the runtime directory.
* Networking - Verifies that the internet is accessible

#### Viewing the Health Inspector <a href="#ariaid-title19" id="ariaid-title19"></a>

Data Pipelines generates Health Inspector details each time you open the Health Inspector page.

1. To view the Data Pipelines Health Inspector, click the Help icon, and then click Health Inspector.
2.  To view all available information, click the Expand All link.

    Green indicates that values are within expected range. Red indicates that values fall beyond the expected range.

    Some details, such as JVM Child Processes, provide additional information. To view that information, click Show Output.
3. To refresh a category of information, click the Rerun link for the category.
4. To refresh all Health Inspector details, navigate away from the page, and then return.

### REST Response <a href="#ariaid-title20" id="ariaid-title20"></a>

You can view REST response JSON data for different aspects of the Data Pipelines, such as pipeline configuration information or monitoring details.

You can use the REST response information to provide Data Pipelines details to a REST-based monitoring system. Or you might use the information in conjunction with the Data Pipelines REST API.

You can access the following REST response data:

* Pipeline Configuration - Provides information about the pipeline and each stage in the pipeline.
* Pipeline Rules - Provides information about metric and data rules and alerts.
* Definitions - Provides information about all available Data Pipelines stages.
* Preview Data- Provides information about the preview data moving through the pipeline. Also includes monitoring information that is not used in preview.
* Pipeline Monitoring - Provides monitoring information for the pipeline.
* Pipeline Status - Provides the current status of the pipeline.
* Data Pipelines Metrics - Provides metrics about Data Pipelines.
* Thread Dump - Lists all active Java threads used by Data Pipelines.

#### &#x20;Viewing REST Response Data <a href="#ariaid-title21" id="ariaid-title21"></a>

You can view REST response data from the location where the relevant information displays. For example, you can view Data Pipelines Metrics REST response data from the Data Pipelines Metrics page.

You can view REST response data from the following locations:Edit modeFrom the Properties panel, you can use the More icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonMore.png)) to view the following REST response data:

* Pipeline Configuration
* Pipeline Rules
* Pipeline Status
* Definitions

Preview modeFrom the Preview panel, you can use the More icon to view the Preview Data REST response data.Monitor modeFrom the Monitor panel, you can use the More icon to view the following REST response data:

* Pipeline Monitoring
* Pipeline Configuration
* Pipeline Rules
* Pipeline Status
* Definitions

Data Pipelines Metrics page

From the Data Pipelines Metrics page, Administration > Metrics , you can use the More icon to view the following REST response data:

* Data Pipelines Metrics
* Thread Dump

#### Disabling the REST Response Menu <a href="#ariaid-title22" id="ariaid-title22"></a>

You can configure the Data Pipelines to disable the display of REST responses.

1. To disable the REST Response menus, click the Help icon, and then click Settings.
2. In the Settings window, select Hide the REST Response Menu.

### &#x20;Command Line Interface <a href="#ariaid-title23" id="ariaid-title23"></a>

Data Pipelines provides a command line interface that includes a basic `cli` command. Use the command to perform some of the same actions that you can complete from the Data Pipelines UI. Data Pipelines must be running before you can use the `cli` command.

You can use the following commands with the basic `cli` command:

**help**

Provides information about each command or subcommand.

**manager**

Provides the following subcommands:

* start - Starts a pipeline.
* status - Returns the status of a pipeline.
* stop - Stops a pipeline.
* reset-origin - Resets the origin when possible.
* get-committed-offsets - Returns the last-saved offset for pipeline failover.
* update-committed-offsets - Updates the last-saved offset for pipeline failover.

**store**

Provides the following subcommands:

* import - Imports a pipeline.
* list - Lists information for all available pipelines.

#### Java Configuration Options for the Cli Command <a href="#ariaid-title24" id="ariaid-title24"></a>

Use the SDC\_CLI\_JAVA\_OPTS environment variable to modify Java configuration options for the `cli` command.

For example, to set the `-Djavax.net.ssl.trustStore` option for the `cli` command when using Data Pipelines with HTTPS, run the following command:

```
export SDC_CLI_JAVA_OPTS="-Djavax.net.ssl.trustStore=<path to truststore file> ${SDC_CLI_JAVA_OPTS}"
```

#### Using the Cli Command <a href="#ariaid-title25" id="ariaid-title25"></a>

Call the `cli` command from the `$SDC_DIST` directory.

Use the following command as the base for all `cli` commands:

```
bin/streamsets cli \
(-U <sdcURL> | --url <sdcURL>) \
[(-a <sdcAuthType> | --auth-type <sdcAuthType>)] \
[(-u <sdcUser> | --user <sdcUser>)] \
[(-p <sdcPassword> | --password <sdcPassword>)] \
<command> <subcommand> [<args>] 
```
