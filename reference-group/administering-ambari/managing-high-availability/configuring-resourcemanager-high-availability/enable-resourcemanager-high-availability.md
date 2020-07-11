# Enable ResourceManager high availability

To access the wizard and enable ResourceManager high availability:  
  
**About this task**  
You can configure high availability for ResourceManager by using the Enable ResourceManager HA wizard.

Before you begin  
  
You must have at least three:

* hosts in your cluster
* Apache ZooKeeper servers running

**Steps**

1. In **Ambari Web**, browse to **Services &gt; YARN &gt; Summary**. 
2. Select **Service Actions** and choose **Enable ResourceManager HA**. The Enable ResourceManager HA wizard launches, describing a set of automated and manual steps that you must take to set up ResourceManager high availability. 
3. On **Get Started**, read the overview of enabling ResourceManager HA.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_rmha-01_get_started.png) Click **Next** to proceed. 
4. On Select Host, accept the default selection or choose an available host.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_rmha-02_select_host.png)  Click **Next** to proceed. 
5. On **Review Selections**, expand YARN if necessary, to review all the configuration changes proposed for YARN.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_rmha-03_review.png)  Click **Next** to approve the changes and start automatically configuring ResourceManager HA. 
6. On **Configure Components**, click **Complete** when all the progress bars finish tracking.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_rmha-04_configure_components.png)

