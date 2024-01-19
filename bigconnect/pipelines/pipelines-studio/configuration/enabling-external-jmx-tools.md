# Enabling External JMX Tools

BigConnect Pipelines uses JMX metrics to generate the graphical display of the status of a running pipeline. You can provide the same JMX metrics to external tools if desired.

Information provided by JMX metrics includes pipeline details like a histogram for the number of error records per batch or the amount of memory the pipeline uses. Stage-related details are also provided, such as the number of output records or stage errors. Some stages have stage-related custom metrics.

The following Java environment variables expose the BigConnect Pipelines JMX metrics on a specified port, allowing integration with external tools:

* com.sun.management.jmxremote
* com.sun.management.jmxremote.port=\<port\_number>
* com.sun.management.jmxremote.local.only=\<true | false>
* com.sun.management.jmxremote.authenticate=\<true | false>
* com.sun.management.jmxremote.ssl=\<true | false>

You can pass the variables in the command line as part of the SDC\_JAVA\_OPTS environment variable. Or, you can add the variables to the SDC\_JAVA\_OPTS environment variable in the required file for your installation type, as described in Modifying Environment Variables.

For example, the following set of variables passes JMX metrics through port 3333:

```
export SDC_JAVA_OPTS="-Dcom.sun.management.jmxremote \
-Dcom.sun.management.jmxremote.port=3333 \
-Dcom.sun.management.jmxremote.local.only=false \
-Dcom.sun.management.jmxremote.authenticate=false \
-Dcom.sun.management.jmxremote.ssl=false"
```

### Viewing JMX Metrics in External Tools <a href="#ariaid-title2" id="ariaid-title2"></a>

You can view the BigConnect PipelinesJMX metrics in external tools. The BigConnect Pipelines JMX metric names all begin with "sdc.pipeline."

BigConnect Pipelines JMX metrics use the following naming pattern:

```
sdc.pipeline.<pipeline name>.<pipeline revision>.<category: pipeline|stage|custom>.\
[<stage library>_<library revision>].<metric name>.<metric type>
```

For example, the following is a batch count meter for the first revision of a pipeline named Write to Kafka:

```
sdc.pipeline.Write to Kafka.0.pipeline.batchCount.meter
```

The following metric is a counter for the memory consumed by the File Tail origin in the same Write to Kafka pipeline:

```
sdc.pipeline.Write to Kafka.0.stage.\
com_streamsets_pipeline_stage_origin_logtail_FileTailDSource_1.memoryConsumed.counter
```

### Custom Metrics <a href="#ariaid-title3" id="ariaid-title3"></a>

BigConnect Pipelines provides custom metrics for some stages. When a pipeline includes the stages below, you can view custom metrics for the stages in the BigConnect Pipelines UI in Monitor mode or when you view JMX metrics using an external tool:

**File Tail origin**\
\
In addition to the standard metrics available for origins, File Tail provides the following custom metrics:

*   Offset Lag - The amount of data remaining in the file being read. This metric displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_origin_logtail_FileTailDSource_\
    <library version>.offsets.lag.<file path>.counter
    ```
*   Pending Files - The number of files in the directory that still need to be read. This metric displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_origin_logtail_FileTailDSource_\
    <library version>.pending.files.<file path>.counter
    ```

**Amazon S3 destination**\
\
In addition to the standard metrics available for origins, Amazon S3 provides the following custom metrics:

*   Transfer Rate KB Meter - Displays the transfer rate in KB. Appears when the destination writes whole files to the destination system with the whole file data format. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_s3_\
    AmazonS3DTarget_<library version>.transferRateKb.meter
    ```

**Hadoop FS destination**\
\
In addition to the standard metrics available for origins, Hadoop FS provides the following custom metrics:

*   Late Records meter and counter - The number of late records written to HDFS. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_HdfsTarget_\ 
    HDFSDTarget_<library version>.lateRecords.<counter | metric>
    ```
*   To HDFS Records meter and counter. The number of records written to HDFS. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_HdfsTarget_\
    HDFSDTarget_<library version>.hdfsRecords.<counter | metric> 
    ```
*   Transfer Rate KB Meter - Displays the transfer rate in KB. Appears when the destination writes whole files to the destination system with the whole file data format. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_HdfsTarget_HDFSDTarget_\
    <library version>.transferRateKb.meter
    ```

**Local FS destination**\
\
In addition to the standard metrics available for origins, Local FS provides the following custom metrics:

*   Late Records meter and counter - The number of late records written to the local file system. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_localfilesystem_\
    LocalFileSystemDTarget_<library version>.lateRecords.\
    <counter | metric>
    ```
*   To HDFS Records meter and counter. The number of records written to the local file system. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_localfilesystem_\
    LocalFileSystemDTarget_<library version>.hdfsRecords.\
    <counter | metric>
    ```
*   Transfer Rate KB Meter - Displays the transfer rate in KB. Appears when the destination writes whole files to the destination system with the whole file data format. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_localfilesystem_\
    LocalFileSystemDTarget_<library version>.transferRateKb.meter
    ```

**MapR FS destination**

In addition to the standard metrics available for origins, MapR FS provides the following custom metrics:

*   Late Records meter and counter - The number of late records written to MapR FS. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_marpfs_\
    MaprFSDTarget_<library version>.lateRecords.<counter | metric>
    ```
*   To HDFS Records meter and counter. The number of records written to MapR FS. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_marpfs_\
    MaprFSDTarget_<library version>.hdfsRecords.<counter | metric>
    ```
*   Transfer Rate KB Meter - Displays the transfer rate in KB. Appears when the destination writes whole files to the destination system with the whole file data format. The counter displays in external tools as follows:

    ```
    sdc.pipeline.<pipeline name>.<pipeline revision>.custom.\
    com_streamsets_pipeline_stage_destination_marpfs_MaprFSDTarget_\
    <library version>.transferRateKb.meter
    ```

