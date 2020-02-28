# Customize notification templates

The notification template content produced by Ambari is tightly coupled to a notification type. Email and SNMP notifications both have customizable templates that you can use to generate content. You can change the template used by Ambari when creating alert notifications.

**About this task**  
Alert Template XML Location

An `alert-templates.xml` file ships with Ambari. This file contains all of the templates for every known type of notification, such as EMAIL and SNMP. This file is bundled in the Ambari server .jar file so that the template is not exposed on the disk; however, that file is used in the following example. When you customize the alert template, you are effectively overriding the default alert template's XML.

**Steps**

1. On the Ambari server host, browse to `/etc/ambari-server/conf` directory. 
2. Edit the ambari.properties file. 
3. Add an entry for the location of your new template. `alerts.template.file=/foo/var/alert-templates-custom.xml` 
4. Save the file. 
5. Restart Ambari server.

**Example**  
After you restart Ambari, any notification types defined in the new template override those bundled with Ambari. If you choose to provide your own template file, you only need to define notification templates for the types that you wish to override. If a notification template type is not found in the customized template, Ambari will default to the templates that ship with the JAR.

**Example**  
Alert Template XML StructureThe structure of the template file is defined as follows. Each `<alert-template>` element declares what type of alert notification it should be used for.

```text
<alert-templates>
  <alert-template type="EMAIL">
    <subject>
    Subject Content
    </subject>
    <body>
    Body Content
   </body>
  </alert-template>
  <alert-template type="SNMP">
    <subject>
    Subject Content
    </subject>
    <body>
    Body Content
   </body>
  </alert-template>
</alert-templates> 
```

**Example**  
Template Variables

The template uses Apache Velocity to render all tokenized content. The following variables are available for use in your template:  
  
**$alert.getAlertDefinition\(\)**   
The definition of which the alert is an instance.   
  
**$alert.getAlertText\(\)**   
The specific alert text.   
  
**$alert.getAlertName\(\)**   
The name of the alert.   
  
**$alert.getAlertState\(\)**   
The alert state \(OK, WARNING, CRITICAL, or UNKNOWN\)   
  
**$alert.getServiceName\(\)**   
The name of the service that the alert is defined for.   
  
**$alert.hasComponentName\(\)**   
True if the alert is for a specific service component.   
  
**$alert.getComponentName\(\)**   
The component, if any, that the alert is defined for.   
  
**$alert.hasHostName\(\)**   
True if the alert was triggered for a specific host.   
  
**$alert.getHostName\(\)**   
The hostname, if any, that the alert was triggered for.   
  
**$ambari.getServerUrl\(\)**   
The Ambari Server URL.   
  
**$ambari.getServerVersion\(\)**   
The Ambari Server version.   
  
**$ambari.getServerHostName\(\)**   
The Ambari Server hostname.   
  
**$dispatch.getTargetName\(\)**   
The notification target name.   
  
**$dispatch.getTargetDescription\(\)**   
The notification target description.   
  
**$summary.getAlerts\(service,alertState\)**   
A list of all alerts for a given service or alert state \(OK\|WARNING\|CRITICAL\|UNKNOWN\)   
  
**$summary.getServicesByAlertState\(alertState\)**   
A list of all services for a given alert state \(OK\|WARNING\|CRITICAL\|UNKNOWN\)   
  
**$summary.getServices\(\)**   
A list of all services that are reporting an alert in the notification.   
  
**$summary.getCriticalCount\(\)**   
The CRITICAL alert count.   
  
**$summary.getOkCount\(\)**   
The OK alert count.

**$summary.getTotalCount\(\)**   
The total alert count.   
  
**$summary.getUnknownCount\(\)**   
The UNKNOWN alert count.   
  
**$summary.getWarningCount\(\)**   
The WARNING alert count.   
  
**$summary.getAlerts\(\)**   
A list of all of the alerts in the notification.

**Example**  
Example: Modify Alert EMAIL Subject  
The following example illustrates how to change the subject line of all outbound email notifications to include a hard-coded identifier:

1. Download the alert-templates.xml code as your starting point. 
2. On the Ambari Server, save the template to a location, such as: `/var/lib/ambari-server/resources/alert-templates-custom.xml` 
3.  Edit the [alert-templates-custom.xml](https://github.com/apache/ambari/blob/branch-2.1/ambari-server/src/main/resources/alert-templates.xml) file and modify the subject link for the `<alert-template type="EMAIL">` template:

```text
<subject>
 <![CDATA[Petstore Ambari has $summary.getTotalCount() alerts!]]>
</subject>
```

4. Save the file.

5. Browse to `/etc/ambari-server/conf` directory.

6. Edit the ambari.properties file.

7. Add an entry for the location of your new template file.  
`alerts.template.file=/var/lib/ambari-server/resources/alert-templates-custom.xml`

8. Save the file.

9. Restart Ambari server.

