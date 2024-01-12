# Upgrade an Installation from the RPM Package

When you upgrade an installation from the RPM package, the new version uses the default configuration, data, log, and resource directories. If the previous version used the default directories, the new version has access to the files created in the previous version.

If the previous version used customized values for the directory environment variables, you must make the same customizations in the new version so that the new version can access the same files.

**Note**: If you installed external libraries or developed custom stages, verify that those libraries are stored in a local directory external to the BigConnect Pipelines installation directory before you upgrade. That way, BigConnect Pipelines can still use the libraries after the upgrade.

To upgrade an installation from the RPM package, perform the following steps:

**Step 1. Shut Down the Previous Version**

**Step 2. Back-Up the Previous Version**

**Step 3. Install the New Version**

**Step 4. Update Environment Variables**

**Step 5. Update the Configuration Files**

**Step 6. Install Additional Libraries for the Core Installation**

**Step 7. Uninstall Previous Libraries**

**Step 8. Start the New Version of BigConnect Pipelines**

### Step 1. Shut Down the Previous Version <a href="#ariaid-title2" id="ariaid-title2"></a>

Stop all pipelines and then shut down the previous version of BigConnect Pipelines.

1. Use one of the following methods to stop all running pipelines:
   *   If the BigConnect Pipelines is not registered to work with BigConnect Pipelines Control Hub, stop the pipelines using the BigConnect Pipelines UI.

       From the BigConnect Pipelines **Home** page, select all running pipelines in the list and then click the **Stop** icon.
   * If the BigConnect Pipelines is registered to work with BigConnect Pipelines Control Hub, stop all jobs running on the BigConnect Pipelines using the Control Hub UI.\
     \
     From the Control Hub **Jobs** page, filter the jobs by engine and by engine label. Select all active jobs in the list and then click the **Stop Jobs** icon.\

2. Use one of the following methods to shut down BigConnect Pipelines:
   *   To use the command line for shutdown, use the required command for your operating system.

       For CentOS 6, Oracle Linux 6, or Red Hat Enterprise Linux 6, use: `service sdc stop`

       For CentOS 7, Oracle Linux 7, or Red Hat Enterprise Linux 7, use: `systemctl stop sdc`
   * To use the BigConnect Pipelines UI, click **Administration** > **Shut Down**. When the confirmation dialog box appears, click **Yes**.

### Step 2. Back-Up the Previous Version <a href="#ariaid-title3" id="ariaid-title3"></a>

Before you install the new version, create a backup of the files in the data and resource directories in the previous version. You’ll also need to create a backup of the environment configuration file so that the file is not overwritten when you install the new version. That way, you can continue to run the previous version if needed.

Back up the following directories and files:

* Data directory defined in the SDC\_DATA environment variable. Default is `/var/lib/sdc`.
* Resource directory defined in the SDC\_RESOURCES environment variable. Default is `/var/lib/sdc-resources`.
* File that defines environment variables, based on the operating system:
  * CentOS 6, Oracle Linux 6, or Red Hat Enterprise Linux 6 - the `$SDC_DIST/libexec/sdcd-env.sh` file.
  * CentOS 7, Oracle Linux 7, or Red Hat Enterprise Linux 7 - the `/usr/lib/systemd/system/sdc.service` file.

For example, if you are upgrading version 3.0.0.0 on CentOS 6, Oracle Linux 6, or Red Hat Enterprise Linux 6, back up the BigConnect Pipelines data directory and name it as follows: `/var/lib/sdc3000`. Create a backup of the environment configuration file and name the backup file as follows: `sdcd-env-3000.sh`.

For more information about these directories including the default values, see BigConnect Pipelines Directories.

### Step 3. Install the New Version <a href="#ariaid-title4" id="ariaid-title4"></a>

Install the new version of the RPM package. Installing the full BigConnect Pipelines as a service requires root privileges.

1. Access the BigConnect Pipelines RPM package from one of the following locations:
   * BigConnect Pipelines Support portal if you have an enterprise account.
   * BigConnect Pipelines archives page if you do not have an enterprise account.
2. Download the RPM package for your operating system:
   * For CentOS 6, Oracle Linux 6, or Red Hat Enterprise Linux 6, download the RPM EL6 package.
   * For CentOS 7, Oracle Linux 7, or Red Hat Enterprise Linux 7, download the RPM EL7 package.
3.  Use the following command to extract the file to a different directory than the previous version:

    ```
    tar xf streamsets-datacollector-<version>-<operating_system>-all-rpms.tar
    ```

    For example, to extract version 3.22.0 on CentOS 7, use the following command:

    ```
    tar xf streamsets-datacollector-3.22.0-el7-all-rpms.tar
    ```
4.  To install the full RPM package and all available stage libraries, use the following command:

    ```
    yum localinstall streamsets*
    ```
5.  Or, to install the core RPM package and then install individual stage libraries as needed, use the following command:

    ```
    yum localinstall streamsets-datacollector-<version>-1.noarch.rpm
    ```

    For example, to install version 3.22.0, use the following command:

    ```
    yum localinstall streamsets-datacollector-3.22.0-1.noarch.rpm
    ```

### Step 4. Update Environment Variables <a href="#ariaid-title5" id="ariaid-title5"></a>

Each RPM installation uses the same default values as the previous version for all of the environment variables. If the previous version used the default values, the new version is configured to use the same environment variables.

If the previous version used customized values for the environment variables, you must make the same customizations in the new version. The new version must use the same data, log, and resource directories as the previous version.

1. Open the environment configuration file that you backed up in the previous version.\
   For example, on CentOS 6, Oracle Linux 6, or Red Hat Enterprise Linux 6, open the `$SDC_DIST/libexec/sdcd-env-3000.sh` file.
2. In the new version of BigConnect Pipelines, open the environment configuration file.\
   For example, on CentOS 6, Oracle Linux 6, or Red Hat Enterprise Linux 6, open the `$SDC_DIST/libexec/sdcd-env.sh` file.
3. Compare the previous and new versions of the environment configuration file, and update the new file as needed with the same customized environment variables.

### Step 5. Update the Configuration Files <a href="#ariaid-title6" id="ariaid-title6"></a>

A new BigConnect Pipelines version can include new properties and configuration files required for BigConnect Pipelines to start or function properly.

When you install the new RPM package, the configuration files are written to the same default directory as the previous version, `/etc/sdc`. The new versions of the configuration files are renamed with the following extension: `.rpmnew`. For example, the new version of the BigConnect Pipelines configuration file is renamed to `sdc.properties.rpmnew`.

To update the configuration files, you must rename the previous and new versions of the files and then update the new files with any customized property values defined in the previous version.

**Note**: If the previous version used a customized value for `$SDC_CONF`, the new configuration files are written to a different directory than the previous version, and so do not require the `.rpmnew` file extension. In this case, you do not rename the configuration files, but must update the new files with any customized values defined in the previous version.

1. In the working `$SDC_CONF` directory, `/etc/sdc` by default, rename all previous configuration files except for the `application-token.txt` file with the following extension: `.old`.The previous version of the `application-token.txt` file includes the authentication token that this BigConnect Pipelines instance requires to issue authenticated requests to Control Hub. As a result, you'll need BigConnect Pipelines to use the previous version of the file.
2. Remove the following extension from all new configuration files except for the `application-token.txt` file: `.rpmnew`.
3. Compare the previous and new versions of the `sdc.properties` file, and update the new file as needed with the same customized property values.
4. Compare the previous and new versions of the remaining files, and update the new files as needed with the same customized property values:
   * The appropriate realm.properties file, based on the authentication type that you use.
   * credential stores properties file
   * `email-password.txt`
   * keystore files
   * LDAP files
   * log4j properties file
   * security policy file
   *   Vault properties file

       \
       As of version 2.7.0.0, most of the Vault configuration properties have been moved to the new credential stores properties file. The properties use the same name, with an added "credentialStore.vault.config" prefix. If you are upgrading from a version earlier than 2.7.0.0, copy any values that you customized in the previous Vault properties file into the same property names in the credential stores properties file.

### Step 6. Install Additional Libraries for the Core Installation <a href="#ariaid-title7" id="ariaid-title7"></a>

If you installed the core RPM package, install the individual stage libraries that the upgraded pipelines require.

For instructions on installing additional stage libraries, see Installing for RPM.

### Step 7. Uninstall Previous Libraries <a href="#ariaid-title8" id="ariaid-title8"></a>

Uninstall all stage libraries used by the previous BigConnect Pipelines version.

1.  Run the following command to list all stage libraries used by the previous BigConnect Pipelines version:

    ```
    rpm -qa | grep streamsets | grep "<version>"
    ```

    For example, to list all stage libraries used by BigConnect Pipelines version 3.0.0.0, run the following command:

    ```
    rpm -qa | grep streamsets | grep "3.0.0.0"
    ```
2.  Run the following command to uninstall all stage libraries used by the previous version:

    ```
    yum remove <library package name> <library package name> ...
    ```

    Where `library package name` is the full name of the libraries that you want to uninstall. Separate each name with commas. Do not include spaces in the command.

### Step 8. Start the New Version of BigConnect Pipelines <a href="#ariaid-title9" id="ariaid-title9"></a>

Use the required command for your operating system to start the new version of BigConnect Pipelines:

*   For CentOS 6, Oracle Linux 6, or Red Hat Enterprise Linux 6, use:

    ```
    service sdc start
    ```
*   For CentOS 7, Oracle Linux 7, or Red Hat Enterprise Linux 7, use:

    ```
    systemctl start sdc
    ```
