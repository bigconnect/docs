# Managing Output Files

This solution describes how to design a pipeline that writes output files to a destination, moves the files to a different location, and then changes the permissions for the files.

Let's say that your pipeline writes data to Hadoop Distributed File System (HDFS). By default, the Hadoop FS destination creates a complex set of directories for output files and late record files, keeping files open for writing based on stage configuration. That's great, but once the files are complete, you'd like the files moved to a different location. And while you're at it, it would be nice to set the permissions for the written files.

So what do you do?

Add an event stream to the pipeline to manage output files when the Hadoop FS destination is done writing them. Then, use the HDFS File Metadata executor in the event stream.

Note: If you prefer, instead of using the Hadoop FS destination with the HDFS File Metadata executor, you can use the Local FS destination with the HDFS File Metadata executor.

Here's a pipeline that reads from a database using JDBC, performs some processing, and writes to HDFS:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Move-BasicPipe.png)

1.  To add an event stream, first configure Hadoop FS to generate events:

    On the General tab of the Hadoop FS destination, select the Produce Events property.

    Now, the event output stream becomes available, and Hadoop FS generates an event record each time it closes an output file. The Hadoop FS event record includes fields for the file name, path, and size.

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Move-HDFS.png)
2.  Connect the Hadoop FS event output stream to a HDFS File Metadata executor.

    Now, each time the HDFS File Metadata executor receives an event, it triggers the tasks that you configure it to run.

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Move-HDFSMetadata.png)
3.  Configure the HDFS File Metadata executor to move the files to the directory that you want and set the permissions for the file.

    In the HDFS File Metadata executor, configure the HDFS configuration details on the HDFS tab. Then, on the Tasks tab, select Change Metadata on Existing File configure the changes that you want to make.

    In this case, you want to move files to /new/location, and set the file permissions to 0440 to allow the user and group read access to the files:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Move-FileMetadata-props.png)

With this event stream added to the pipeline, each time the Hadoop FS destination closes a file, it generates an event record. When the HDFS File Metadata executor receives the event record, it moves the file and sets the file permissions. No muss, no fuss.
