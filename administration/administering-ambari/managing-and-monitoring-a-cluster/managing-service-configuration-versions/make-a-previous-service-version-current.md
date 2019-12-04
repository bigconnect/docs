# Make a previous service version current

Use **Make Current** to revert a cluster to operate using a previous configuration version.

**About this task  
\[SERVICE\_NAME\] &gt; Configs** displays specific property values from the current service configuration. **Configs** also supports comparing details from the current version with a previous one. All versions in a service configuration history display as version-numbered rows across the **Versions** menu. When browsing multiple config versions displayed in the **Versions** menu, click a version row to display options to view, compare, or make current. The **Make Current** option reverts the cluster to operate using a previous config version. Make Current creates \(clones\) a new, current service configuration version using the configuration properties from the older version you are comparing.

 From the **Version** list, click any version, then click **Make Current**.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_cnfig_hstry_3.png)

**After you finish**  
After initiating the **Make Current** operation, **Make Current Confirmation** prompts you to enter text that describes the new version. To confirm and save the new version after typing descriptive text, click **Make Current**.

