# Rules and Alerts

You can define rules to enable capturing information about a running pipeline. You can enable an alert for any rule to be notified when the specified condition occurs.

You can create the following types of rules:

* Metric rule - Gathers statistics about the pipeline such as pipeline idle time or error record counts. Provides an alert when enabled.
* Data rule - Gathers details about data as it passes between two stages. Can provide a meter and alert.
* Data drift rule - Gathers details about data drift as data passes between two stages. Can provide a meter and alert.

When you enable alerts, you can be informed of the alert in the following ways:

* Pipeline monitoring - All alerts display when you view the pipeline in Monitor mode.
* Webhooks - All alerts trigger all configured webhooks.
* Email - You can configure rules to send email alerts.

## Metric Rules and Alerts

Metric rules and alerts provide notifications about real-time statistics for pipelines.

When Data Pipelines runs a pipeline, the UI displays the pipeline in Monitor mode and displays real-time statistics about the pipeline. You can define and enable metric rules so that you are sent an alert when a statistic reaches a certain threshold.

For example, the Record Count and Record Throughput statistics in Monitor mode display the number of input, output, and error records that the pipeline has processed:

![](<../../../.gitbook/assets/image (756).png>)

You can view the number of error records in these statistics. However, you might want to be notified when the number of error records reaches a certain threshold. You can enable the default rule for the Pipeline Error Records Counter metric to send an alert when the pipeline encounters more than 100 error records.&#x20;

When you enable a metric rule, it automatically enables an alert for the rule. You can also configure a metric rule to send email alerts to all email addresses associated with the pipeline.

You configure metric rules when you configure or monitor the pipeline. Data Pipelines provides a set of default metric rules that you can edit and enable for any pipeline. Metric rules take effect after you enable them.

You can also create custom metric rules. When you create a custom metric rule, you select the metric type. The metric type determines which statistic triggers the alert. You configure the condition that triggers the alert, and enter the text to display in the alert.

## Default Metric Rules

Data Pipelines provides a set of default metric rules that you can edit and enable for any pipeline.

You might want to edit a default metric rule to modify the alert text or the condition for the rule. By default, none of the rules are enabled. Select Active to enable a rule.

Data Pipelines provides the following default metric rules:

![](<../../../.gitbook/assets/image (655).png>)

#### &#x20;Metric Types <a href="#ariaid-title5" id="ariaid-title5"></a>

You can use different metric types when you create a metric rule. The metric type determines which statistic triggers the alert.

After selecting a metric type, you select the metric ID which specifies the metric to use. For example, a metric ID can be a runtime statistics gauge or an input records meter. You then select the metric element that defines what the metric is measuring. A metric element can be a count, rate, median, minimum, maximum, or percentage. The possible metric ID and metric element vary by metric type.

**Gauge**

The gauge metric type provides alerts based on the number of input, output, or error records for the last processed batch. It also provides alerts on the age of the current batch, the amount of time a stage takes to process a batch, or the time that Data Pipelines last received a record from the origin.

The gauge metric type provides alerts about some of the Runtime Statistics displayed in Monitor mode:

The gauge metric type includes a single metric ID, Runtime Statistics Gauge. You can configure the alert to trigger on the following metric elements:

<figure><img src="https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/MetricGauge.png" alt=""><figcaption></figcaption></figure>

* Last Batch Input, Output, or Error Records Counts
* Last Batch Error Messages Count
* Current Batch Age
* Time in Current Stage
* Time of Last Received Record

For example, you can configure a gauge metric rule that triggers an alert when the pipeline has been processing a batch for more than 5 minutes.

**Counter**

The counter metric type provides alerts based on the number of input, output, or error records for the pipeline or for a stage in the pipeline.

The following counter metric displays the records processed by the selected stage:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/MetricCounter.png)

The counter metric type includes the following metric IDs:

* Pipeline batch count.
* Number of input records, output records, error records, or stage errors for the pipeline or for a stage in the pipeline.

For any of the selected metric IDs, you can configure the alert to trigger on the count metric element.

For example, you can configure a counter metric rule that triggers an alert when a pipeline encounters more than 1,000 error records.

**Histogram**

The histogram metric type provides alerts based on a histogram of different record types and stage errors for the pipeline or for a stage in the pipeline.

The histogram metric type provides alerts about the Records Per Batch Histogram statistics displayed in Monitor mode for the pipeline or for a stage :

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/MetricHistogram.png)

The histogram metric type includes metric IDs for the input records, output records, error records, or stage errors for the pipeline or for a stage in the pipeline. You can configure the alert to trigger on the metric elements displayed in the monitoring histogram: mean, standard deviation, percentage, or count.

For example, you can configure a histogram metric rule that triggers an alert when the mean of all input records processed by the pipeline reaches 10,000.

**Meter**

The meter metric type provides alerts based on rates of different record types and stage errors for pipelines or for a stage in the pipeline.

The meter metric type can provide alerts about the number of batches processed by the pipeline. The meter metric type can also provide alerts about the Record Count and Record Throughput statistics displayed in Monitor mode for the pipeline or for a stage:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/MetricStatistics.png)

The meter metric type includes metric IDs for the pipeline batch count and for the input records, output records, error records, or stage errors for the pipeline or for a stage in the pipeline. You can configure the alert to trigger on the following metric elements displayed in the Record Count and Record Throughput statistics: count, time rates, or mean.

For example, you can configure a meter metric rule that triggers an alert when the number of output records that a stage processes reaches 5,000 in one minute.

**Timer**

The timer metric type provides alerts based on batch processing timers for the pipeline or for a stage in the pipeline.

The timer metric type provides alerts about the Batch Throughput and Batch Processing Timer statistics displayed in Monitor mode for the pipeline or for a stage:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/MetricTimer.png)The timer metric type includes the following metric IDs:

* Pipeline Batch Processing Timer - Amount of time for the pipeline to process a batch.
* \<stage\_name> Batch Processing Timer - Amount of time for a stage to process a batch.

You can configure the alert to trigger on the following metric elements displayed in the Batch Processing Timer statistics: mean, standard deviation, percentage, time rates, or count.

For example, you can configure a timer metric rule that triggers an alert when the mean amount of time that the pipeline takes to process a batch reaches 10 minutes.

#### Metric Conditions <a href="#ariaid-title11" id="ariaid-title11"></a>

When you configure a metric rule, you configure the condition that defines the threshold at which the metric rule triggers an alert. Use the expression language to configure the condition.

The expression language provides the following functions for creating metric rule conditions:

value()Returns the value of the current metric selected in the metric rule. Use in conditions for any type of metric rule.For example, the default rule for the Pipeline Error Records Counter metric includes the following condition:

```
${value() > 100}
```

The alert is triggered when the pipeline encounters more than 100 error records.

time.now()Returns the current time of the Data Pipelines machine as a java.util.Date object. Use in conditions for gauge metric rules.For example, the default rule for the Runtime Statistics Gauge metric that checks whether the pipeline is idle includes the following condition:

```
${time:now() - value() > 120000}
```

The alert is triggered when the current time is greater than the time of the last received record by 120,000 milliseconds.

jvm:maxMemoryMB()Returns the Java heap size allocated to Data Pipelines in MB. Use in conditions for counter metric rules.For example, the default rule for the Pipeline Memory Consumption Counter metric includes the following condition:

```
${value() > (jvm:maxMemoryMB() * 0.65)}
```

The alert is triggered when the pipeline uses 65% of the Java heap size allocated to Data Pipelines.

#### Configuring a Metric Rule and Alert <a href="#ariaid-title12" id="ariaid-title12"></a>

Create a custom metric rule to receive alerts when a real-time statistic reaches a certain threshold. You can create metric rules and alerts when you configure or monitor a pipeline. You can edit or delete metric rules when they are not enabled.

Configure metric rules based on the metric type, the metric, an element associated with the metric, and a condition.

1. In the Properties panel or Monitor panel, click the Metric Rules tab, and click the Add icon.
2.  In the Metric Rule dialog box, configure the following properties:

    | Metric Rule Property                                                                                                                                     | Description                                                                                                                                                                                                          |
    | -------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Alert Text                                                                                                                                               | <p>Text to display when the alert is triggered.</p><p>Enter text that explains the reason for the alert. For example, "Over 1000 pipeline error records."</p>                                                        |
    | [Metric Type](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Alerts/RulesAlerts\_title.html#concept\_y4d\_tcw\_1r) | <p>Type of metric information the alert is based on:</p><ul><li>Gauge</li><li>Counter</li><li>Histogram</li><li>Meter</li><li>Timer</li></ul>                                                                        |
    | Metric ID                                                                                                                                                | Metric to use. Provides a list of available metrics based on the metric type.                                                                                                                                        |
    | Metric Element                                                                                                                                           | Metric element to use. Provides a list of available elements based on the metric ID.                                                                                                                                 |
    | Condition                                                                                                                                                | Condition to trigger the alert. Use the expression language to configure the condition.                                                                                                                              |
    | Send Email                                                                                                                                               | <p>Sends an email when the alert is triggered.</p><p>To send an email, add email addresses for alerts and configure the Data Pipelines email properties. For more information, see Configuring Email for Alerts.</p> |
3. Click Save.The new metric rule displays in the list.
4. To enable the rule, select Active.You can enable and disable email alerts from the list of rules.

## Data Rules and Alerts <a href="#ariaid-title13" id="ariaid-title13"></a>

Data rules define the information that you want to see about the data that passes between stages. You can create data rules based on any link in the pipeline. You can also enable metrics and create alerts for data rules.

When you click a link between two stages while monitoring a pipeline, the Monitor panel displays summary statistics, data rules, and information about the stream of data that the link represents. The summary statistics that display are based on the data rules that you create.

You can configure data rules when you configure or monitor the pipeline. To create a data rule, you need familiarity with the data being processed. You might preview data or take a snapshot of data to help determine how to configure data rules.

### Configuring a Data Rule and Alert <a href="#ariaid-title14" id="ariaid-title14"></a>

Create a data rule to view metrics, sample data, and alerts based on the rule. You can configure data rules and alerts when you configure or monitor a pipeline. You can edit or delete data rules when they are not enabled.

1. In the Properties panel or Monitor panel, click the Data Rules tab, and then click the Add icon.
2.  In the Data Rule dialog box, configure the following properties:

    | Data Rule Property         | Description                                                                                                                                                                                                                                                                                                                                       |
    | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Stream                     | Link selected for the data rule.                                                                                                                                                                                                                                                                                                                  |
    | Label                      | The label to display for the data rule.                                                                                                                                                                                                                                                                                                           |
    | Condition                  | Condition that defines the data rule. Use the expression language to configure the condition.                                                                                                                                                                                                                                                     |
    | Sampling Percentage        | Percentage of records to sample to generate information for the data rule. A higher percentage can provide greater accuracy but use more resources on the Data Pipelines machine.                                                                                                                                                                 |
    | Sampling Records to Retain | Number of sampled records to keep in memory for display.                                                                                                                                                                                                                                                                                          |
    | Enable Meter               | Enables gathering information for the data rule. The information gathered displays when you select the link in Monitor mode.                                                                                                                                                                                                                      |
    | Enable Alert               | Enables an alert based on the data rule. Alerts display when the configured conditions occur.                                                                                                                                                                                                                                                     |
    | Alert Text                 | <p>Text to display when the alert is triggered.</p><p>Enter text that explains the reason for the alert. For example, "Over 100 missing phone numbers."</p><p>You can use the expression language to define the alert text. For example, use the <code>record:errorMessage()</code> function to display the error message in the alert text. </p> |
    | Threshold Type             | <p>Type of threshold that defines when the alert becomes active:</p><ul><li>Count - A specified number of records.</li><li>Percentage - A specified percentage of records.</li></ul>                                                                                                                                                              |
    | Threshold Value            | Value that defines the threshold at which the rule triggers an alert.                                                                                                                                                                                                                                                                             |
    | Min Volume                 | Minimum number of records to process before evaluating a percentage threshold type.                                                                                                                                                                                                                                                               |
    | Send Email                 | <p>Sends an email when the alert is triggered.</p><p>To send an email, add email addresses for alerts and configure the Data Pipelines email properties. For more information, see Configuring Email for Alerts.</p>                                                                                                                              |
3. Click Save.The new data rule displays in the list.
4. To enable the new rule, click Active.You can enable and disable the meter and alert from the list of rules. You can edit and delete disabled rules.

### Viewing Data Rule Metrics and Sample Data <a href="#ariaid-title15" id="ariaid-title15"></a>

You can view the sample data generated by a data rule in Monitor mode. For data rules with metrics enabled, you can also view a graph that displays the metrics for the rule.

1. In Monitor mode, select the link with the data metrics that you want to view.
2. If necessary, in the Monitor panel, select the Summary tab. Sample data displays. When enabled for the rule, metrics display on the right.

## Data Drift Rules and Alerts <a href="#ariaid-title16" id="ariaid-title16"></a>

You can create data drift rules to indicate when the structure of data changes. You can create data drift rules on any link in the pipeline. You can also enable metrics and create alerts for data drift rules.

The expression language provides data drift functions for creating data drift rules. You can use specific field types with each function. The following table describes the type of data drift rules that you can generate on the different field types:

| Data Drift Rule     | Drift Function  | Valid Field Data Types               |
| ------------------- | --------------- | ------------------------------------ |
| Field name changes  | `drift:name()`  | <p>list-map</p><p>map</p>            |
| Field order changes | `drift:order()` | list-map                             |
| Number of fields    | `drift:size()`  | <p>list</p><p>list-map</p><p>map</p> |
| Field data type     | `drift:type()`  | any                                  |

You can view the metrics and sample records for data drift rules in the same way that you view data rule metrics and records. For more information, see Viewing Data Rule Metrics and Sample Data.

For details about the data drift functions, see Data Drift Functions.

### Data Drift Alert Triggers <a href="#ariaid-title17" id="ariaid-title17"></a>

Data drift alerts trigger when a change of the specified type occurs from record to record.

For example, you have an alert that triggers when the number of fields in the record changes. When processing the records with the following number of columns, an alert triggers for both the third and fourth records:

| RECORD NUMBER | NUMBER OF COLUMNS |
| ------------- | ----------------- |
| 1             | 10                |
| 2             | 10                |
| 3             | 15                |
| 4             | 10                |

Data drift functions include an ignoreWhenMissing flag to determine the behavior when the specified field does not exist. When the specified field is missing and ignoreWhenMissing is set to true, an alert is not triggered.

When the specified field is missing and the ignoreWhenMissing flag is set to false, the expression triggers an alert for the missing field, and again for the next record when the field is present.

For example, the following expression checks the data type of the ID column with ignoreWhenMissing set to false:

```
${drift:type('/UserID', false)}
```

Say all records include the UserID field, and then a single record passes without the UserID field. This expression triggers an alert for the record with the missing field, and again when the next record arrives that includes the UserID field.

### Configuring Data Drift Rules and Alerts <a href="#ariaid-title18" id="ariaid-title18"></a>

Create a data drift rule to view metrics, sample data, and alerts based on the rule. You can configure data drift rules and alerts when you configure or monitor a pipeline. You can edit or delete data drift rules when they are not enabled.

1. In the Properties panel or Monitor panel, click the Data Drift Rules tab, and then click the Add icon.
2.  In the Data Drift Rule dialog box, configure the following properties:

    | Data Rule Property         | Description                                                                                                                                                                                                                                 |
    | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Stream                     | Link selected for the data drift rule.                                                                                                                                                                                                      |
    | Label                      | The label to display for the data drift rule.                                                                                                                                                                                               |
    | Condition                  | Condition that defines the data drift rule. You can use data drift functions and other aspects of the expression language to configure the condition.                                                                                       |
    | Sampling Percentage        | Percentage of records to sample to generate information for the data drift rule. A higher percentage can provide greater accuracy but use more resources on the Data Pipelines machine.                                                     |
    | Sampling Records to Retain | Number of sampled records to keep in memory for display.                                                                                                                                                                                    |
    | Enable Meter               | Enables gathering information for the data drift rule. The information gathered displays when you select the link in Monitor mode.                                                                                                          |
    | Enable Alert               | Enables an alert based on the data drift rule. Alerts display upon when the configured conditions occur.                                                                                                                                    |
    | Alert Text                 | <p>Text to display when the alert is triggered. You can use the expression language to define the alert text. </p><p>By default, uses the following expression to return text related to the drift alert: <code>${alert:info()}</code>.</p> |
    | Send Email                 | <p>Sends an email when the alert is triggered.</p><p>To send an email, add email addresses for alerts and configure the Data Pipelines email properties. For more information, see Configuring Email for Alerts.</p>                        |
3. Click Save.The new data drift rule displays in the list.
4. To enable the new rule, click Active.You can enable and disable the meter and alert from the list of rules. You can edit and delete disabled rules.

## Alert Webhooks <a href="#ariaid-title19" id="ariaid-title19"></a>

You can configure webhooks that are sent when alerts are triggered. A webhook is a user-defined HTTP callback - an HTTP request that the pipeline sends automatically when certain actions occur. You can use webhooks to automatically trigger external tasks based on an HTTP request. Tasks can be as simple as sending a message through an application API or as powerful as passing commands to the Data Pipelines command line interface.

The pipeline sends all alert webhooks each time an alert is triggered. So when you configure an alert webhook, create a webhook payload that is applicable for all triggered alerts. You can configure a payload that includes the details of each alert.

Important: You must configure webhooks as expected by the receiving system. For details on how to configure incoming webhooks check the receiving system's documentation. You might also need to enable webhook usage within that system.

When you configure an alert webhook, you specify the URL to send the request and the HTTP method to use. Some HTTP methods allow you to include a request body or payload. In the payload, you can use parameters to include information about the cause of the trigger, such as the pipeline that triggered the alert and the alert details. You can also include request headers, content type, authentication type, username and password as needed.

For details about webhook methods, payloads and parameters, see Webhooks.

### Configuring an Alert Webhook <a href="#ariaid-title20" id="ariaid-title20"></a>

Configure an alert webhook to automatically send an HTTP request each time the pipeline triggers an alert.

1. To view pipeline configuration options, click an unused section of the pipeline canvas.
2. In the Properties panel, click Rules, and then click Webhooks.
3.  On the Webhooks tab, configure the following properties:

    | Webhook Property    | Description                                                                                                                                                                                                                                                                                                                                                 |
    | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Webhooks            | Webhook to send when an alert triggers. Using simple or bulk edit mode, click the Add icon to add additional webhooks.                                                                                                                                                                                                                                      |
    | Webhook URL         | URL to send the HTTP request.                                                                                                                                                                                                                                                                                                                               |
    | Headers             | Optional HTTP request headers.                                                                                                                                                                                                                                                                                                                              |
    | HTTP Method         | <p>HTTP method. Use one of the following methods:</p><ul><li>GET</li><li>PUT</li><li>POST</li><li>DELETE</li><li>HEAD</li></ul>                                                                                                                                                                                                                             |
    | Payload             | <p>Optional payload to include. Available for PUT, POST, and DELETE methods.</p><p>Use any valid content type.</p><p>You can use webhook parameters in the payload to include information about the triggering event, such as the alert name or condition. Enclose webhook parameters in double curly brackets as follows: <code>{{ALERT_NAME}}</code>.</p> |
    | Content Type        | Optional content type of the payload. Configure this property when the content type is not declared in the request headers.                                                                                                                                                                                                                                 |
    | Authentication Type | <p>Optional authentication type to include in the request. Use None, Basic, Digest, or Universal.</p><p>Use Basic for Form authentication.</p>                                                                                                                                                                                                              |
    | User Name           | User name to include when using authentication.                                                                                                                                                                                                                                                                                                             |
    | Password            | Password to include when using authentication.                                                                                                                                                                                                                                                                                                              |
4. To create an additional webhook, click the Add icon.

## Configuring Email for Alerts <a href="#ariaid-title21" id="ariaid-title21"></a>

You can define the email addresses to receive metric and data alerts. When an alert triggers an email, the Data Pipelines sends an email to every address in the list.

To send email alerts, create an email account to send the alerts and define the email alert properties in the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`.

1. To view pipeline configuration options, click an unused section of the pipeline canvas.
2. In the Properties panel, click Rules, and then click Email IDs.
3. Enter an email address where you want to send email alerts.The email address receives every email alert generated by the pipeline.
4. To add additional email addresses, click the Add icon, and enter additional email addresses.
