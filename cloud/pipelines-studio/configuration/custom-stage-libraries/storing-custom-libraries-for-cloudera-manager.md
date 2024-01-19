# Storing Custom Libraries for Cloudera Manager

To store custom stage libraries for a Cloudera Manager installation, perform the following steps:

1.  On every node that runs BigConnect Pipelines, create a local directory for the custom stage libraries.\
    For example, you might create the custom stage library directory at:

    ```
    /opt/sdc-user-libs
    ```
2.  On every node that runs BigConnect Pipelines, copy the custom stage libraries to the directory defined for the USER\_LIBRARIES\_DIR environment variable. Use a directory structure for each custom stage.For example to store libraries for a custom stage named customstage1, you would copy them to the following directory:

    ```
    /opt/sdc-user-libs/customstage1
    ```
3. In Cloudera Manager, select the **BigConnect Pipelines** service and then click **Configuration**.
4.  On the **Configuration** page, in the **BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-env.sh** field, add the USER\_LIBRARIES\_DIR environment variable and point it to the custom stage library directory, as follows:

    ```
    export USER_LIBRARIES_DIR="<custom stage library directory>"
    ```

    For example:

    ```
    export USER_LIBRARIES_DIR="/opt/sdc-user-libs/"
    ```
5.  When using the Java Security Manager, which is enabled by default, update the **BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-security.policy** property to include the custom stage library directory as follows:

    ```
    // custom stage library directory
    grant codebase "file://<custom stage library directory>/-" {
       permission java.security.AllPermission;
    };
    ```

    For example:

    ```
    // custom stage library directory
    grant codebase "file:///opt/sdc-user-libs/-" {
       permission java.security.AllPermission;
    };
    ```
6. Restart BigConnect Pipelines.
