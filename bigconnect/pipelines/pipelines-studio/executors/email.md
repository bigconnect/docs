# Email

The Email executor sends the configured email to the specified recipients upon receiving an event. You can also configure the executor to send an email based on a condition, such as the arrival of a specific type of event.

You can configure the Email executor to send multiple emails, each with its own condition, set of recipients, and email message. You can use expressions in all email fields.

Before you use an Email executor in a pipeline, you must enable Data Pipelines to send email. For more information, see Sending Email.

For a solution that describes how to use the Email executor, see Sending Email During Pipeline Processing.&#x20;

### Prerequisite <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you run a pipeline that includes the Email executor, define the email alert properties in the Data Pipelines configuration file, $SDC\_CONF/sdc.properties.

For more information about enabling Data Pipelines to send email, see Sending Email.

### Conditions <a href="#ariaid-title3" id="ariaid-title3"></a>

You can configure the Email executor to send an email based on a condition, such as the type of event to trigger the email. If you omit a condition, the Email executor sends the email each time it receives an event.

For example, say you have a pipeline that uses a JDBC Query Consumer origin. The origin generates several types of events: one for query success, one for query failure, and one for the processing of all available data. To have the Email executor send a email only when a query fails, you use the event type in the email condition, as follows:

```
${record:eventType() == 'jdbc-query-failure'} 
```

The event type is stored in the record header attribute of every event record. The expression language provides the `record:eventType()` function to return the event type for event records.

To determine the event type to use on the right side of the condition, review the "Event Record" documentation for the stage. In this case, jdbc-query-failure is the event type for JDBC Query Consumer query failure events.

Tip: By using conditions, you can route multiple event streams to the same Email executor and configure all event-driven email notification in the executor. For an example, see Sending Email During Pipeline Processing.

### Using Expressions <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use expressions in any email property in the Email executor. For example, you can base the condition on information in the event record, and include information about the pipeline in the email message.

You can use any functions that work for your use case, but here are some suggestions:pipeline functions

You can use the pipeline functions to provide pipeline information such as the pipeline name and ID.

For example, you can use `pipeline:title()` and `pipeline:id()` in the email message to indicate the pipeline that generated the event and email. You might use the following message when the email is configured to send after the Pipeline Finisher stops the pipeline:

```
Heads up! ${pipeline:title()}, ${pipeline:id()}, has successfully completed.
```

record functionsYou can use record functions to provide information from the event record. For example, you should use `record:eventType()` in the condition if the pipeline routes multiple types of events to the Email executor and you want to send the email only upon receiving a particular event type. If you use the executor with the JDBC Query Consumer, you can use the following condition to send the email when a query successfully completes:

```
${record:eventType() == 'jdbc-query-success'}
```

You might also use the `record:eventCreation()` function to include the time that the event occurred in the message. The creation time is returned in epoch time, so to create a readable timestamp, you can use the following expression:

```
${time:millisecondsToDateTime(record:eventCreation() * 1000)}
```

And of course, you can use the `record:value` function to include information from the event record, such as the query that successfully completed.

file functionsYou can use file functions to provide information about a closed or written file.For example, you might use the `file:fileName` function to extract the name of a closed file from the filepath field of a Hadoop FS file-closure event as follows:

```
${file:fileName(record:value('/filepath'))}
```

### Configuring an Email Executor <a href="#ariaid-title5" id="ariaid-title5"></a>

Configure an Email executor to send email upon receiving events.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Email tab, configure the following properties:

    | Email Configuration Property | Description                                                                                                                                                                                                                                                            |
    | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Condition                    | <p>Optional condition for when to send the email. Use a condition that evaluates to true or false. When the condition evaluates to true, the executor sends the configured email.</p><p>When not used, the executor sends an email each time it receives an event.</p> |
    | Email IDs                    | Email addresses to use. Click the Add icon to add additional recipients.                                                                                                                                                                                               |
    | Email Subject                | The information to display in the subject field of the email.                                                                                                                                                                                                          |
    | Email Body                   | The information to display in the body of the email.                                                                                                                                                                                                                   |

    Note: You can use expressions in all email configuration properties. For more information, see Using Expressions.
3. Using simple or bulk edit mode, click the Add icon to configure an additional email.
