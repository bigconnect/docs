# Upgrade an Installation with Cloudera Manager

## Upgrade an Installation with Cloudera Manager <a href="#ariaid-title1" id="ariaid-title1"></a>

When you upgrade an installation with Cloudera Manager, the new version uses the same configuration, data, log, and resource directories. As a result, the new version has access to the files created in the previous version.

**Note**: If you installed external libraries or developed custom stages, verify that those libraries are stored in a local directory external to the BigConnect Pipelines runtime directory before you upgrade. That way, BigConnect Pipelines can still use the libraries after the upgrade.\
\
To upgrade BigConnect Pipelines through Cloudera Manager, perform the following steps:

**Step 1. Stop All Pipelines**

**Step 2. Back-Up the Previous Version**

**Step 3. Install the BigConnect Pipelines Custom Service Descriptor**

**Step 4. Manually Install the Parcel and Checksum Files (Optional)**

**Step 5. Distribute and Activate the New BigConnect Pipelines Parcel**

**Step 6. Verify Modified Safety Valves**

**Step 7. Restart the BigConnect Pipelines Service**\


<mark style="color:red;">**Warning**</mark>**:** You must perform the steps in this order, or BigConnect Pipelines will fail to start.

### Step 1. Stop All Pipelines <a href="#ariaid-title2" id="ariaid-title2"></a>

Stop all pipelines running on the BigConnect Pipelines to be upgraded.

Use one of the following methods to stop all pipelines:

*   If the BigConnect Pipelines is not registered to work with BigConnect Pipelines Control Hub, stop the pipelines using the BigConnect Pipelines UI.\


    From the BigConnect Pipelines **Home** page, select all running pipelines in the list and then click the Stop icon.\

*   If the BigConnect Pipelines is registered to work with BigConnect Pipelines Control Hub, stop all jobs running on the BigConnect Pipelines using the Control Hub UI.\


    From the Control Hub **Jobs** page, filter the jobs by engine and by engine label. Select all active jobs in the list and then click the **Stop Jobs** icon.

### Step 2. Back-Up the Previous Version <a href="#ariaid-title3" id="ariaid-title3"></a>

Before you install the new version, create a backup of the files in the previous version by copying and renaming the configuration, data, and resource directories. That way, you can continue to run the previous version if needed.

Copy and rename the following directories on every Cloudera Manager node that runs BigConnect Pipelines:

* SDC\_DATA - The BigConnect Pipelines directory for pipeline state and configuration information.
* SDC\_RESOURCES - The BigConnect Pipelines directory for runtime resource files.

For example, if you are upgrading version 3.0.0.0, copy the BigConnect Pipelines configuration directory and rename it as follows: `/etc/sdc3000`.

If you need to roll back to the previous version, you must restore the previous directories on every Cloudera Manager node that runs BigConnect Pipelines.

For more information about these directories including the default values, see BigConnect Pipelines Directories.

### Step 3. Install the BigConnect Pipelines Custom Service Descriptor <a href="#ariaid-title4" id="ariaid-title4"></a>

Install the new BigConnect Pipelines custom service descriptor file (CSD), and then restart Cloudera Manager.

1.  Download the CSD from one of the following locations:

    * BigConnect Pipelines Support portal if you have an enterprise account.
    * BigConnect Pipelines archives page if you do not have an enterprise account.

    Or, you can use the GNU Wget program to download the CSD from the command line by running the following commands:

    ```
    export VERSION="3.22.0"
    wget https://archives.streamsets.com/datacollector/$VERSION/csd/STREAMSETS-$VERSION.jar
    ```
2.  Remove the previous BigConnect Pipelines CSD file from Cloudera Manager.\
    \
    For example:

    ```
    rm -f /opt/cloudera/csd/STREAMSETS*.jar
    ```
3. Copy the BigConnect Pipelines CSD file to the **Local Descriptor Repository Path**. \
   By default, the path is `/opt/cloudera/csd`.\
   \
   To verify the path to use, in Cloudera Manager, click **Administration** > **Settings**. In the navigation panel, select the **Custom Service Descriptors** category. Place the CSD file in the path configured for **Local Descriptor Repository Path**.\

4.  Set the file ownership to `cloudera-scm:cloudera-scm` with permission **644**.\
    \
    For example:

    ```
    chown cloudera-scm:cloudera-scm /opt/cloudera/csd/STREAMSETS*.jar
    chmod 644 /opt/cloudera/csd/STREAMSETS*.jar
    ```
5.  Use one of the following commands to restart Cloudera Manager Server:For Ubuntu 14.04, CentOS 6, Red Hat Enterprise Linux 6, or Oracle Linux 6:

    ```
    service cloudera-scm-server restart
    ```

    For Ubuntu 16.04, CentOS 7, Red Hat Enterprise Linux 7, or Oracle Linux 7:

    ```
    systemctl restart cloudera-scm-server
    ```
6. In Cloudera Manager, to restart the Cloudera Management Service, click **Home** > **Status**. To the right of Cloudera Management Service, click the **Menu** icon and select **Restart**.

### Step 4. Manually Install the Parcel and Checksum Files (Optional) <a href="#ariaid-title5" id="ariaid-title5"></a>

You can manually install the BigConnect Pipelines parcel and related checksum files. Manually install the files when the Cloudera Manager Server does not have internet access.

When working with multiple clusters, perform the following steps for each cluster.

1. Download the BigConnect Pipelines parcel and related checksum file for the Cloudera Manager Server operating system from the BigConnect Pipelines archives page.
2. Copy the BigConnect Pipelines parcel and checksum file to the **Cloudera Manager Local Parcel Repository Path**.\
   By default, the path is `/opt/cloudera/parcel-repo`.\
   To verify the path to use, click **Administration** > **Settings**. In the navigation panel, select the **Parcels** category. Place the BigConnect Pipelines parcel file in the path configured for **Local Parcel Repository Path.**
3.  Change ownership on the parcel and checksum file to the user that runs the Cloudera Manager process.For example, if the Cloudera Manager process runs as the cloudera-scm user, use the following command to change ownership to cloudera-scm:

    ```
    sudo chown cloudera-scm:cloudera-scm /opt/cloudera/parcel-repo/STREAMSETS_DATACOLLECTOR*
    ```

### Step 5. Distribute and Activate the New BigConnect Pipelines Parcel <a href="#ariaid-title6" id="ariaid-title6"></a>

After you add the BigConnect Pipelines repository to Cloudera Manager, you can download and distribute the new BigConnect Pipelines parcel across the cluster. Stop the BigConnect Pipelines service and deactivate the previous parcel before you activate the new parcel.

1.  To view the list of available parcels, in the menu bar, click the **Parcels** icon.

    The new BigConnect Pipelines parcel displays in the list of available parcels. If it doesn't display, click **Check for New Parcels**.
2.  To download the new BigConnect Pipelines parcel to the local repository, click **Download**.

    After the parcel is downloaded, the Download button becomes the Distribute button.
3. To distribute the new BigConnect Pipelines parcel to the cluster, click **Distribute**.
4. To stop the BigConnect Pipelines service, click **Clusters** > **BigConnect Pipelines** and then click **Actions** > **Stop**.
5. Click the **Parcels** icon to return to the Parcels page.
6. To deactivate the previous BigConnect Pipelines parcel, choose the appropriate cluster in the Location selector, and then click **Deactivate** for the parcel.
7. To activate the new BigConnect Pipelines parcel, choose the appropriate cluster in the Location selector, and then click **Activate** for the parcel.

### Step 6. Verify Modified Safety Valves <a href="#ariaid-title7" id="ariaid-title7"></a>

When you upgrade, Cloudera Manager updates the BigConnect Pipelines configuration properties for you. However, if you modified any of the **Advanced Configuration Snippet (Safety Valve) properties** in Cloudera Manager for the previous BigConnect Pipelines version, those values override any property settings in the new configuration files.

You must compare the new configuration files shipped with the parcel in `/opt/cloudera/parcels/STREAMSETS` with your modified safety valves and update the safety valves as needed to include any new properties.

For example, if you used the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc.properties to override the `system.stagelibs.blacklist` property, you must add any new stage libraries listed in the blacklist property in the new `sdc.properties` file to the overridden property in the safety valve.

### Step 7. Restart the BigConnect Pipelines Service <a href="#ariaid-title8" id="ariaid-title8"></a>

When you restart the BigConnect Pipelines service, Cloudera Manager updates the BigConnect Pipelines configuration properties for you. Cloudera Manager retains any customized values that you added in the previous BigConnect Pipelines version. It also adds any new properties included in the new BigConnect Pipelines version.

To restart the BigConnect Pipelines service, click **Clusters** > **BigConnect Pipelines** and then click **Actions** > **Start**.
