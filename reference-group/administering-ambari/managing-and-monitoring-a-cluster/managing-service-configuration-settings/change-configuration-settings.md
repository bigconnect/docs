# Change configuration settings

Use **Services &gt; \[SERVICE\_NAME\] &gt; Configs** to optimize service performance for the service.

**About this task**  
The **Configs** page includes several tabs you can use to manage configuration versions, groups, settings, properties and values. You can adjust settings called Smart Configs that control at a macro-level, memory allocation for each service. Adjusting Smart Configs requires related configuration settings to change throughout your cluster. Ambari prompts you to review and confirm all recommended changes and restart affected services.

**Steps**

1. In **Ambari Web**, click a service name in the service summary list on the left. 
2. From the the service **Summary** page, click the **Configs** tab, then use one of the following tabs to manage configuration settings.

   * Use the **Configs** tab to manage configuration versions and groups.
   * Use the **Settings** tab to manage Smart Configs by adjusting the green, slider buttons.
   * Use the **Advanced** tab to edit specific configuration properties and values.

  
   ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_service_configs_hdfs.png)  

3. Click **Save**. 
4. Enter a description for this configuration version that includes your current changes. 
5. Review and confirm each recommended change.

**After you finish**  
Restart all affected services.

