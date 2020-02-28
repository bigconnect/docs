# Restart all required services

As prompted in Ambari Web, restart all required services whenever you update configuration properties.

**About this task**  
 When you update service configuration settings in Ambari, Ambari marks the services that require a restart with a prompt similar to this:  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_restart_prompt.png)  
To restart all required services:  
  
**Steps**

1. In **Ambari Web**, click **Services...**. 
2. Click **Restart All Required**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_restart_all_required.png)

{% hint style="danger" %}
**Important**  
Apache Oozie requires a restart after an Atlas configuration update, but might not be marked as requiring restart in Ambari. If Oozie is not included, follow these steps to restart Oozie:
{% endhint %}

* In **Ambari Web**, click **Oozie** in the services summary pane on the left of the display. 
* Click **Service Actions &gt; Restart All**.

