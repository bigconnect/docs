# Installing from Stage Properties

When configuring a pipeline, you can use the stage properties panel to install external libraries for the stage library that includes the stage.\
\
Important: For an RPM installation, you must configure BigConnect Pipelines to use an external directory before you can install external libraries from the stage properties panel.

1. While configuring a pipeline, select a stage that requires an external library in the pipeline canvas.
2.  In the stage properties panel, click the **External Libraries** tab:\


    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/ExternalLib\_StageProps.png)
3. Click the **Install External Libraries** icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_InstallExternalLibs.png).
4.  In the **Install External Libraries** dialog box, select the stage library that needs to access the external library.\


    For example, to install a JDBC driver for the JDBC Multitable Consumer origin, select the JDBC stage library. To install an external Java library for the Groovy Evaluator processor, select the Groovy stage library.
5. Browse to select the external library to install and click **Open**.
6.  To install the external library into the specified stage library, click **Upload**.\


    BigConnect Pipelines installs the external library. All stages included in the specified stage library can use this external library. For example, if you installed a JDBC driver for the JDBC stage library, then every stage included in the JDBC stage library can also access the driver.\


    To use the external library with other stage libraries, you must install the library into the additional stage libraries. For example, if you want to use the same JDBC driver with the MySQL Binary Log origin, you must also install the driver as an external library for the MySQL Binary Log stage library.
7. Restart BigConnect Pipelines in one of the following ways:
   * If you started BigConnect Pipelines manually from the command line, click **Restart BigConnect Pipelines** in the **Install External Libraries** dialog box.
   * If you started BigConnect Pipelines as a service, you must use the command line for restart. Click **Cancel** in the **Install External Libraries** dialog box, and then run the required command for your operating system:
     *   For CentOS 6, Oracle Linux 6, Red Hat Enterprise Linux 6, or Ubuntu 14.04 LTS, use:

         ```
         service sdc restart
         ```
     *   For CentOS 7, Oracle Linux 7, Red Hat Enterprise Linux 7, or Ubuntu 16.04 LTS, use:

         ```
         systemctl restart sdc
         ```
