# Logging

MWARE AS captures information about events that occur and records this information using its logging mechanism into log files.

In a MWARE AS domain, the information is logged into the following files by default:

| Instance                   | Log File Location               |
| -------------------------- | ------------------------------- |
| Domain Admin Server (DAS)  | `domain-dir/logs/server.log`    |
| Standalone server instance | `instance-dir/logs/server.log`  |
| Cluster instance           | `instance-dir/logs/cluster.log` |

|   | If an instance is started using the `-verbose` option, it will run in the foreground and log information directly into the console. |
| - | ----------------------------------------------------------------------------------------------------------------------------------- |

MWARE AS uses Java Logging (JUL) to format and output log records. The default configuration file is located at `${DOMAIN_DIR}/config/logging.properties`.

Logging settings can be configured by either using the Asadmin CLI or in the Web Admin Console, like in the following screenshot:

<figure><img src="../../../../../.gitbook/assets/logging_setup.webp" alt=""><figcaption></figcaption></figure>

### Fast Logging

Whenever logging occurs on an application, if the LogRecord in question contains set parameters, they will undergo a forced transformation by having a `toString()` method call. In most cases this is the desirable outcome, but will not provide the best performance. You can enable the **Fast Logging** setting to skip this forced parameter transformation at runtime.

A common use case of this feature would be to prevent database access done by JPA entities, as it is a common occurrence for entity data to be logged out for auditing purposes.

|   | Enabling this setting can cause log entries that contain mutable parameters to be out of sync when they are resolved. Exert caution when using fast logging, as it may cause stale information to be printed out to the log. |
| - | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Using the Admin Console

To configure the `Fast Logging` option, set it in the Logger settings.

<figure><img src="../../../../../.gitbook/assets/fast_logging.webp" alt=""><figcaption></figcaption></figure>

#### Using Asadmin CLI

Use the following command to enable or disable fast logging:

```shell
asadmin> set-log-attributes com.sun.enterprise.server.logging.GFFileHandler.fastLogging=true
```

### Log to File

The `Log to File` option allow you to enable and disable the action of logging to a file. When disabled this should help to minimize disk usage.

#### Enable Log to File using the Admin Console

To configure the `Log to File` option using theAdmin Console:

<figure><img src="../../../../../.gitbook/assets/log_to_file.webp" alt=""><figcaption></figcaption></figure>

#### Enable Log to File using the Asadmin CLI

Use the following command to enable or disable the logging to a file feature:

```shell
asadmin> set-log-attributes com.sun.enterprise.server.logging.GFFileHandler.logtoFile=false
```

### Log To Console

The `Log to Console` option controls if the server writes the logging entries directly to the console. The property only has an effect when the server or instance is started in verbose mode.

To enable the log to console option you simply have to start the domain or instance using the `verbose` option:

```shell
asadmin> start-domain --verbose
```

When the server or domain is not started in verbose mode, the `java.util.ConsoleHandler` will not be active for logging although it is defined as handler in the _logging.properties_ file because there is no console attached with the corresponding server process.

|   | When in verbose mode, the log to Console option will also be active, regardless of the initial value set in the _logging.properties_ file as otherwise, no log output would be visible on the console. |
| - | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

Once the server is run in verbose mode, you can use either the Admin Console or the Asadmin CLI to modify this setting.

#### Configure Logging To Console using the Admin Console

To configure the `Log to Console` option using the Admin Console, check the **Log to Console** option

#### Configure Logging To Console using the Asadmin CLI

Use the following command to enable or disable the log to console feature:

```shell
asadmin> set-log-attributes com.sun.enterprise.server.logging.GFFileHandler.logtoConsole=false
```

### Multiline Mode Support

When the Multiline mode is enabled, each log entry’s message body will be printed on a new line after the message header for each log record. This will improve the overall readability of the server’s log.

Here’s a quick example of how multiline formatted log entries look like:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/multiline_example.png" alt=""><figcaption></figcaption></figure>

#### Enable Multiline Mode using the Admin Console

To enable multiline mode from the Admin Console:

<figure><img src="../../../../../.gitbook/assets/multiline.webp" alt=""><figcaption></figcaption></figure>

#### Enable Multiline Mode using the Admin Console

To enable multiline mode from the command line:

```shell
asadmin> set-log-attributes com.sun.enterprise.server.logging.GFFileHandler.multiLineMode='true'
```

### Environment Variable Replacement

The logging properties file supports environment variables like

```java
.level=${ENV=logLevel}
```

Whenever the server starts up or the logging properties are changed, the value for the `.level` property will be taken from the environment variable `logLevel`.

### Access Logging Max File Size

MWARE AS provides different ways to rotate HTTP access log files. This section will detail the use of the max size of the log file to trigger a rotation.

The **Max File Size** option provides a way to change the file size at which the PayaraServer rotates the access log file. This option accepts an integer value specifying the maximum size of the log file, after which a file rotation will occur.

|   | If the value for this option is set to `0`, log rotation will be disabled. |
| - | -------------------------------------------------------------------------- |

#### Using the Admin Console

To configure the **Max File Size** option using the Admin Console:

<figure><img src="../../../../../.gitbook/assets/max_file_size.webp" alt=""><figcaption></figcaption></figure>

#### Using the Asadmin CLI

Use the following command to set maximum file size option:

```shell
asadmin> set configs.config.server-config.http-service.access-log.maximum-file-size=5000000
```

### Notification Logging Service

The Notification Logging Service captures information about events which come from other services, such as the JMX Monitoring Service, the HealthCheck service or the Request Tracing service and stores these entries it into a log file.

All the generated entries are stored in _server.log_ by default. It is possible to configure the Log Notifier to store its output in a separate log file. More information on the Log Notifier can be found on the [the Log Notifier section](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Logging%20and%20Monitoring/Notification%20Service/Overview.html#log-notifier) of the Notification Service overview.

|   | If this setting is enabled, the default log file will be found at `${DOMAIN_DIR}/logs/notification.log`/ |
| - | -------------------------------------------------------------------------------------------------------- |

The Notification Logging Service uses its own collection of logging properties which are separate from the standard logging facilities of MWARE AS. However, they are stored in the same configuration file.

#### Configuring the Notification Logging Service

**Enabling or Disabling Logging to a File**

The **Log to File** option will provide a way to enable and disable logging to a file of MWARE AS Notifications. When disabled this should help to minimize disk usage. This option will be more accommodating if MWARE AS is running in cloud environment where disk space is scarce.

**Using the Admin Console**

To configure the **Log to File** option using Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/notification-logging/payara_notification_log_to_file.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

You use the following Asadmin command to enable/disable the logging to a file setting:

```shell
asadmin> set-log-attributes fish.payara.enterprise.server.logging.PayaraNotificationFileHandler.logtoFile=false
```

#### Configuring Log File Rotation

By default log rotation is set to rotate when the log file reaches 2 Megabytes(MB). Nonetheless, there are number of ways log rotation can be configured. Size of file at which the log rotates can be changed, it can be set to rotate at date change, rotate when time limit is reached and, finally based on maximum number of history files.

**Rotation on Date Change**

The **Rotation On Date Change** option provides a way to set the log rotation when the system date changes (at midnight, by default).

**Using the Admin Console**

To configure the **Rotation on Date Change** option using the Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/notification-logging/payara_notification_rotation_on_date_change.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

Use the following Asadmin command to enable or disabled the log rotation on date change setting:

```shell
asadmin> set-log-attributes fish.payara.enterprise.server.logging.PayaraNotificationFileHandler.rotationOnDateChange=false
```

**Rotation on File Size**

The **File Rotation Limit** option provides a way to change the file size at which the server triggers the log file rotation. This option accepts an integer value specifying the maximum size of the log file, after which a file rotation will occur. The minimum size it can be set to is `500KB` (`500.000` bytes).

|   | If the value for this option is set to `0`, log rotation will be disabled. |
| - | -------------------------------------------------------------------------- |

**Using the Admin Console**

To configure the **File Rotation Limit** option using the Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/notification-logging/payara_notification_rotation_on_file_size.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

Use the following Asadmin command to set the file rotation limit:

```shell
asadmin> set-log-attributes fish.payara.enterprise.server.logging.PayaraNotificationFileHandler.rotationLimitInBytes=500000
```

**Rotation on Time Limit**

The **File Rotation Time Limit** option provides a way to trigger the log file rotation based on a fixed time limit. The value of this setting is an integer that defines the time limit in minutes until the log rotation gets triggered.

**Using the Admin Console\***

To configure the **File Rotation Time Limit** option using the Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/notification-logging/payara_notification_rotation_on_time_limit.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

Use the following command to set the fixed time limit used to trigger the log rotation:

```shell
asadmin> set-log-attributes fish.payara.enterprise.server.logging.PayaraNotificationFileHandler.rotationTimelimitInMinutes=4
```

**Change the Logging Format**

The **Log File Logging Format** option can be used to change the log entries' format. There are 3 logging formats available: `ULF`, `ODL` and `JSON`, each one represented by a specific formatter class present in the Payara Platform API.

**Using the Admin Console**

To configure the **Log File Logging Format** option using the Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/notification-logging/payara_notification_change_logging_format.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

Use the following command to change the logging format:

```shell
asadmin> set-log-attributes fish.payara.enterprise.server.logging.PayaraNotificationFileHandler.formatter=fish.payara.enterprise.server.logging.JSONLogFormatter
```

**Set the Maximum Number of Historic Files**

The `Maximum History Files` option provides a way to set the limit on the number of log files that can be created by MWARE AS. Once the number of files reaches the set limit, the oldest rotated log file is deleted. If the value for this option is set `0`, all the rotated log files are preserved.

**Using the Admin Console**

To configure the `Maximum History File` option using the Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/notification-logging/payara_notification_set_maximum_number_of_history_files.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

Use the following command to set the limit on the number of log files that can be created by MWARE AS:

```shell
asadmin> set-log-attributes fish.payara.enterprise.server.logging.PayaraNotificationFileHandler.maxHistoryFiles=20
```

**Change the Name and Location of the Log File**

The `Log File` option provides a way to change the default name and location of the server log files.

**Using the Admin Console**

To configure `Log File` option using Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/notification-logging/payara_notification_change_name_and_location_of_log_file.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

Use the following command to change the default name and location of the server log files:

```shell
asadmin> set-log-attributes fish.payara.enterprise.server.logging.PayaraNotificationFileHandler.file=${com.sun.aas.instanceRoot}/notification/testNotification.log
```

|   | As seen in the sample command, you can use the `${com.sun.aas.instanceRoot}` system property to reference the current domain directory location. |
| - | ------------------------------------------------------------------------------------------------------------------------------------------------ |

**Enable File Compression on Rotation**

The `Compress on Rotation` option provides a way to enable the automatic compression of log files on rotation.

**Using the Admin Console**

To configure the `Compress on Rotation` option using the Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/notification-logging/payara_notification_set_compress_on_rotation.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

Use the following command to enable or disable compression of log files on rotation:

```shell
asadmin> set-log-attributes fish.payara.enterprise.server.logging.PayaraNotificationFileHandler.compressOnRotation=true
```

### Log Rotation

File rotation keeps log files manageable, as older log files are automatically deleted after a certain amount of time, and its proper configuration is recommended to keep a healthy disk space management.

#### Enabling file rotation

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/daily-log-rotation.png" alt=""><figcaption></figcaption></figure>

By default a size rotation of `2MB` is used for server logs, meaning no log files will be deleted until the size limit is reached and a new file is created at midnight.

MWARE AS has different rotation conditions which can be fine-tuned based on your needs:

Time

Daily, weekly, monthly or even hourly log rotation.

Size

Logs are rotated when they exceed a certain limit.

Number

Maximum number of entries kept in a log file.

These settings can be configured in the Admin Console:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/log_rotation_settings.png" alt=""><figcaption></figcaption></figure>

Which allows you to change how the logs are rotated to your needs and can be combined with the default "daily" log rotation.

|   | Enabling daily log rotation and setting a limit on the number of logs to keep will keep a certain number of days of logs before the oldest log file gets deleted at midnight. |
| - | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### ANSI Coloured Logging

MWARE AS supports the use of ANSI coloured log entries when running in verbose mode.

To enable ANSI colours run the following command using the Asadmin CLI:

```shell
asadmin> set-log-attributes com.sun.enterprise.server.logging.UniformLogFormatter.ansiColor=true
```

|   | This setting is enabled by default when a domain is started in Linux and MacOS environments. |
| - | -------------------------------------------------------------------------------------------- |

#### Log File Compression on Rotation

MWARE AS can be configured to automatically compress rotated log files in an automatic manner to save disk space.

**Using the Web Admin Console**

When log rotation is enabled, you can turn on automatic compression in the `Logger Settings` section of the Admin Console, by ticking the `Compress on Rotation` checkbox:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/compress_on_rotation.png" alt=""><figcaption></figcaption></figure>

**Using the Asadmin CLI**

Use the following command to enable or disabled the automatic compression of log files on rotation:

```shell
asadmin> set-log-attributes com.sun.enterprise.server.logging.GFFileHandler.compressOnRotation='true'
```

**View Log entries in the Admin Console**

Log entries that live in compressed rotated files can be viewed in Admin Console as if they were uncompressed.

This is an example how the **Log Viewer** may visualize the entries of 3 compressed files:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/zipped_logs.png" alt=""><figcaption></figcaption></figure>

### JSON Log Formatter

Besides the standard Uniform Log Format (ULF) and Oracle Diagnostics Logging (ODL) formats (inherited from MWARE AS’s source: GlassFish Server Open Source Edition), MWARE AS provides a JSON format. With this format, every entry is formatted as a JSON object string. These entries can be easily processed by any JSON parser for further data processing.

Once the JSON formatter is enabled, the server’s log file may look similar to this sample:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/json_example.png" alt=""><figcaption></figcaption></figure>

#### Enable the JSON formatter using the Web Admin Console

To enable the JSON formatter using the Admin Console, just select `JSON` from the list of Logging Formats, either for `Console` or `Log File`:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/json_config.png" alt=""><figcaption></figcaption></figure>

#### Enable the JSON formatter using the Asadmin CLI

The following command will enable the JSON formatter:

```shell
asadmin> set-log-attributes com.sun.enterprise.server.logging.GFFileHandler.formatter='fish.payara.enterprise.server.logging.JSONLogFormatter'
```

#### Configure Prefixed field names

In some situations, the JSON representation of a log entry may use field names that clash with existing standard field names that logging gathering tools may use them for specific purposes. To solve this problem, MWARE AS can be configured to automatically prefix all field names in the JSON object representation with an underscore (`_`) character. See the following sample to get an idea of how such a JSON payload would look like:

<figure><img src="https://docs.payara.fish/community/docs/_images/logging/json_underscore_prefix_example.png" alt=""><figcaption></figcaption></figure>

The following command will enable this configuration setting:

```shell
asadmin> set-log-attributes fish.payara.deprecated.jsonlogformatter.underscoreprefix=true
```

#### Support for Additional Fields

The JSON Log Formatter also supports the customization of additional fields through the `setParameters` method of the [LogRecord](https://docs.oracle.com/javase/8/docs/api/java/util/logging/LogRecord.html) class that is part of the standard `java.util.logging` package. This action is done when logging a new entry at runtime, so it is limited to an application business logic context.

Here’s a quick example of how to pass additional fields to the resulting JSON object by using a map with a single entry:

```java
LogRecord lr = new LogRecord(Level.INFO, "Sample message");
lr.setParameters(new Object[]{Collections.singletonMap("key", "value")});
logger.log(lr);
```

### Exclude Fields

All the three log formatters `ODLLogFormatter`, `UniformLogFormatter`, and `JSONLogFormatter` support excluding log entry fields when being recorded. This makes the log file more compact and removes unnecessary information in the case you do not need it or want to use it.

You can change the `com.sun.enterprise.server.logging.GFFileHandler.excludeFields` within the `<PAYARA_HOME>/glassfish/domains/<domain-name>/config/logging.properties` file or use the Admin Console the Asadmin CLI.

|   | If the configuration is changed while the server is running, a log rotation happens so that a log file always contains the same set of information. |
| - | --------------------------------------------------------------------------------------------------------------------------------------------------- |

#### Using the Admin Console

To configure the excluded fields in the log entries, select them on the _Logger_ settings screen:

<figure><img src="../../../../../.gitbook/assets/exclude-fields.webp" alt=""><figcaption></figcaption></figure>

#### Using the Asadmin CLI

Use the following command to change the excluded fields:

```shell
asadmin> set-log-attributes com.sun.enterprise.server.logging.GFFileHandler.excludeFields=tid,version
```

|   | Multiple fields must be separated by a `,` and no additional spaces should be included. |
| - | --------------------------------------------------------------------------------------- |

#### Excludable Fields

The following table lists the fields that can be excluded:

| Field      | Description                                                                                                                                                                                           |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tid        | ThreadId, the thread name that generated the log entry.                                                                                                                                               |
| timeMillis | The time in milliseconds since January 1st, 1970 (Unix epoch) when the log entry is generated. The human-readable version of the timestamp at the beginning of the log entry is always in the output. |
| levelValue | The numeric value corresponding to the severity level of the message. The severity level name is always present in the log entry. SEVERE = 1000, INFO = 800, FINE = 500, etc…​                        |
| version    | The MWARE AS version number.                                                                                                                                                                          |
