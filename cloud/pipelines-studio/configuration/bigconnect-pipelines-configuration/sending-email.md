# Sending Email

You can configure email configuration properties to enable BigConnect Pipelines to send email notifications.

**Note**: Not valid in BigConnect Pipelines Edge pipelines. BigConnect Pipelines Edge pipelines do not send email.

For BigConnect Pipelines, BigConnect Pipelines can send email in the following ways:

* **Email alert** - Sends a basic email when an email-enabled alert is triggered, such as when the error record threshold has been reached.
* **Pipeline notification** - Sends a basic email when the pipeline state changes to a specified state. For example, you might use pipeline notification to send an email when a pipeline transitions to a Run\_Error or Finished state.
*   **Email executor** - Sends a custom email upon receiving an event from an event-generating stage. Use in an event stream to send a user-defined email. You can include expressions to provide information about the pipeline or event in the email.

    For example, you might use an Email executor to send an email upon receiving a failed-query event from the Hive Query executor, and you can include the failed query in the message.

To enable sending email, in the BigConnect Pipelines configuration file, configure the mail.transport.protocol property, and then configure the smtp/smtps properties and the xmail properties. For more information, see Configuring BigConnect Pipelines.
