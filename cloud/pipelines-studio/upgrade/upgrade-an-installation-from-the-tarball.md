# Upgrade an Installation from the Tarball

Users with an enterprise account can upgrade to a full, common, or core tarball installation. Other users can upgrade to the common tarball installation and install additional stage libraries as needed.

When you upgrade a tarball installation, you configure the new version to use a new configuration directory outside of the base BigConnect Pipelines runtime directory. You then configure the new version to use the same data, log, and resource directories as the previous version. As a result, the new version has access to the files created in the previous version.\
\
**Note**: If you installed additional drivers or developed custom stages, verify that those libraries are stored in a local directory external to the BigConnect Pipelines runtime directory before you upgrade. That way, BigConnect Pipelines can still use the libraries after the upgrade.

To upgrade a full, common, or core installation from the tarball, perform the following steps:

**Step 1. Shut Down the Previous Version**

**Step 2. Back Up the Previous Version**

**Step 3. Install the New Version**

**Step 4. Update Environment Variables**

**Step 5. Update the Configuration Files**

**Step 6. Install Additional Libraries for the Core Installation**

**Step 7. Start the New Version of BigConnect Pipelines**

### Step 1. Shut Down the Previous Version <a href="#ariaid-title2" id="ariaid-title2"></a>

Stop all running pipelines and then shut down the previous version of BigConnect Pipelines.

1. Use one of the following methods to stop all running pipelines:
   *   If the BigConnect Pipelines is not registered to work with BigConnect Pipelines Control Hub, stop the pipelines using the BigConnect Pipelines UI.

       From the BigConnect Pipelines **Home** page, select all running pipelines in the list and then click the **Stop** icon.
   *   If the BigConnect Pipelines is registered to work with BigConnect Pipelines Control Hub, stop all jobs running on the BigConnect Pipelines using the Control Hub UI.

       From the Control Hub **Jobs** page, filter the jobs by engine and by engine label. Select all active jobs in the list and then click the **Stop Jobs** icon.\

2. Use one of the following methods to shut down BigConnect Pipelines:
   *   To use the command line for shutdown when BigConnect Pipelines is started as a service, use the required command for your operating system.

       For CentOS 6, Oracle Linux 6, Red Hat Enterprise Linux 6, or Ubuntu 14.04 LTS, use: `service sdc stop`

       For CentOS 7, Oracle Linux 7, Red Hat Enterprise Linux 7, or Ubuntu 16.04 LTS, use: `systemctl stop sdc`
   * To use the BigConnect Pipelines UI, click **Administration > Shut Down**. When the confirmation dialog box appears, click **Yes**.

### Step 2. Back-Up the Previous Version <a href="#ariaid-title3" id="ariaid-title3"></a>

Before you install the new version, create a backup of the files in the previous version by copying and renaming the configuration, data, and resource directories. That way, you can continue to run the previous version if needed.

Copy and rename the following directories:

* SDC\_CONF - The BigConnect Pipelines configuration directory.
* SDC\_DATA - The BigConnect Pipelines directory for pipeline state and configuration information.
* SDC\_RESOURCES - The BigConnect Pipelines directory for runtime resource files.

For example, if you are upgrading version 3.0.0.0, copy the BigConnect Pipelines configuration directory and rename it as follows: `/etc/sdc3000`.

For more information about these directories including the default values, see BigConnect Pipelines Directories.

### Step 3. Install the New Version <a href="#ariaid-title4" id="ariaid-title4"></a>

The instructions that you use to install the new version depend on whether you start BigConnect Pipelines manually or as a service and on your operating system.

Users with an enterprise account can upgrade to a full, common, or core tarball installation. Other users can upgrade to the common tarball installation and install additional stage libraries as needed.

#### Installing from the Tarball (Manual Start) <a href="#ariaid-title5" id="ariaid-title5"></a>

Install the new version of the tarball.

1. Download the tarball from one of the following locations:
   * BigConnect Pipelines Support portal if you have a BigConnect Pipelines enterprise account.
   * BigConnect Pipelines website if you do not have an enterprise account.
2. Extract the tarball to a different directory than the previous version.
3.  Use the following command to set the `$SDC_DIST` environment variable to the location where you extracted the tarball:

    ```
    export SDC_DIST=<extraction directory>
    ```

    For example:

    ```
    export SDC_DIST=/sdc/streamsets-datacollector-3.22.0
    ```

#### Installing from the Tarball for Systems Using SysV (Service Start) <a href="#ariaid-title6" id="ariaid-title6"></a>

Operating systems that use the SysV init system include CentOS 6, Oracle Linux 6, Red Hat Enterprise Linux 6, or Ubuntu 14.04 LTS.

Install the new version of the tarball. Installing the full BigConnect Pipelines as a service requires root privileges.

1. Download the tarball from one of the following locations:
   * BigConnect Pipelines Support portal if you have a BigConnect Pipelines enterprise account.
   * BigConnect Pipelines website if you do not have an enterprise account.
2. Extract the tarball to a different directory than the previous version.
3. Create a backup of the `/etc/init.d/sdc` file that was used in the previous version.
4.  Use the following commands from the directory where you extracted the tarball to copy the initd/\_sdcinitd\_prototype file to the /etc/init.d directory and then change ownership of the file to `sdc`:

    ```
    cp initd/_sdcinitd_prototype  /etc/init.d/sdc
    chown sdc:sdc /etc/init.d/sdc
    ```
5. Edit the `/etc/init.d/sdc` file and set the `$SDC_DIST` and `$SDC_HOME` environment variables to the location where you extracted the tarball.
6.  Use the following command to make the sdc file executable:

    ```
    chmod 755 /etc/init.d/sdc
    ```

#### Installing from the Tarball for Systems Using Systemd (Service Start) <a href="#ariaid-title7" id="ariaid-title7"></a>

Operating systems that use the systemd init system include CentOS 7, Oracle Linux 7, Red Hat Enterprise Linux 7, or Ubuntu 16.04 LTS.

Install the new version of the tarball. Installing the full BigConnect Pipelines as a service requires root privileges.

1. Download the tarball from one of the following locations:
   * BigConnect Pipelines Support portal if you have a BigConnect Pipelines enterprise account.
   * BigConnect Pipelines website if you do not have an enterprise account.
2. Extract the tarball to a different directory than the previous version.
3.  Use the following command from the directory where you extracted the tarball to copy `systemd/sdc.service` to the `/etc/systemd/system` directory:

    ```
    cp systemd/sdc.service /etc/systemd/system/sdc.service
    ```
4.  Optionally, override the /etc/systemd/system/sdc.service file to modify the environment variables that define the directories and the system user and group.

    \
    For example, if you modified the default system user and group used by the previous version of BigConnect Pipelines, configure the new version to use the same system user and group.

    \
    Override the default values in the `sdc.service` file using the same procedure that you use to override unit configuration files on a systemd init system. For an example, see "Example 2. Overriding vendor settings" in this systemd.unit manpage.
5.  Use the following command from the directory where you extracted the tarball to copy `systemd/sdc.socket` to the `/etc/systemd/system` directory:

    ```
    cp systemd/sdc.socket /etc/systemd/system/sdc.socket
    ```
6. Edit the /etc/systemd/system/sdc.socket file to modify the BigConnect Pipelines port number to match the previous version. The port must match the one defined in `sdc.properties`. Default is `18630`.
7.  Use the following command to reload the systemd manager configuration:

    ```
    systemctl daemon-reload
    ```

### Step 4. Update Environment Variables <a href="#ariaid-title8" id="ariaid-title8"></a>

Update the BigConnect Pipelines environment variables so that the new version of BigConnect Pipelines uses a new configuration directory but the same working data, log, and resource directories as the previous version.

For example, your previous BigConnect Pipelines version used the directory `/var/lib/sdc` to store the data files for pipeline configuration and run details. When you upgrade, you configure the new version of BigConnect Pipelines to use the same working directory `/var/lib/sdc` for the data files. As a result, the new version has access to the pipelines created in the previous version.

Update the environment variables in the required file based on your installation type. For more information about the required file to edit, see Modifying Environment Variables.

1.  Update the directory environment variables to use the following values:

    | Environment Variable | Value                                                                                                                                                                                                                                 |
    | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | SDC\_CONF            | New location outside of the base BigConnect Pipelines runtime directory and unique from the previous renamed directory. For example, if you renamed the previous configuration directory to `/etc/sdc3000`, use the value `/etc/sdc`. |
    | SDC\_DATA            | Same directory that the previous version used.                                                                                                                                                                                        |
    | SDC\_LOG             | Same directory that the previous version used.                                                                                                                                                                                        |
    | SDC\_RESOURCES       | Same directory that the previous version used.                                                                                                                                                                                        |
2. If you installed external libraries or developed custom stages, add the STREAMSETS\_LIBRARIES\_EXTRA\_DIR or USER\_LIBRARIES\_DIR environment variable to the required file, and set it to the same directory used in the previous version.
3. Manually update the required file with any other customized environment variable values that you defined in the previous version.
4.  Use the following command to create the BigConnect Pipelines configuration directory at `/etc/sdc`:

    ```
    mkdir /etc/sdc
    ```
5.  Use the following command from the directory where you extracted the tarball to copy all files from `etc` into the BigConnect Pipelines configuration directory that you just created:

    ```
    cp -R etc/* /etc/sdc
    ```
6. To run BigConnect Pipelines as a service, change the owner of the `/etc/sdc` directory and all files in the directory to the system user and group that starts BigConnect Pipelines.\
   \
   By default, BigConnect Pipelines uses a system user and group named `sdc`.\

7.  Use the following command to set owner only permission on the `form-realm.properties` file in the `/etc/sdc` directory:

    ```
    chmod go-rwx /etc/sdc/form-realm.properties
    ```

### Step 5. Update the Configuration Files <a href="#ariaid-title9" id="ariaid-title9"></a>

A new BigConnect Pipelines version can include new properties and configuration files required for BigConnect Pipelines to start or function properly. In the previous step, you updated the environment configuration file so that the new version of BigConnect Pipelines uses the new configuration files stored in the `$SDC_CONF` directory. In this step, you’ll compare the previous and new versions of the configuration files, and update the new files as needed with the same customized property values.

For example when upgrading from version 3.0.0.0, you'd compare the files in your back up directory, `/etc/sdc3000`, with the files in the `/etc/sdc` directory. Then update the new files in the `/etc/sdc` directory with any customizations made in the previous files in the `/etc/sdc3000` directory.

1. Compare the previous and new versions of the `sdc.properties` file, and update the new file as needed with the same customized property values.
2. If you registered the previous BigConnect Pipelines to work with BigConnect Pipelines Control Hub, complete the following steps to update the configuration files used by Control Hub:
   1. Compare the previous and new version of the `dpm.properties` file, and update the new file as needed with the same customized property values.
   2. Replace the new version of the `application-token.txt` file with the previous version of the file.The previous version of the file includes the authentication token that this BigConnect Pipelines instance requires to issue authenticated requests to Control Hub. As a result, you'll need BigConnect Pipelines to use the previous version of the file.
3. Compare the previous and new versions of the remaining files, and update the new files as needed with the same customized property values:
   * The appropriate realm.properties file, based on the authentication type that you use.
   * credential stores properties file
   * `email-password.txt`
   * keystore files
   * LDAP files
   * log4j properties file
   * security policy file
   *   Vault properties file

       As of version 2.7.0.0, most of the Vault configuration properties have been moved to the new credential stores properties file. The properties use the same name, with an added "credentialStore.vault.config" prefix. If you are upgrading from a version earlier than 2.7.0.0, copy any values that you customized in the previous Vault properties file into the same property names in the credential stores properties file.

### Step 6. Install Additional Libraries for the Core Installation <a href="#ariaid-title10" id="ariaid-title10"></a>

If you upgraded a common or core installation of BigConnect Pipelines, install the individual stage libraries that the upgraded pipelines require.

### Step 7. Start the New Version of BigConnect Pipelines <a href="#ariaid-title11" id="ariaid-title11"></a>

Start the new version of BigConnect Pipelines.

**To start BigConnect Pipelines manually**\
\
Use the following command from the `$SDC_DIST` directory to run BigConnect Pipelines as the system user account logged into the command prompt:

```
bin/streamsets dc
```

Or, use the following command to run BigConnect Pipelines in the background:

```
nohup bin/streamsets dc &
```

Use the following command to run BigConnect Pipelines as another system user account:

```
sudo -u <user> bin/streamsets dc
```

**To start BigConnect Pipelines as a service**\
\
Use the required command for your operating system:

*   For CentOS 6, Oracle Linux 6, Red Hat Enterprise Linux 6, or Ubuntu 14.04 LTS, use:

    ```
    service sdc start
    ```
*   For CentOS 7, Oracle Linux 7, Red Hat Enterprise Linux 7, or Ubuntu 16.04 LTS, use:

    ```
    systemctl start sdc
    ```
