# View service configuration history

Ambari saves previous versions of configurations and provides access to them for comparision and revert.

**About this task**  
**Ambari Web** provides two ways to view your configuration change history. The **Dashboard** page includes the **Config History** tab. Each service page provides a **Configs** tab, specific to that service. Using **Configs** enables you to quickly access the most recent changes to a service configuration.

* From the **Dashboard**, click **Config History** ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/concepts/images/amb_cnfig_hstry_1.png) **Config History** shows all versions across all services, with each version number and the date and time the version was created. You can also see which user authored the change, and any notes about the change. Use controls on **Config History** to filter, sort, and search across versions. 
* For a selected service, click **Configs**, then expand **Version**: ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/concepts/images/amb_cnfig_hstry_2.png)  **Configs** shows you details of the most recent configuration version by default. Use the version scrollbar to see previous config versions. 
* From the **Version** list, click any version in to view detailed information.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/concepts/images/amb_cnfig_hstry_3.png) 
* Click one of the following options:

  | **Select Option** | T**o:** |
  | :--- | :--- |
  | **Click a Version** | display detailed information for the selected version in Configs |
  | ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/concepts/images/amb_compare_config_versions.png) | display and compare two versions |
  | ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/concepts/images/amb_config_version_make_current.png) | apply configs from the currently selected version to the cluster |

**After you finish**  
If you choose **Make Curren**t, restart any affected services after changing versions.  


