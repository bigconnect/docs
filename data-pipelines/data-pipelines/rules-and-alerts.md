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

When Data Collector runs a pipeline, the UI displays the pipeline in Monitor mode and displays real-time statistics about the pipeline. You can define and enable metric rules so that you are sent an alert when a statistic reaches a certain threshold.

For example, the Record Count and Record Throughput statistics in Monitor mode display the number of input, output, and error records that the pipeline has processed:

![](../../.gitbook/assets/image%20%28255%29.png)

You can view the number of error records in these statistics. However, you might want to be notified when the number of error records reaches a certain threshold. You can enable the default rule for the Pipeline Error Records Counter metric to send an alert when the pipeline encounters more than 100 error records. When you enable the metric rule and the alert triggers, the alert displays in the Data Collector UI as follows:

![](../../.gitbook/assets/image%20%28455%29.png)

When you enable a metric rule, it automatically enables an alert for the rule. You can also configure a metric rule to send email alerts to all email addresses associated with the pipeline.

You configure metric rules when you configure or monitor the pipeline. Data Collector provides a set of default metric rules that you can edit and enable for any pipeline. Metric rules take effect after you enable them.

You can also create custom metric rules. When you create a custom metric rule, you select the metric type. The metric type determines which statistic triggers the alert. You configure the condition that triggers the alert, and enter the text to display in the alert.

## Default Metric Rules

Data Collector provides a set of default metric rules that you can edit and enable for any pipeline.

You might want to edit a default metric rule to modify the alert text or the condition for the rule. By default, none of the rules are enabled. Select Active to enable a rule.

Data Collector provides the following default metric rules:

![](../../.gitbook/assets/image%20%28160%29.png)

