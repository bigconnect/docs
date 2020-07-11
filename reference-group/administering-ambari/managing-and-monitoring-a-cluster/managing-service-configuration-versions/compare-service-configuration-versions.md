# Compare service configuration versions

Use **Compare Versions** to display differences between the current configuration version and an older one.

**About this task  
Services &gt; \[SERVICE\_NAME\] &gt; Configs** displays specific property values from the current service configuration. **Configs** also supports comparing details from the current version with a previous one. All versions of a service configuration history display as version-numbered rows in the **Versions** menu. When browsing multiple config versions displayed in the **Versions** menu, hover your cursor over a version block to display options to view, compare, or make current.

**Steps**

1. While details of the current version display on **Configs**, find a version you want to compare with the current one, using the **Versions** menu. For example, if you want to compare the current version 7 to 6, find version 6 in the **Versions** menu. 
2. In the **Versions** menu, hover your cursor over the V2 block to display options, then click ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_compare_config_versions.png).**Configs** displays a comparison of V6 to V7. Specific differences appear in the main window.  
   ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_compare_config_versions_full.png)  


   In this example, V6 differs from V7 by nine properties.  

3. To filter the comparison, expand the **Filter** menu, and select an option. For example, to display only properties with changes between two versions, in **Filters**, click **Changed Properties**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_cmpare_vrsns_2.png) 
4. To close **Comparing Changes**, click the X.![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_close_compare_version_cnfigs.png)

**After you finish**  
If you want to revert the service to use the older version settings, click **Make \[PREVIOUS\_VERSION\] Current**.

