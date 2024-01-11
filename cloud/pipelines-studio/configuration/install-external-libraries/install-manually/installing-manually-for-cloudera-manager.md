# Installing Manually for Cloudera Manager

To manually install external libraries for an installation with Cloudera Manager, perform the following steps:

1.  On every node that runs BigConnect Pipelines, create subdirectories in the directory where BigConnect Pipelines installs external libraries.\


    Create a subdirectory for each set of external libraries based on the stage library name. For example, if you [set up an external directory](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Configuration/ExternalLibs.html#concept\_hhl\_nxz\_bz) to store the libraries at `/opt/sdc-extras`, then create the subdirectories as follows on every node:

    ```
    /opt/sdc-extras/<stage library name>/lib/
    ```

    To install drivers for JDBC, create the following subdirectory on every node:

    ```
    /opt/sdc-extras/streamsets-datacollector-jdbc-lib/lib/
    ```

    To also install drivers for JMS, create the following subdirectory on every node:

    ```
    /opt/sdc-extras/streamsets-datacollector-jms-lib/lib/
    ```

    \
    **Note**: If you use multiple stage libraries for a particular stage, and you want to use an external library with all stage libraries, you must install the external library for each stage library.

    For example, say you want to use an external library with the Spark Evaluator processor, but you use two versions of the processor - each from a different stage library. To make the external library available to both processor versions, you must upload the external library to both stage libraries.\


    **Tip**: For a list of stage library names, see Available Stage Libraries.
2. Copy the external libraries to the appropriate subdirectories on every node.
3. Restart BigConnect Pipelines.
