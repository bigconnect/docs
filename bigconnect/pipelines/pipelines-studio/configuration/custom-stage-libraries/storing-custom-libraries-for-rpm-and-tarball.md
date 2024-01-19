# Storing Custom Libraries for RPM and Tarball

To store custom stage libraries for an RPM or tarball installation, perform the following steps:

1.  Create a local directory external to the BigConnect Pipelines installation directory for the custom stage libraries. Use an external directory to enable use of the libraries after BigConnect Pipelines upgrades.\
    \
    For example, if you installed BigConnect Pipelines in the following directory:

    ```
    /opt/sdc/
    ```

    you might create the custom stage library directory at:

    ```
    /opt/sdc-user-libs
    ```
2.  Copy the custom stage libraries to the directory. Use a directory structure for each custom stage.For example to store libraries for a custom stage named customstage1, you would copy them to the following directory:

    ```
    /opt/sdc-user-libs/customstage1
    ```
3.  Add the USER\_LIBRARIES\_DIR environment variable to the appropriate file and point it to the custom stage library directory.

    Modify environment variables using the method required by your installation type.

    Set the environment variable as follows:

    ```
    export USER_LIBRARIES_DIR="<custom stage library directory>"
    ```

    For example:

    ```
    export USER_LIBRARIES_DIR="/opt/sdc-user-libs/"
    ```
4. When using the Java Security Manager, which is enabled by default, update the BigConnect Pipelines security policy to include the custom stage library directory as follows:
   1.  In the BigConnect Pipelines configuration directory, open the security policy file:

       ```
       $SDC_CONF/sdc-security.policy
       ```
   2.  Add the following lines to the file:

       ```
       // custom stage library directory
       grant codebase "file://<custom stage library directory>/-" {
          permission java.security.AllPermission;
       };
       ```

       Note: You must include a trailing /- after the directory name.For example:

       ```
       // custom stage library directory
       grant codebase "file:///opt/sdc-user-libs/-" {
          permission java.security.AllPermission;
       };
       ```
5. Restart BigConnect Pipelines.\
   \
   The BigConnect Pipelines stage library displays the custom stages.
