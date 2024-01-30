# Working with Data Governance Tools

## Working with Data Governance Tools <a href="#ariaid-title1" id="ariaid-title1"></a>

You can configure BigConnect Pipelines to integrate with data governance tools, giving you visibility into data movement - where the data came from, where it’s going to, and who is interacting with it.

Data governance tool integration is available with a paid subscription. For details, contact BigConnect Pipelines.

BigConnect Pipelines can work with the following data governance tools:\
\
**Apache Atlas**\
\
If you have Apache Atlas installed, you can configure BigConnect Pipelines to publish metadata about running pipelines to Apache Atlas. When publishing metadata, BigConnect Pipelines uses Apache Kafka to send the metadata to Apache Atlas. The data is sent in JSON format.\
\
**Cloudera Navigator**\
\
If you use Cloudera Manager and have Cloudera Navigator installed, you can configure BigConnect Pipelines to publish metadata about running pipelines to Cloudera Navigator. When publishing metadata, BigConnect Pipelines makes an HTTPS request to Cloudera Navigator using basic authentication. The data is sent in JSON format.\
\
**Collibra**\
\
If you use Collibra Platform, you can configure BigConnect Pipelines to publish metadata about running pipelines to Collibra. When publishing metadata, BigConnect Pipelines passes data to Collibra using the Collibra import API. The data is sent in JSON format.



You then use one of the data governance tools to explore the pipeline metadata, including viewing lineage diagrams of the metadata.

BigConnect Pipelines uses separate threads to publish metadata to data governance tools - so enabling metadata publishing has minimal effect on the running pipeline threads. Every pipeline publishes metadata when the pipeline starts and when it stops. Some origins and destinations publish metadata only once, typically when they initialize. However, some stages publish metadata each time they create a new object - for example, when the Hadoop FS or Local FS destination creates a new output file.

### Supported Stages <a href="#ariaid-title2" id="ariaid-title2"></a>

At this time, only some pipeline stages support publishing metadata to data governance tools.

BigConnect Pipelinespublishes metadata for all supported stages in all running pipelines. If you have multiple BigConnect Pipelines that run pipelines, configure each BigConnect Pipelines to publish metadata to the same governance tool instance.

BigConnect Pipelines can publish metadata for the following stages:

* Amazon S3 origin
* Dev Data Generator origin
* Directory origin
* Hadoop FS origin
* JDBC Query Consumer origin
* Kafka Consumer origin
* Kafka Multitopic Consumer origin
* SFTP/FTP/FTPS Client origin
* Hadoop FS destination
* Local FS destination
* HBase destination
* Hive Streaming destination
* Kafka Producer destination
* Kudu destination

**Note**: Remember that you don't enable metadata publishing at the stage level or even at the pipeline level. You simply configure BigConnect Pipelines to publish metadata for all running pipelines.

When a pipeline includes an unsupported stage, the data governance tool does not display that stage as an input or output in the lineage diagram. For example, if a running pipeline includes an unsupported origin and the Hadoop FS destination, then the data governance tool displays the pipeline as having 0 inputs and multiple outputs - one output for each generated output file. If a running pipeline includes no supported origins or destinations, the data governance tool displays the pipeline as having 0 inputs and 0 outputs.

### Apache Atlas <a href="#ariaid-title3" id="ariaid-title3"></a>

You can configure BigConnect Pipelines to publish metadata about running pipelines to Apache Atlas. You then use Apache Atlas to explore the pipeline metadata, including viewing lineage diagrams of the metadata.

Data governance tool integration is available with a paid subscription. For details, contact BigConnect Pipelines.

To integrate BigConnect Pipelines with Apache Atlas, download and install one of the following Apache Atlas stage libraries as a custom stage library:

| Stage Library                                      | Description                                                     |
| -------------------------------------------------- | --------------------------------------------------------------- |
| streamsets-datacollector-apache-atlas\_0\_8\_1-lib | Use when working with HDP version 2.6.x and Apache Atlas 0.8.1. |
| streamsets-datacollector-apache-atlas\_1\_1\_0-lib | Use when working with HDP version 3.1.x and Apache Atlas 1.1.0. |

You then configure BigConnect Pipelines to connect to Apache Atlas. You also must deploy the BigConnect Pipelines data model to the machine where the Apache Atlas server is installed.

#### Prerequisites <a href="#ariaid-title4" id="ariaid-title4"></a>

Before you enable BigConnect Pipelines to publish pipeline metadata to Apache Atlas, you must complete the following prerequisites:

**Verify that BigConnect Pipelines has a required Hortonworks Data Platform (HDP) stage library installed**\
\
To connect to and publish metadata to Apache Atlas, BigConnect Pipelines requires HDP versions 2.6.x or 3.1.x.\
\
To verify that BigConnect Pipelines has a valid HDP stage library installed, click the Package Manager icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PackageManager.png)) to display the list of installed stage libraries. If a required library version is not installed, install the library before configuring BigConnect Pipelines to publish pipeline metadata to Apache Atlas.\
\
**Configure the required settings in Apache Atlas**\
\
To connect to and publish metadata to Apache Atlas, you must configure the following required settings in Apache Atlas:

* Configure Apache Atlas to use HBase as a persistence engine.
* Configure Apache Atlas to use Apache Kafka for notifications.
* Ensure that the graphing engine is enabled for Apache Atlas.

For more information on configuring Apache Atlas, see the Apache Atlas documentation.

#### Step 1. Download and Install the Stage Library <a href="#ariaid-title5" id="ariaid-title5"></a>

Download and install the appropriate Apache Atlas stage library as a custom stage library.

1. Using the link provided in the email that you received from BigConnect Pipelines, download the tarball containing the Apache Atlas stage library.
2.  Extract the tarball.The tarball is extracted into the following directory:

    ```
    <stage library name>/lib/
    ```

    For example, the Apache Atlas version 0.8.1 stage library is extracted to:

    ```
    streamsets-datacollector-apache-atlas_0_8_1-lib/lib/
    ```

    The Apache Atlas version 1.1.0 stage library is extracted to:

    ```
    streamsets-datacollector-apache-atlas_1_1_0-lib/lib/
    ```
3.  Create a local directory external to the BigConnect Pipelines installation directory for the custom stage libraries. Use an external directory to enable use of the libraries after BigConnect Pipelines upgrades.For example, if you installed BigConnect Pipelines in the following directory:

    ```
    /opt/sdc/
    ```

    you might create the custom stage library directory at:

    ```
    /opt/sdc-user-libs
    ```
4.  Copy the Apache Atlas stage library to this custom stage library directory, as follows:

    ```
    /opt/sdc-user-libs/<stage library name>/lib/
    ```

    For example, for Apache Atlas version 0.8.1:

    ```
    /opt/sdc-user-libs/streamsets-datacollector-apache-atlas_0_8_1-lib/lib/
    ```

    For Apache Atlas version 1.1.0:

    ```
    /opt/sdc-user-libs/streamsets-datacollector-apache-atlas_1_1_0-lib/lib/
    ```
5.  Add the USER\_LIBRARIES\_DIR environment variable to the appropriate file and point it to the custom stage library directory.

    Modify environment variables using the method required by your installation type.

    Set the environment variable as follows:

    ```
    export USER_LIBRARIES_DIR="<custom stage library directory>"
    ```

    For example:

    ```
    export USER_LIBRARIES_DIR="/opt/sdc-user-libs/"
    ```
6. When using the Java Security Manager, which is enabled by default, update the BigConnect Pipelines security policy to include the custom stage library directory as follows:
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
7. Restart BigConnect Pipelines.

#### Step 2. Configure the Connection to Apache Atlas <a href="#ariaid-title6" id="ariaid-title6"></a>

Configuring BigConnect Pipelines to connect to Apache Atlas involves working with the following files:\
\
**BigConnect Pipelines configuration file**\
\
Add lineage publisher properties to the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`.\
\
**Apache Atlas application properties file**\
\
If BigConnect Pipelines is installed on a separate machine from the Apache Atlas server, copy the atlas-application.properties file to the BigConnect Pipelines machine. Modify the Apache Atlas application properties file as needed, and then specify the location of the file in the SDC\_JAVA\_OPTS environment variable on the BigConnect Pipelines machine.\
\
<mark style="color:red;">**Warning**</mark>: If metadata publishing is enabled and Apache Atlas cannot access the atlas-application.properties file at BigConnect Pipelines start up, then BigConnect Pipelines fails to start.

1.  To enable metadata publishing to Apache Atlas, modify the BigConnect Pipelines configuration file, $SDC\_CONF/sdc.properties.

    For Apache Atlas version 0.8.1, add the following lines to the file:

    ```
    # Lineage Publisher
    #
    lineage.publishers=atlas
    lineage.publisher.atlas.def=streamsets-datacollector-apache-atlas_0_8_1-lib::com_streamsets_pipeline_stage_plugin_atlas_AtlasLineagePublisher
    ```

    For Apache Atlas version 1.1.0, add the following lines to the file:

    ```
    # Lineage Publisher
    #
    lineage.publishers=atlas
    lineage.publisher.atlas.def=streamsets-datacollector-apache-atlas_1_1_0-lib::com_streamsets_pipeline_stage_plugin_atlas_AtlasLineagePublisher1_1_0
    ```
2.  If BigConnect Pipelines is installed on a separate machine from the Apache Atlas server, copy the atlas-application.properties file to the BigConnect Pipelines machine.

    StreamSets recommends storing the atlas-application.properties file in the `$SDC_CONF directory`. \
    \
    For example, for an RPM installation of BigConnect Pipelines, store the file in the /etc/sdc/ directory.
3.  Verify that the atlas-application.properties file is correctly configured.For example, if you copied the atlas-application.properties file to the BigConnect Pipelines machine, you might need to modify the following properties which specify the Kafka installation on the Apache Atlas server:

    ```
    atlas.kafka.zookeeper.connect=localhost:9026
    atlas.kafka.bootstrap.servers=localhost:9027
    ```

    For more information on the properties in this file, see the Apache Atlas documentation.
4.  On the BigConnect Pipelines machine, define the -Datlas.conf system property in the SDC\_JAVA\_OPTS environment variable.

    \
    Modify environment variables using the method required by your installation type.

    \
    The -Datlas.conf system property defines the location of the local atlas-application.properties file. Define the Atlas system property as follows:

    ```
    export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Datlas.conf=<configuration directory> -Xmx1024m  -Xms1024m -server"
    ```

    The defined configuration directory must end in a forward slash (/). For example, if you stored the file in the $SDC\_CONF directory for an RPM installation of BigConnect Pipelines, define the Atlas system property as follows:

    ```
    export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Datlas.conf=/etc/sdc/ -Xmx1024m  -Xms1024m -server"
    ```
5. Restart BigConnect Pipelines for the changed properties to take effect.

#### Step 3. Deploy the StreamSets Data Model to Apache Atlas <a href="#ariaid-title7" id="ariaid-title7"></a>

Download the StreamSets data model and deploy the data model to the Apache Atlas server.

1.  Using the link provided in the email that you received from StreamSets, download the StreamSets data model for Apache Atlas:

    ```
    0090-streamsets_model.json
    ```
2.  Deploy the data model to the machine where the Apache Atlas server is installed.On the Apache Atlas server, copy the StreamSets data model to the same directory that contains the Sqoop data model.For example, if using Hortonworks version 2.6.1 for testing, copy the StreamSets data model to the following directory:

    ```
    /usr/hdp/2.6.1.0-129/atlas/models/0090-streamsets_model.json
    ```
3. Restart the Apache Atlas server so that the server can recognize the newly deployed data model.

#### Viewing Published Metadata <a href="#ariaid-title8" id="ariaid-title8"></a>

BigConnect Pipelines publishes pipeline metadata to Apache Atlas when a pipeline stops. Apache Atlas lists each pipeline by pipeline title and displays supported origins as inputs and supported destinations as outputs. Apache Atlas also includes other metadata, such as the pipeline description, version, labels, and the user who started the pipeline.

For example, let's assume that you run a pipeline that includes a JDBC Query Consumer origin and a Local FS destination. Apache Atlas displays a single input representing the JDBC Query Consumer origin. As the pipeline runs, the Local FS destination creates multiple output files. Apache Atlas displays multiple outputs, each output representing one of the generated output files.

Apache Atlas displays the details of the published pipeline metadata as follows. Note how our example pipeline is listed with 1 input and 3 outputs - one output for each generated output file:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Atlas\_Details.png)

Apache Atlas displays the pipeline lineage diagram as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Atlas\_Lineage.png)

### Cloudera Navigator <a href="#ariaid-title9" id="ariaid-title9"></a>

You can configure BigConnect Pipelines to publish metadata about running pipelines to Cloudera Navigator. You then use Cloudera Navigator to explore the pipeline metadata, including viewing lineage diagrams of the metadata.

Data governance tool integration is available with a paid subscription. For details, contact BigConnect Pipelines.

To integrate BigConnect Pipelines with Cloudera Navigator, download and install one of the following Cloudera Navigator stage libraries as a custom stage library:

| Stage Library                          | Description                                          |
| -------------------------------------- | ---------------------------------------------------- |
| streamsets-datacollector-cm\_5\_10-lib | Use when working with Cloudera Manager version 5.10. |
| streamsets-datacollector-cm\_5\_11-lib | Use when working with Cloudera Manager version 5.11. |
| streamsets-datacollector-cm\_5\_12-lib | Use when working with Cloudera Manager version 5.12. |
| streamsets-datacollector-cm\_5\_13-lib | Use when working with Cloudera Manager version 5.13. |
| streamsets-datacollector-cm\_5\_14-lib | Use when working with Cloudera Manager version 5.14. |
| streamsets-datacollector-cm\_5\_15-lib | Use when working with Cloudera Manager version 5.15. |
| streamsets-datacollector-cm\_6\_1-lib  | Use when working with Cloudera Manager version 6.1.  |

You then configure BigConnect Pipelines to connect to Cloudera Navigator.

#### Prerequisites <a href="#ariaid-title10" id="ariaid-title10"></a>

Before you enable BigConnect Pipelines to publish pipeline metadata to Cloudera Navigator, complete the following prerequisites:

**Verify that BigConnect Pipelines has a required Cloudera CDH stage library version installed**\
\
To connect to and publish metadata to Cloudera Navigator, BigConnect Pipelines requires Cloudera Manager versions 5.10 through 5.15 or version 6.1.\
\
To verify that BigConnect Pipelines has a valid Cloudera CDH stage library installed, click the Package Manager icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PackageManager.png)) to display the list of installed stage libraries. If a required library version is not installed, install the library before configuring BigConnect Pipelines to publish pipeline metadata to Cloudera Navigator.\
\
**Add the Cloudera Navigator data management roles to your Cloudera Manager cluster**\
\
Add the following Navigator roles to the Cloudera Management Service:

* Cloudera Navigator Audit Server role
* Cloudera Navigator Metadata Server role

For instructions, see the Cloudera Manager documentation.

#### Step 1. Download and Install the Stage Library <a href="#ariaid-title11" id="ariaid-title11"></a>

Download and install the Cloudera Navigator stage library as a custom stage library.

1. Using the link provided in the email that you received from StreamSets, download the tarball containing the Cloudera Navigator stage library for your version of Cloudera Manager.For example, if BigConnect Pipelines has the Cloudera CDH stage library version 5.14 installed, download the `streamsets-datacollector-cm_5_14-lib-1.6.0.tgz` file.
2.  Extract the tarball.The tarball is extracted into the following directory:

    ```
    <stage library name>/lib/
    ```

    For example:

    ```
    streamsets-datacollector-cm_5_14-lib/lib/
    ```
3.  On every node that runs BigConnect Pipelines, create a local directory for the custom stage libraries.For example, you might create the custom stage library directory at:

    ```
    /opt/sdc-user-libs
    ```
4.  On every node that runs BigConnect Pipelines, copy the extracted Cloudera Navigator stage library to the directory defined for the USER\_LIBRARIES\_DIR environment variable as follows:

    ```
    /opt/sdc-user-libs/<stage library name>/lib/
    ```

    For example:

    ```
    /opt/sdc-user-libs/streamsets-datacollector-cm_5_14-lib/lib/
    ```
5. In Cloudera Manager, select the **BigConnect Pipelines** service and then click **Configuration**.
6.  On the **Configuration** page, in the **BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-env.sh** field, add the USER\_LIBRARIES\_DIR environment variable and point it to the custom stage library directory, as follows:

    ```
    export USER_LIBRARIES_DIR="<custom stage library directory>"
    ```

    For example:

    ```
    export USER_LIBRARIES_DIR="/opt/sdc-user-libs/"
    ```
7.  When using the Java Security Manager, which is enabled by default, update **the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-security.policy** property to include the custom stage library directory as follows:

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
8. Restart BigConnect Pipelines.

#### Step 2. Configure the Connection to Cloudera Navigator <a href="#ariaid-title12" id="ariaid-title12"></a>

Add lineage publisher properties to the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties,` and then configure the properties as needed. When administering BigConnect Pipelines with Cloudera Manager, configure the BigConnect Pipelines configuration properties through the StreamSets service in Cloudera Manager. Manual changes to the configuration file can be overwritten by Cloudera Manager.

**Important**: Instead of entering sensitive data such as passwords in clear text in the configuration file, you can protect the sensitive data by storing the data in an external location and then using functions to retrieve the data.

Configuring the properties includes enabling metadata publishing and defining the connection to Cloudera Navigator.

If Cloudera Navigator is configured for TLS/SSL, configuring the properties also requires specifying the local truststore file that BigConnect Pipelines uses to verify the identity of the Cloudera Navigator Metadata Server.

<mark style="color:red;">**Warning**</mark>: If metadata publishing is enabled and BigConnect Pipelines cannot connect to Cloudera Navigator at start up, then BigConnect Pipelines fails to start.

1.  To enable metadata publishing, add the following lines to the sdc.properties file:

    ```
    # Lineage Publisher
    #
    lineage.publishers=navigator
    lineage.publisher.navigator.def=streamsets-datacollector-cm_5_14-lib::com_streamsets_pipeline_stage_plugin_navigator_NavigatorLineagePublisher
    lineage.publisher.navigator.config.application_url=http://localhost:18630
    lineage.publisher.navigator.config.navigator_url=http://navigatorhost:7187
    lineage.publisher.navigator.config.namespace=sdc
    lineage.publisher.navigator.config.username=user
    lineage.publisher.navigator.config.password=passwd
    lineage.publisher.navigator.config.autocommit=false
    ```
2.  To define the connection to Cloudera Navigator, modify the default values of the following properties:

    | Metadata Publisher Property                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | lineage.publishers                                               | <p>Application to publish metadata to.<br></p><p>Use the default value, <code>navigator</code>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | lineage.publisher.navigator.def                                  | <p>Stage library name used to connect to Cloudera Navigator, entered in the following format:</p><p><code>&#x3C;stage library name>::com_streamsets_pipeline_ stage_plugin_navigator_NavigatorLineagePublisher</code></p><p><br>Define the appropriate Cloudera CDH stage library name to enable metadata publishing. For example, if you downloaded version 5.14 of the stage library, define the stage library name as follows:</p><pre><code>streamsets-datacollector-cm_5_14-lib
    </code></pre><p>Do not modify the second half of the property value.</p> |
    | <p>lineage.publisher.navigator.</p><p>config.application_url</p> | <p>URL to BigConnect Pipelines. For example:</p><p><code>http://&#x3C;hostname>:18630/</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | <p>lineage.publisher.navigator.</p><p>config.navigator_url</p>   | <p>URL to the Cloudera Navigator UI. For example:</p><p><code>http://&#x3C;Navigator Metadata Server host>:&#x3C;port number>/</code></p><p><code>&#x3C;Navigator Metadata Server host></code> is the name of the host on which you are running the Navigator Metadata Server role. <code>&#x3C;port number></code> is the port configured for the role.</p>                                                                                                                                                                                                  |
    | <p>lineage.publisher.navigator.</p><p>config.namespace</p>       | Namespace that Cloudera Navigator uses to uniquely identify BigConnect Pipelines properties.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | <p>lineage.publisher.navigator.</p><p>config.username</p>        | User name to connect to Cloudera Navigator.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | <p>lineage.publisher.navigator.</p><p>config.password</p>        | <p>Password for the Cloudera Navigator account.<br></p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                                                                                           |
    | <p>lineage.publisher.navigator.</p><p>config.autocommit</p>      | <p>Enables Cloudera Navigator to immediately process the published metadata.<br></p><p>In most cases, use the default value of <code>false</code>. Setting to true can use a large number of resources on the Cloudera Navigator machine.</p>                                                                                                                                                                                                                                                                                                                 |
3.  If Cloudera Navigator is configured for TLS/SSL, add the following lines to the Lineage Publisher section of the sdc.properties file and then modify the default values of the properties:\


    ```
    lineage.publisher.navigator.config.ssl_truststore_location=truststore.jks
    lineage.publisher.navigator.config.ssl_truststore_password=changeit
    ```

    | Metadata Publisher Property for TLS/SSL                                  | Description                                                                                                                                                                                                                                 |
    | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | <p>lineage.publisher.navigator.</p><p>config.ssl_truststore_location</p> | <p>Path to the truststore file containing the Cloudera Navigator Metadata Server certificate.</p><p>Enter an absolute path to the file or a path relative to the BigConnect Pipelines resources directory: <code>$SDC_RESOURCES</code>.</p> |
    | <p>lineage.publisher.navigator.</p><p>config.ssl_truststore_password</p> | <p>Password to the truststore file.</p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                         |
4. Restart BigConnect Pipelines for the changed properties to take effect.

#### Viewing Published Metadata <a href="#ariaid-title13" id="ariaid-title13"></a>

You can view published pipeline metadata in near real-time in Cloudera Navigator. Cloudera Navigator lists each pipeline by pipeline title and displays supported origins as inputs and supported destinations as outputs. Cloudera Navigator also includes a URL to the pipeline within BigConnect Pipelines along with other metadata, such as the pipeline ID, version, labels, and the user who started the pipeline.

For example, let's assume that you run a pipeline that includes a JDBC Query Consumer origin and a Local FS destination. Cloudera Navigator displays a single input representing the JDBC Query Consumer origin. As the pipeline runs, the Local FS destination creates multiple output files. Cloudera Navigator displays multiple outputs, each output representing one of the generated output files.

Cloudera Navigator displays the details of the published pipeline metadata as follows. Note how our example Jdbc2LocalFS pipeline is listed with 1 input and 42 outputs - one output for each generated output file:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Navigator\_Details.png)

Cloudera Navigator displays the pipeline lineage diagram as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Navigator\_Lineage.png)

### Collibra <a href="#ariaid-title14" id="ariaid-title14"></a>

You can configure BigConnect Pipelines to publish metadata about running pipelines to Collibra. You then use Collibra to explore the pipeline metadata, including viewing lineage diagrams of the metadata. You can configure BigConnect Pipelines version 3.20.x or later to publish metadata to the latest version of Collibra.

Data governance tool integration is available with a paid subscription. For details, contact BigConnect Pipelines.

To integrate BigConnect Pipelines with Collibra, download and install the following Collibra stage library as a custom stage library:

| Stage Library                         | Description                         |
| ------------------------------------- | ----------------------------------- |
| streamsets-datacollector-collibra-lib | Use to work with Collibra Platform. |

You then configure BigConnect Pipelines to connect to Collibra. You also must deploy the BigConnect Pipelines data model to the machine where the Collibra server is installed.

#### Prerequisites <a href="#ariaid-title15" id="ariaid-title15"></a>

Before you enable BigConnect Pipelines to publish pipeline metadata to Collibra, complete the following prerequisites.

This document includes steps for performing tasks in Collibra for your convenience. For the latest steps, see the Collibra documentation.

You must be a Collibra system administrator to complete these tasks:\
\
**Import BigConnect Pipelines Collibra resources** \
\
To connect to Collibra from BigConnect Pipelines, you must download and import the Collibra resource file.

1. If necessary, place a request with BigConnect Pipelines Support for access to the Collibra resource file, `streamsets-collibra.cma`.
2. In the Collibra user interface, from your Collibra instance, click **Settings > Migration > Import,** then upload the `streamsets-collibra.cma` file.

**Create a community in the Collibra instance**\
\
To connect to Collibra from BigConnect Pipelines, you must create a community in Collibra for BigConnect Pipelines, and assign the Community Manager role to a user account. You use the Community Manager user account to configure the connection to Collibra.

1. In the Collibra user interface, from your Collibra instance, click Create in the top menu bar.
2. In the **Create Community** dialog box, specify a name to uniquely identify the community, such as `BigConnect Pipelines`, then click **Create**.
3. in the **Add Responsibility** dialog box, specify a user for the Community Manager role, and click **Add**.

#### Step 1. Download and Install the Stage Library <a href="#ariaid-title16" id="ariaid-title16"></a>

Download and install the Collibra stage library as a custom stage library.

1. Using the link provided in the email that you received from BigConnect Pipelines, download the tarball containing the Collibra stage library.
2.  Extract the tarball.The tarball is extracted into the following directory:

    ```
    <stage library name>/lib/
    ```

    For example, the Collibra version 1.0.0 stage library is extracted to:

    ```
    streamsets-datacollector-collibra-lib/lib/
    ```
3.  Create a local directory external to the BigConnect Pipelines installation directory for the custom stage libraries. Use an external directory to enable use of the libraries after BigConnect Pipelines upgrades.For example, if you installed BigConnect Pipelines in the following directory:

    ```
    /opt/sdc/
    ```

    you might create the custom stage library directory at:

    ```
    /opt/sdc-user-libs
    ```
4.  Copy the Collibra stage library to this custom stage library directory, as follows:

    ```
    /opt/sdc-user-libs/<stage library name>/lib/
    ```

    For example, for Collibra version 1.0.0:

    ```
    /opt/sdc-user-libs/streamsets-datacollector-collibra-lib/lib/
    ```
5.  Add the USER\_LIBRARIES\_DIR environment variable to the appropriate file and point it to the custom stage library directory.\


    Modify environment variables using the method required by your installation type.

    \
    Set the environment variable as follows:

    ```
    export USER_LIBRARIES_DIR="<custom stage library directory>"
    ```

    For example:

    ```
    export USER_LIBRARIES_DIR="/opt/sdc-user-libs/"
    ```
6. When using the Java Security Manager, which is enabled by default, update the BigConnect Pipelines security policy to include the custom stage library directory as follows:
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

       \
       **Note**: You must include a trailing /- after the directory name.For example:

       ```
       // custom stage library directory
       grant codebase "file:///opt/sdc-user-libs/-" {
          permission java.security.AllPermission;
       };
       ```
7. Restart BigConnect Pipelines.

#### Step 2. Configure the Connection to Collibra <a href="#ariaid-title17" id="ariaid-title17"></a>

Configuring BigConnect Pipelines to connect to Collibra involves adding lineage publisher properties to the BigConnect Pipelines configuration file.

1.  To enable metadata publishing to Collibra, modify the BigConnect Pipelines configuration file, $SDC\_CONF/sdc.properties.\


    Add the following lines to the file:

    ```
    # Lineage Publisher
    lineage.publishers=collibra
    lineage.publisher.collibra.def=streamsets-datacollector-collibra-lib::com_streamsets_pipeline_stage_plugin_collibra_CollibraLineagePublisher
    lineage.publisher.collibra.config.collibra_url=<collibra url>
    lineage.publisher.collibra.config.community=<community name>
    lineage.publisher.collibra.config.username=<user name>
    lineage.publisher.collibra.config.password=<password>
    ```
2.  To define the connection to Collibra, configure the properties as follows:

    | Metadata Publisher Property                     | Description                                                                                                                                                                                                        |
    | ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | lineage.publishers                              | Application to publish metadata to. Use the default value: `collibra`.                                                                                                                                             |
    | lineage.publisher.collibra.def                  | <p>Stage library name used to connect to Collibra.Use the default value:</p><pre><code>streamsets-datacollector-collibra-lib::com_streamsets_pipeline_stage_plugin_collibra_CollibraLineagePublisher
    </code></pre> |
    | lineage.publisher.collibra.config.collibra\_url | <p>URL for your Collibra instance. Use the following format:</p><pre><code>https://&#x3C;collibra host>:&#x3C;collibra port>
    </code></pre>                                                                         |
    | lineage.publisher.collibra.config.community     | <p>Community that Collibra uses to uniquely identify BigConnect Pipelines properties.</p><p><br>Specify the community name that you created in the prerequisite tasks.</p>                                         |
    | lineage.publisher.collibra.config.username      | <p>User name to connect to Collibra.<br></p><p>Specify the user account that you assigned to the Community Manager role in the prerequisite tasks.</p>                                                             |
    | lineage.publisher.collibra.config.password      | Password for the Collibra account.                                                                                                                                                                                 |
3. Restart BigConnect Pipelines.

#### Viewing Published Metadata <a href="#ariaid-title18" id="ariaid-title18"></a>

BigConnect Pipelines publishes pipeline metadata to Collibra when a pipeline stops. Collibra lists each pipeline by pipeline title and displays supported origins as inputs and supported destinations as outputs. Collibra also includes other metadata, such as the pipeline ID, version, and the user who started the pipeline.

For example, say you run a pipeline that includes an Oracle Bulkloader origin and a Local FS destination. Collibra displays a single input representing the origin. As the pipeline runs, the Local FS destination creates multiple output files. Collibra displays multiple outputs, each output representing one of the generated output files.

Collibra displays the details of the published pipeline metadata as follows. Note how the pipeline is listed with 1 input and 4 outputs - one output for each generated output file:

![](../../../../.gitbook/assets/calibra\_pipeline.png)

Collibra displays the pipeline lineage diagram as follows:

![](../../../../.gitbook/assets/calibra\_lineage\_diagram.png)
