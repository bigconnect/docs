# Miscellaneous Functions

In miscellaneous functions, you can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following miscellaneous functions:

### alert:info()

Returns information about the trigger for a data drift alert. Use only in alert text for data drift alerts.avro:decode(\<schema>, \<byte array>)

Returns an Avro record by using the specified schema to decode the specified byte array. You can use this function in the Kafka Message Key property when the Kafka stage processes Avro data.

Uses the following arguments:

* schema - Avro schema to use to decode the specified byte array.
* byte array - Byte array to decode.

### emptyList()

Creates an empty list.

### emptyMap()

Creates an empty map.

### every(\<interval>, < hh() | mm() | ss() >)

Represents the interval of hours, minutes, or seconds for generating output directories for the Hadoop FS or Local FS destination.

When used, a destination generates output directories for the specified interval beginning on the hour. For example, when generating directories every 30 minutes, it generates a directory on the hour and on the half-hour.

You can use the function once in the Directory Template property to replace the hour, minute, or second datetime variables.Use the function to replace the smallest time interval in the directory template.

Note: Destinations generate a directory for the smallest unit of measure by default, so do not use the `every` function to generate a directory every hour, minute, or second.&#x20;

Uses the following arguments:

* interval - An integer factor or submultiple of 60 that represents the interval of minutes or seconds to wait between directory generation. Use one of the following values: 1, 2, 3, 4, 5, 6, 10, 12, 15, 20, or 30.
* < hh() | mm() | ss() > - Use `hh()` for hours, `mm()` for minutes, and `ss()` for seconds.

For example, the following Directory Template generates a new directory every twelve hours, beginning on the hour:

```
/outputfiles/${YY()}-${MM()}-${DD()}-${every(12,hh())}
```

The following Directory Template generates a new directory every fifteen minutes, beginning on the hour:

```
/outputfiles/${YY()}-${MM()}-${DD()}-${hh()}-${every(15,mm())}
```

The following Directory Template generates a new directory every 30 seconds, beginning on the hour:

```
/outputfiles/${YY()}-${MM()}-${DD()}-${hh()}-${mm()}-${every(30,ss())}
```

### field:field()

Returns the field name. Available only in the Decimal field expression properties of the Hive Metadata processor.

Return type: String.

### isEmptyList()

Returns true or false based on whether a list is empty.

Return type: Boolean.

### isEmptyMap()

Returns true or false based on whether a map is empty.Return type: Boolean.

### jvm:maxMemoryMB()

Returns the Java heap size allocated to the Data Pipelines in MB. You can use this function in an expression to specify the maximum amount of memory a pipeline can use.

For example, since 65% of the Java heap size is the recommended maximum, the following expression is the default memory limit for a pipeline:

```
${jvm:maxMemoryMB() * 0.65}
```

### length()

Returns the length of a list.

Return type: Integer.

### list:join(\<list field>, \<separator>)

Merges elements in a List field into a String field, using the specified separator between elements.Uses the following arguments:

* list field - The List field that you want to merge.
* separator - The string to use to separate the elements in the merged field.

For example, to merge the list in a colors field using a semicolon as a separator character, you can use the following expression:

```
${list:join(record:value('/colors'), ";")}
```

And if the list field includes "red", "blue", and "yellow" as elements, the expression produces the following string data:

```
red;blue;yellow
```

Return type: String.

### list:joinSkipNulls(\<list field>, \<separator>)

Merges elements in a List field into a String field, using the specified separator between elements and skipping null values.

Uses the following arguments:

* list field - The List field that you want to merge.
* separator - The string to use to separate the elements in the merged field.

For example, say you use the following expression to merge the list in the colors field:

```
${list:join(record:value('/colors'), ";")}
```

And if the list field includes "red", "blue, null, "yellow", the expression ignores the null value and produces the following string data:

```
red;blue;yellow
```

Return type: String.

### offset:column(\<position>)

Returns the value of the positioned offset column for the current table. Available only in the additional offset column conditions of the JDBC Multitable Consumer origin.

Uses the following argument:

* position - Position of the offset column. For example, enter 0 for the first offset column defined in the table configuration. Enter 1 for the second defined offset column.

Return type: String.

### runtime:availableProcessors()

Returns the number of processors available to the Java virtual machine. You can use this function when you want to configure multithreaded processing based on the number of processors available to Data Pipelines.

Return type: Integer.

### runtime:conf(\<runtime property>)

Returns the value for the specified runtime configuration. Use to call a runtime property.Uses the following argument:

* runtime configuration name - Name of the configuration property to use. The property must defined in the sdc.properties file or in a runtime configuration file specified in the sdc.properties file.

For more information, see Using Runtime Properties.

### runtime:loadResource(\<file name>, \<restricted: true | false>)

Returns the value in the specified file, trimming any leading or trailing whitespace characters from the file. Use to call a runtime resource.Uses the following arguments:

* file name - Name of the file that contains the information to be loaded. The file must reside in the `$SDC_RESOURCES` directory:
* restricted - Whether the file has restricted permissions. If set to true, the file must be owned by the system user who runs the Data Pipelines and read and writable only by the owner.

For example, the following expression returns the contents of the restricted JDBCpassword.txt file, trimming any leading or trailing whitespace characters:

```
${runtime:loadResource("JDBCpassword.txt", true)}
```

For more information about runtime resources, see Using Runtime Resources.

### runtime:loadResourceRaw(\<file name>, \<restricted: true | false>)

Returns the entire contents in the specified file, including any leading or trailing whitespace characters in the file. Use to call a runtime resource.Uses the following arguments:

* file name - Name of the file that contains the information to be loaded. The file must reside in the `$SDC_RESOURCES` directory.
* restricted - Whether the file has restricted permissions. If set to true, the file must be owned by the system user who runs the Data Pipelines and read and writable only by the owner.

For example, the following expression returns the entire contents of the restricted JDBCpassword.txt file, including any leading or trailing whitespace characters:

```
${runtime:loadResourceRaw("JDBCpassword.txt", true)}
```

For more information about runtime resources, see Using Runtime Resources.

### sdc:hostname()

Returns the host name of the Data Pipelines machine.For example, you might use the function in the directory template for the Hadoop FS destination to write to a directory that includes the Data Pipelines host name.Return type: String.

### sdc:id()

Returns the Data Pipelines ID.

For a pipeline that runs in standalone execution mode, the ID is a unique identifier associated with the Data Pipelines, such as 58efbb7c-faf4-4d8e-a056-f38667e325d0. The ID is stored in the following file: $SDC\_DATA/sdc.id.

For a pipeline that runs in cluster mode, the ID is the Data Pipelines worker partition ID generated by a cluster application, such as Spark or MapReduce.

### size()

Returns the size of a map.Return type: Integer.

### uuid:uuid()

Returns a randomly generated UUID. For example, you might use the function in an Expression Evaluator processor to generate a UUID for an ID field added to each record.

The uuid:uuid() function uses a lot of entropy on Linux systems and can cause your entropy pools to run dry. When this happens, your pipelines slow to a halt but continue to run. Throughput effectively goes to zero while the system waits for entropy to again become available. As a best practice, we recommend running the haveged daemon on any Data Pipelines node where the uuid:uuid() function is used. The haveged daemon regenerates your entropy pools.

Return type: String.

### vault:read(\<path>, \<key>)

Returns the value for the key on the specified path. You can use the function in username, password, and similar properties such as AWS access key IDs and secret access keys. You can also use the function in HTTP headers and bodies when using HTTPS.

Important: This function is now deprecated and will be removed in a future release. We recommend using the credential functions available with the Vault credential store integration in pipelines that include JDBC stages.

Return type: String.

Uses the following arguments:

* path - The path in Vault to read.
* key - The key for the value that you want returned.

### vault:readWithDelay(\<path>, \<key>, \<delay>)

Returns the value for the key on the specified path after waiting the specified amount of time. Use when you want a delayed response to allow time for external processing.Important: This function is now deprecated and will be removed in a future release. We recommend using the credential functions available with the Vault credential store integration in pipelines that include JDBC stages.

You can use the function in username, password, and similar properties such as AWS access key IDs and secret access keys. You can also use the function in HTTP headers and bodies when using HTTPS.

Return type: String.

Uses the following arguments:

* path - The path in Vault to read.
* key - The key for the value that you want returned.
* delay - Milliseconds to wait before returning the value.
