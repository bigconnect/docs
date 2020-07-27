# Webhooks

You can configure a pipeline to use webhooks.

A webhook is a user-defined HTTP callback - an HTTP request that the pipeline sends automatically when certain actions occur. You can use webhooks to automatically trigger external tasks based on an HTTP request. Tasks can be as simple as sending a message through an application API or as powerful as passing commands to the Data Collector command line interface.

You can configure the following types of webhooks:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Trigger</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Configuration Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Alert</td>
      <td style="text-align:left">
        <p>The pipeline sends all alert webhooks each time an alert is triggered.</p>
        <p>For example, if your text message application has a webhook API, you can
          have the pipeline send texts when alerts are triggered..</p>
      </td>
      <td style="text-align:left">Pipeline Rules tab &gt; Webhook tab</td>
    </tr>
    <tr>
      <td style="text-align:left">State notification</td>
      <td style="text-align:left">
        <p>The pipeline sends all state notification webhooks when the pipeline transitions
          to the selected pipeline states.</p>
        <p>For example, you can send an HTTP request to the Data Collector REST API
          to start a different pipeline when the current pipeline transitions to
          a Finished state.</p>
      </td>
      <td style="text-align:left">Pipeline Configuration tab &gt; Notifications tab</td>
    </tr>
  </tbody>
</table>

The flexibility of webhooks enables you to automatically trigger a wide range of tasks, depending on the ability of external systems to support webhooks or process HTTP requests.

{% hint style="warning" %}
You must configure webhooks as expected by the receiving system. For details on how to configure incoming webhooks check the receiving system's documentation. You might also need to enable webhook usage within that system.
{% endhint %}

When you configure a webhook, you specify the URL to send the request and the HTTP method to use. Some HTTP methods allow you to include a request body or payload. In the payload, you can use parameters to include information about the cause of the trigger, such as the text of the alert or the latest pipeline state. You can also include request headers, content type, authentication type, username and password as needed.

## Request Method

You can use the following methods in webhooks:

* GET
* PUT
* POST
* DELETE
* HEAD

## Payload and Parameters

You can include a request body or payload for PUT, POST, and DELETE request webhooks. Depending on the receiving system, the payload might not be used. For example, when using the Data Collector REST API, you simply include all required information in the URL.

When you configure a payload, you can use any valid content type, then specify the content type in the webhook properties. When defining a message, be sure to consider when the pipeline sends the webhook and include the information that the recipient needs.

You can use parameters in the payload to include information about the action that triggered the webhook. For example, when configuring a webhook that sends a text message when a pipeline stops, you might include the pipeline name, pipeline state, and the time parameters in the message.Enclose parameters in double curly brackets, as follows:

`{{<parameter name>>}}`

You can use the following parameters in webhooks:

**state notification parameters**

When configuring a state notification webhook, you can use the following parameters:

* PIPELINE\_TITLE - The pipeline title or name.
* PIPELINE\_URL - The direct URL to the pipeline.
* PIPELINE\_STATE - The current state of the pipeline.
* TIME - The time of the triggered request.
* PIPELINE\_STATE\_MESSAGE - The pipeline state and any related information, such as the last-saved offset or the error message for error states.
* PIPELINE\_INPUT\_RECORDS\_COUNT - The number of records read.
* PIPELINE\_OUTPUT\_RECORDS\_COUNT - The number of records written.
* PIPELINE\_ERROR\_RECORDS\_COUNT - The number of error records.
* PIPELINE\_ERROR\_MESSAGES\_COUNT - The number of error messages.
* PIPELINE\_RUNTIME\_PARAMETERS - The number of pipeline parameters.
* PIPELINE\_METRICS - Metrics data for the pipeline.

**alert parameters**

When configuring an alert webhook, you can use the following parameters:

* ALERT\_CONDITION - The condition of the rule associated with the alert.
* ALERT\_NAME - The alert label or name.
* ALERT\_TEXT - The text configured for the alert.
* ALERT\_VALUE - The value that triggered the condition. For example, if the alert is configured to trigger upon reaching 1000 error records, the alert value will be 1000.
* PIPELINE\_TITLE - The pipeline title.
* PIPELINE\_URL - The direct URL to the pipeline.
* TIME - Time of the triggered request.

