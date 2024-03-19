# Audit Configuration Reference

Configuration settings for audits are available at a system scope. You can find these settings by opening the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigating to _Control Panel_ → _System Settings_ → _Audit_.

### [Audit](https://learn.liferay.com/web/guest/w/dxp/system-administration/audit-framework/audit-configuration-reference#audit) <a href="#audit" id="audit"></a>

![Enable or disable audit logging for your instance entirely.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/audit-framework/audit-configuration-reference/images/01.png)

Use this checkbox to enable or disable auditing entirely for your Liferay instance. Note that when auditing is enabled, you can adjust the _Audit Message Maximum Queue Size_ field from its default value.

### [CSV Logging Message Formatter](https://learn.liferay.com/web/guest/w/dxp/system-administration/audit-framework/audit-configuration-reference#csv-logging-message-formatter) <a href="#csv-logging-message-formatter" id="csv-logging-message-formatter"></a>

![Control the number of columns in the exported CSV file.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/audit-framework/audit-configuration-reference/images/02.png)

Use this to define the columns present in the CSV export of audit events. Note, you cannot add new columns. You can only remove the ones present or add them back after removal.

### [Logging Message Audit Message Processor](https://learn.liferay.com/web/guest/w/dxp/system-administration/audit-framework/audit-configuration-reference#logging-message-audit-message-processor) <a href="#logging-message-audit-message-processor" id="logging-message-audit-message-processor"></a>

![Enable logging of events to Liferay DXP's log and console.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/audit-framework/audit-configuration-reference/images/03.png)

Follow these steps to configure the reporting of log events in Liferay DXP’s log and console:

1. Select the _Enabled_ checkbox to report audit events in Liferay DXP’s log.
2. Select the _Output to Console_ checkbox to report audit events in the console.
3. In the _Log Message Format_ dropdown, select the format for the audit events (CSV or JSON).
4. Click _Save_.

### [Persistent Message Audit Message Processor](https://learn.liferay.com/web/guest/w/dxp/system-administration/audit-framework/audit-configuration-reference#persistent-message-audit-message-processor) <a href="#persistent-message-audit-message-processor" id="persistent-message-audit-message-processor"></a>

![Enable this setting to write audit logs to the database.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/audit-framework/audit-configuration-reference/images/04.png)

Enable this setting to write audit messages to the database. The interface uses data from the database to render audit messages. Disabling it causes the interface to be empty.

### [Audit Log Context](https://learn.liferay.com/web/guest/w/dxp/system-administration/audit-framework/audit-configuration-reference#audit-log-context) <a href="#audit-log-context" id="audit-log-context"></a>

![Add additional audit information to Log4J logs.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/audit-framework/audit-configuration-reference/images/05.png)

Use this to add audit information, in JSON format, to every log message written by Log4J. You must enable it and configure the Log4J logging via an XML file.

When enabling _Use Incoming X-Request-ID_, a client can send the `X-Request-ID` header, and Liferay tags every log statement with this value. This allows tracing of specific requests through multiple systems.

### [Configuring Audit Events for Scheduled Jobs](https://learn.liferay.com/web/guest/w/dxp/system-administration/audit-framework/audit-configuration-reference#configuring-audit-events-for-scheduled-jobs) <a href="#configuring-audit-events-for-scheduled-jobs" id="configuring-audit-events-for-scheduled-jobs"></a>

By default, Liferay’s scheduled jobs don’t trigger audit events. To change it,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _System Settings_.
2. Select _Infrastructure_ from the _Platform_ section.
3. Under _System Scope_ on the left, select _Scheduler Engine Helper_.
4. Enable the checkbox for _Audit Scheduler Job Enabled_.
5. Click _Save_.

\
