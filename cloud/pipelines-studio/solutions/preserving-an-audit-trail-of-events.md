# Preserving an Audit Trail of Events

This solution describes how to design a pipeline that preserves an audit trail of pipeline and stage events that occur.

Store event records to preserve an audit trail of the events that occur. You can store event records from any event-generating stage. For this solution, say you want to keep a log of the files written to HDFS by the following pipeline:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Storage.png)

To do this, you simply:

1.  Configure the Hadoop FS destination to generate events.

    On the General tab, select the Produce Events property

    Now the event output stream becomes available, and the destination generates an event each time it closes a file. For this destination, each event record includes fields for the file name, file path, and size of the closed file.

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Storage-HDFS.png)
2.  You can write the event records to any destination, but let's assume you want to write them to HDFS as well:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Storage-HDFS-2.png)

    You could be done right there, but you want to include the time of the event in the record, so you know exactly when the Hadoop FS destination closed a file.
3.  All event records include the event creation time in the sdc.event.creation\_timestamp record header attribute, so you can add an Expression Evaluator to the pipeline and use the following expression to include the creation time in the record:

    ```
    ${record:attribute('sdc.event.creation_timestamp')}
    ```

    The resulting pipeline looks like this:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Storage-EEval.png)

    Note that event creation time is expressed as an epoch or Unix timestamp, such as 1477698601031. And record header attributes provide data as strings.
