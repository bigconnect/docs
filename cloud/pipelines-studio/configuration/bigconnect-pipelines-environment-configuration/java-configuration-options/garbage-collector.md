# Garbage Collector

You can define the Java garbage collector that BigConnect Pipelines uses. By default, BigConnect Pipelines uses the Concurrent Mark Sweep (CMS) garbage collector.

For example, if you configure BigConnect Pipelines to use a large heap size, you might want to use the G1 garbage collector. If you define another garbage collector, test and evaluate BigConnect Pipelines performance before making the same change in a production environment. Garbage collector performance depends on each particular use case.

Define the garbage collector based on your installation:

**Tarball or RPM installation**

Define the garbage collector in the SDC\_JAVA8\_OPTS environment variable.

For example, the default garbage collector is defined as follows:

```
export SDC_JAVA8_OPTS=${SDC_JAVA8_OPTS:-"-XX:+UseConcMarkSweepGC -XX:+UseParNewGC"}
```

To use the G1 garbage collector, set the option as follows:

```
export SDC_JAVA8_OPTS=${SDC_JAVA8_OPTS:-"-XX:+UseG1GC"}
```

Modify environment variables using the method required by your installation type.

**Cloudera Manager installation**

Define the garbage collector in the **BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-env.sh** field for the BigConnect Pipelines service in Cloudera Manager.\
\
For example, to use the G1 garbage collector, add the following to the `sdc-env.sh` safety valve:

```
 export SDC_JAVA8_OPTS="-XX:+UseG1GC"
```



**Logging**

BigConnect Pipelines enables garbage collector logging by default to facilitate troubleshooting. Log files are written to `$SDC_LOG/gc.log`. You can disable logging.

Disable garbage collector logging based on your installation:

**Tarball or RPM installation**\
\
Set the SDC\_GC\_LOGGING environment variable to false. For example:

```
export SDC_GC_LOGGING=false
```

Modify environment variables using the method required by your installation type.

**Cloudera Manager installation**

Set the SDC\_GC\_LOGGING environment variable to false in the **BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-env.sh** field for the BigConnect Pipelines service in Cloudera Manager.\
\
For example, add the following to the `sdc-env.sh` safety valve:

```
export SDC_GC_LOGGING=false
```
