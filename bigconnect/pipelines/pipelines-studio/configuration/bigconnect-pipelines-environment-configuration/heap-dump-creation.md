# Heap Dump Creation

By default, when BigConnect Pipelines encounters an out of memory error (OOME), it creates a heap dump.

By default, heap dump files are written to the file defined in the SDC\_LOG environment variable and use a naming convention that allows generating multiple heap dump files, as follows: `$SDC_LOG/sdc_heapdump_${timestamp}.hprof`.

You can change the name of the heap dump files, but we recommend using the `${timestamp}` or similar variable to ensure that the heap dump name is unique.

Note that Java Virtual Machine, and therefore BigConnect Pipelines, does not overwrite existing heap dump files. For example, if you use `$SDC_LOG/sdc_heapdump.hprof` as the file name, after BigConnect Pipelines creates the first heap dump file, it will not create another until you remove the existing file.

**Note**: Depending on the number and size of the generated heap dump files, you might want to increase the BigConnect Pipelines Java heap size.\
\
You can configure the following heap dump environment variables:

| Heap Dump Environment Variable | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| SDC\_HEAPDUMP\_ON\_OOM         | <p>Specifies whether BigConnect Pipelines generates a heap dump upon encountering an out of memory error.<br></p><p>Default is true.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| SDC\_HEAPDUMP\_PATH            | <p>Specifies the file name and location to use for heap dump files.<br></p><p>By default, heap dumps are written to <code>$SDC_LOG/sdc_heapdump_${timestamp}.hprof</code>.<br></p><p>To specify a different file name or location, uncomment the property and enter the location and file name to use.<br></p><p><strong>Tip</strong>: To write multiple heap dump files to a directory, use a function or variable to ensure that the file name is unique. If a file of the same name exists in the directory, BigConnect Pipelines does not create a new heap dump file.</p> |

Modify environment variables using the method required by your installation type.
