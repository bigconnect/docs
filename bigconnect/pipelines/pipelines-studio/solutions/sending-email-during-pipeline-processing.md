# Sending Email During Pipeline Processing

This solution describes how to design a pipeline to send email notifications at different moments during pipeline processing.

You can configure a pipeline to send email upon pipeline state change and upon triggering an alert. Both methods can be useful in their own way. In this solution, we'll use an Email executor to send email upon receiving an event.

Say you have a pipeline that reads from a database using the JDBC Query Consumer origin, uses a Jython Evaluator to evaluate the data and generate events for invalid transactions, and writes to HDFS. You want this pipeline to send two types of email: one when the Jython Evaluator finds an invalid transaction and one when the JDBC Query Consumer fails to complete a query.

To do this, you simply route the events from the origin and processor to the Email executor and configure two email messages. The Email executor allows you to specify a condition for sending email and to use expressions to create customized email that provides event-related information to the recipient.

Say this is the original pipeline:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Email-Pipe.png)

1.  First, configure the JDBC Query Consumer to generate events.

    On the General tab of the origin, select the Produce Events property.

    Now, the event output stream becomes available. Note that the JDBC Query Consumer generates several types of events. You know this because you checked the [Event Record](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/JDBCConsumer.html#concept\_rzl\_s1t\_kz) section of the JDBC Query Consumer documentation. Every event-generating stage has event details in a similar section.
2.  Now configure the Jython Evaluator to generate events in the same way.

    Of course, the Jython Evaluator will only generate events if your script is set up to do so. But you also need to enable the Produce Events property on the General tab.
3.  Connect both event streams to an Email executor.

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Email-ExecutorPipe.png)
4.  Now since the JDBC Query Consumer generates several types of events, you need to configure the Email executor to send the first email only after receiving a query failure event:

    1.  The query failure event has the jdbc-query-failure event type, so on the Email tab, you use the following condition:

        ```
        ${record:eventType() == 'jdbc-query-failure'}
        ```
    2.  All of the email properties allow expressions, so for the email subject, you might include the pipeline name as follows:

        ```
        Query failure in ${pipeline:title()}!
        ```
    3.  When you compose the email body, you might use additional expressions to include information about the pipeline and information included in the event record in the email.

        Remember, the Event Record documentation lists all header attributes and fields in the event record, so you can refer to it when configuring the email.\


        For this email, you might include the following information:

        ```
        Pipeline ${pipeline:title()} encountered an error. 

        At ${time:millisecondsToDateTime(record:eventCreation() * 1000)}, the JDBC Query
        Consumer failed to complete the following query: ${record:value('/query')}

        Only the following number of rows were processed: ${record:value('/row-count')} 
        ```

    The email configuration looks like this:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Email-Origin.png)
5.  Click the Add icon to configure the email for the Jython Evaluator events.

    Since you want to send an email for each event that the Jython Evaluator generates, for the condition, you can use the event type defined in the script. Let's say it's "invalidTransaction". As with the first email, you can include additional information about the pipeline and data from the event record in the email body, as follows:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Email-Jython.png)

When you run the pipeline, the specified email recipients receive custom messages each time the Email executor receives the specified events. And the email recipients can act on the information included in the email without further ado.
