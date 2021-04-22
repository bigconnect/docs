# Notifications

You can configure a pipeline to send an email or webhook when the pipeline changes to specified states.

For example, you might send an email when someone manually stops the pipeline, causing it to transition to a Stopped state. Or you might send a Slack or text message when the pipeline changes to a Start\_Error or Run\_Error state.

You can send notifications when the pipeline converts to the following states:

* Running
* Start\_Error
* Run\_Error
* Running\_Error
* Stopped
* Finished
* Disconnected
* Connecting
* Stop\_Error

You can specify multiple states to trigger notifications, but you cannot configure the pipeline to send different notifications based on different pipeline state changes at this time. For example, if you configure notifications for the Running and Finished states, the pipeline sends notifications when changing to both states.

 However, when configuring a webhook, you can use webhook parameters in the payload to indicate the state change that triggered the notification. For more information about webhooks, see [Webhooks](dc-webhooks.md).

To send email notification, Data Pipelines must be configured to send email. To enable sending email, configure the email alert Data Pipelines properties.

For more information about pipeline states, see [Understanding Pipeline States](../pipeline-concepts-and-design/understanding-pipeline-states.md).

