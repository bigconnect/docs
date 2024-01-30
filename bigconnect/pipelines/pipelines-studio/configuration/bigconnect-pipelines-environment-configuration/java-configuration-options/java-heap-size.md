# Java Heap Size

Increase or decrease the BigConnect Pipelines Java heap size as necessary, based on the resources available on the host machine. By default, the Java heap size is 1024 MB.

The Java heap size determines the heap size allocated to BigConnect Pipelines and affects the amount of memory BigConnect Pipelines can use when it runs a pipeline. Running a pipeline can use up to 65% of the allocated heap size.

Use the following Java options to define the Java heap size:

* Xmx - Defines the maximum heap size.
* Xms - Defines the minimum heap size.

**Tip**: To avoid constant recalculation of the allocated heap size, set both properties to the same value. To define the unit of measure, use m for MB and g for GB.

Define the heap size based on your installation:

**Tarball or RPM installation**

Define the heap size in the SDC\_JAVA\_OPTS environment variable.

For example, to double the heap size, increase the Xmx and Xms settings as follows:

```
export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Xmx2048m -Xms2048m -server"
```

Modify environment variables using the method required by your installation type.

**Cloud service provider installation**\
\
Define the heap size percentage in the SDC\_HEAP\_SIZE\_PERCENTAGE environment variable. Default is 50% of the available memory on the virtual machine.\
\
To define a specific heap size instead of a percentage, comment out the SDC\_HEAP\_SIZE\_PERCENTAGE environment variable and then define the heap size in the SDC\_JAVA\_OPTS environment variable as described above for an RPM installation.

Modify environment variables using the method required by your installation type.

**Cloudera Manager installation**\
\
Define the heap size in the **BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-env.sh** field for the BigConnect Pipelines service in Cloudera Manager.\
\
For example, to double the heap size, add the following to the `sdc-env.sh` safety valve:

```
export SDC_JAVA_OPTS="-Xmx2048m -Xms2048m"
```

With a heap size of 2048 MB, you can configure a pipeline to use up to 65% - that's 1331 MB of memory.\
\
**Note**: In the pipeline properties, you can use the `jvm:maxMemoryMB()` function to help define the percentage of the heap size the pipeline uses.
