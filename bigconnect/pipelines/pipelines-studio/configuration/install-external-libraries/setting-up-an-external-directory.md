# Setting Up an External Directory

By default, external libraries are installed to the `$SDC_DIST/streamsets-libs-extras` directory.

For an RPM installation, you must configure BigConnect Pipelines to use an external directory before you can install external libraries from Package Manager or from the stage properties panel.

For a tarball or Cloudera Manager installation, you can use the default directory as you get started with BigConnect Pipelines. However, BigConnect Pipelines recommends configuring BigConnect Pipelines to use an external directory to enable use of the libraries after BigConnect Pipelines upgrades.

Use the required procedure for your installation type.

#### Setting Up for RPM and Tarball

Before you install external libraries for an RPM or tarball installation, set up an external directory to store the libraries.

1.  Create a local directory external to the BigConnect Pipelines installation directory.\
    \
    For example, if you installed BigConnect Pipelines in the following directory:

    ```
    /opt/sdc/
    ```

    you might create the external directory at:

    ```
    /opt/sdc-extras
    ```
2.  Grant the user who starts BigConnect Pipelines ownership on the external directory.For example, if you use the default system user and group named `sdc` to run BigConnect Pipelines as a service, use the following command to change the owner of the external directory and all files in the directory to `sdc:sdc`:

    ```
    chown -R sdc:sdc /opt/sdc-extras
    ```
3.  Add the STREAMSETS\_LIBRARIES\_EXTRA\_DIR environment variable to the appropriate file and point it to the external directory.

    Modify environment variables using the method required by your installation type.

    Set the environment variable as follows:

    ```
    export STREAMSETS_LIBRARIES_EXTRA_DIR="<external directory>"
    ```

    For example:

    ```
    export STREAMSETS_LIBRARIES_EXTRA_DIR="/opt/sdc-extras/"
    ```
4. When using the Java Security Manager, which is enabled by default, update the BigConnect Pipelines security policy to include the external directory as follows:
   1. In the BigConnect Pipelines configuration directory, open the security policy file, `$SDC_CONF/sdc-security.policy`.
   2.  Add the following lines to the file:

       ```
       // user-defined external directory
       grant codebase "file://<external directory>-" {
         permission java.security.AllPermission;
       };
       ```

       For example:

       ```
       // user-defined external directory
       grant codebase "file:///opt/sdc-extras/-" {
         permission java.security.AllPermission;
       };
       ```
5. Restart BigConnect Pipelines.

#### Setting Up for Cloudera Manager

Before you install external libraries for a Cloudera Manager installation, set up an external directory to store the libraries.

1. In Cloudera Manager, select the **BigConnect Pipelines** service and then click **Configuration**.
2.  On the **Configuration** page, in the **BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-env.sh** field, add the STREAMSETS\_LIBRARIES\_EXTRA\_DIR environment variable and point it to the external directory, as follows:

    ```
    export STREAMSETS_LIBRARIES_EXTRA_DIR="<external directory>"
    ```

    For example:

    ```
    export STREAMSETS_LIBRARIES_EXTRA_DIR="/opt/sdc-extras/"
    ```

    By default, the path is `/var/lib/sdc`.
3. Create the `/opt/sdc-extras/` directory on every node that runs **BigConnect Pipelines**.
4.  Grant the user who starts **BigConnect Pipelines** ownership on the external directory added to every node.\
    \
    For example, if you use the default system user and group named `sdc` to run **BigConnect Pipelines** as a service, use the following command to change the owner of the external directory and all files in the directory to `sdc:sdc`:

    ```
    chown -R sdc:sdc /opt/sdc-extras
    ```
5.  When using the Java Security Manager, which is enabled by default, update the **BigConnect Pipelines** **Advanced Configuration Snippet (Safety Valve) for sdc-security.policy** property to include the external directory as follows:

    ```
    // user-defined external directory
    grant codebase "file://<external directory>-" {
      permission java.security.AllPermission;
    };
    ```

    For example:

    ```
    // user-defined external directory
    grant codebase "file:///opt/sdc-extras/-" {
      permission java.security.AllPermission;
    };
    ```
6. Restart BigConnect Pipelines.
