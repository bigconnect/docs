# Runtime Values

Runtime values are values that you define outside of the pipeline and use for stage and pipeline properties. You can change the values for each pipeline run without having to edit the pipeline.

You can use runtime values for any pipeline property that allows the use of the expression language. You can, for example, use runtime values to represent batch sizes, timeouts, directories, and URI. You cannot use runtime values to represent fields.

You can use the following methods of passing runtime values to pipelines:

**Runtime parameters**

Use runtime parameters when you want to specify the values for pipeline properties when you start the pipeline.

You define runtime parameters when you configure the pipeline, and then you call the parameters from within that pipeline. When you start the pipeline, you specify the parameter values to use.

Runtime parameters are defined for a single pipeline - only that pipeline can call them.

**Runtime properties**

Use runtime properties when you want to define values for multiple pipeline properties in a file.

You define runtime properties in a file local to the Data Pipelines, and then you call the properties from within a pipeline. At runtime, Data Pipelines loads the property values from the file. A runtime properties file can contain multiple properties.

Runtime properties are defined for the entire Data Pipelines - any pipeline can call them.

**Runtime resources**

Use runtime resources when you want to store common pipeline configuration properties in files with restricted permissions.

You define runtime resources in a file local to Data Pipelines, and then you call the resources from within a pipeline. You can restrict the permissions for the resource files, but any user who can create a pipeline can access the data stored in the files.

{% hint style="info" %}
To more securely define sensitive values, use credential stores.
{% endhint %}

## Using Runtime Parameters

Runtime parameters are parameters that you define in a pipeline and then call from within that same pipeline. When you start the pipeline, you specify the parameter values to use. Use runtime parameters to specify values for pipeline properties when you start the pipeline.

Use runtime parameters to define values for stage and pipeline properties. For example, you can define an error directory parameter that points to different directories on a test machine and a production machine. Or you can define a connection parameter that points to different database connections for an origin in the test and production environments.

When you define a runtime parameter, you enter the default value to use. When you start the pipeline, you can specify another value to override the default. When the pipeline runs, the value replaces the name of the runtime parameter.

{% hint style="info" %}
If you shut down and then restart Data Pipelines without stopping the pipeline, the pipeline continues running with the last set of parameter values.
{% endhint %}

To implement runtime parameters, perform the following steps:

1. Define runtime parameters.
2. Use an expression in the pipeline to call a runtime parameter.
3. Start the pipeline with parameter values.

When you monitor a pipeline started with parameters, you can view the parameter values that the pipeline is currently using.

## Step 1. Define Runtime Parameters

Define runtime parameters when you configure the pipeline.

1. In the pipeline properties, click the Parameters tab.
2. Using simple or bulk edit mode, click the Add icon and define the name and the default value for each parameter.For example, define a parameter named JDBCConnectionString with the default value of `jdbc:mysql://localhost:3306/sample`.

## Step 2. Call the Runtime Parameters

Use an expression in the pipeline to call a runtime parameter.

You can use runtime parameters to represent any stage or pipeline property that allows the use of the Data Pipelines expression language, including properties that display as text boxes, checkboxes, or drop-down menus. You can also call a runtime parameter in the code developed for a scripting processor.

### Calling from Text Boxes

To call a runtime parameter in a stage or pipeline property that displays as a text box, use the following syntax:

`${<parameter name>}`

For example, to use the JDBCConnectionString runtime parameter for the JDBC Multitable Consumer origin, enter the following syntax for the JDBC Connection String property:

`${JDBCConnectionString}`

You can call a runtime parameter from within an expression language function by simply entering the parameter name. For example, the following expression returns the value of the JDBCConnectionString runtime parameter:

&#x20;`${record:value(JDBCConnectionString)}`

You can use a runtime parameter to represent a part of a property. For example, you could use a RootDir runtime parameter and append the rest of the directory in the property as follows:

`${RootDir}/logfiles`

### Calling from Checkboxes and Drop-Down Menus

To call a runtime parameter in a stage or pipeline property that displays as a checkbox or drop-down menu, you first must convert the property to a text box.

Click the Use Parameter icon (![](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon\_UseParam.png)) next to the checkbox or drop-down menu to convert the property to a text box, and then call the parameter using the required syntax.

For example, the following image shows that the Delivery Guarantee property that displays as a drop-down menu has been converted to a text box so that a parameter can be called from the property:

![](<../../../.gitbook/assets/image (853).png>)

The parameter must evaluate to a valid option for the property type:

**Checkboxes**

Parameters called from properties that display as checkboxes must evaluate to true or false.

**Drop-down menus**

Parameters called from properties that display as drop-down menus must evaluate to a valid key value. Each option in the menu has an associated key value.

For example, to use a parameter for the Delivery Guarantee property that displays as a drop-down menu, the parameter must evaluate to one of the valid key values, AT\_LEAST\_ONCE or AT\_MOST\_ONCE, and not to one of the menu options, At Least Once or At Most Once.

To view a valid key value, select the desired option from the menu, then click the **Use Parameter** icon. The text box displays the key value for the selected option.&#x20;

For example, after selecting At Least Once for the Delivery Guarantee property and then clicking the Use Parameter icon, the text box displays AT\_LEAST\_ONCE as the key value for the At Least Once option:

![](<../../../.gitbook/assets/image (699).png>)

### Calling from Scripting Processors

You can call a runtime parameter in the code developed for a scripting processor.

The method you use to call the runtime parameter depends on the following scripting processor types:

**JavaScript Evaluator or Jython Evaluator processor**

Use the following syntax in any of the processor scripts: `${<parameter name>}`. For example, the following line of JavaScript code assigns the value of the NewFieldValue parameter to a map field:

`records[i].value.V= ${NewFieldValue}`

Groovy Evaluator processor

Use the `sdcFunctions.pipelineParameters()` method in any of the processor scripts to return a map of all runtime parameters defined for the pipeline. For example, the following line of Groovy code assigns the value of the CompanyParam parameter to the Company Name field:

`record.value['Company Name'] = sdcFunctions.pipelineParameters()['CompanyParam']`

## Step 3. Start the Pipeline with Parameters

When you start the pipeline, specify the values to use for the parameters.

{% hint style="info" %}
If you want to use the default parameter values, you can simply click the Start icon to start the pipeline.
{% endhint %}

1.  From the pipeline canvas, click the More icon, and then click **Start with Parameters**.If Start with Parameters is not enabled, the pipeline is not valid.

    The **Start with Parameters** dialog box lists all parameters defined for the pipeline and their default values.
2. Override any default values with the values you want to use for this pipeline run.
3. Click Start.

## Viewing Runtime Parameters

When you monitor a pipeline started with parameters, you can view the parameter values that the pipeline is currently using.

Before you can view the current runtime parameter values, verify that the monitoring settings include the Runtime Parameters chart.

1. In the Summary tab of the Monitor panel, click the More icon, and then click Settings.
2. In the Monitoring Settings dialog box, click in the Charts field and then select Runtime Parameters.
3. Click Save.

When you monitor a pipeline that you started with parameter values, the Runtime Parameters chart displays as follows:

<div align="left">

<img src="../../../.gitbook/assets/image (753).png" alt="">

</div>

The Runtime Parameters chart does not display when you start a pipeline without parameters.\


### Using Runtime Properties <a href="#ariaid-title10" id="ariaid-title10"></a>

Runtime properties are properties that you define in a file local to the BigConnect Pipelines and call from within a pipeline.

With runtime properties, you can define different sets of values for different BigConnect Pipelines. Runtime properties allow you to easily update the property definitions without editing the pipeline.

For example, you might define a set of runtime properties that points to test systems for pipeline development. When you move to production, you simply update the runtime property definitions in the file. This effectively updates every pipeline that uses those runtime properties without having to edit the pipelines.

When defining a runtime property, you can use a static value or an environment variable.

When calling the runtime property, you can use it as part of a larger property definition. For example, you can set a runtime property to the HOME environment variable, which would differ on different machines, and then use the runtime property as a base directory for a longer directory.

To implement runtime properties, perform the following steps:

1. Define runtime properties.
2. Use an expression in the pipeline to call a runtime property.

#### Step 1. Define Runtime Properties <a href="#ariaid-title11" id="ariaid-title11"></a>

You can define runtime properties in the BigConnect Pipelines configuration file, `sdc.properties`, or in a separate runtime properties file.

If you define the properties in a separate runtime properties file, use the required procedure for your installation type.

**BigConnect Pipelines configuration file**\
\
Use the following steps to define runtime properties in the BigConnect Pipelines configuration file:

1.  In the `$SDC_CONF/sdc.properties` file, configure the **runtime.conf.location** property as follows:

    ```
    runtime.conf.location=embedded
    ```
2. To define the runtime properties in the `$SDC_CONF/sdc.properties` file, use either of the following formats:
   *   To use a value for a runtime property, use the following format:

       ```
       runtime.conf_<property name>=<value>
       ```

       For example, the following runtime property defines a Hadoop FS directory template:

       ```
       runtime.conf_HDFSDirTemplate=/HDFS/DirectoryTemplate
       ```
   *   To use an environment variable for a runtime property, use the following format:

       ```
       runtime.conf_<property name>=${env("<environment_variable>")}
       ```

       For example, the following runtime property defines a base directory, setting it to the HOME environment variable:

       ```
       runtime.conf_BaseDir=${env("HOME")}
       ```
3. Restart BigConnect Pipelines to enable the changes.

**Separate runtime properties file for RPM and tarball**\
\
Use the following steps to define runtime properties in a separate runtime properties file for an RPM or tarball installation:

1. Create a text file and save it in a directory relative to the `$SDC_CONF` directory.
2. To define runtime properties in a separate text file, use either of the following formats:
   *   To use a value for a runtime property, use the following format:

       ```
       <property name>=<value>
       ```

       For example, the following runtime property defines a Hadoop FS directory template:

       ```
       HDFSDirTemplate=/HDFS/DirectoryTemplate
       ```
   *   To use an environment variable for a runtime property, use the following format:

       ```
       <property name>=${env("<environment_variable>")}
       ```

       For example, the following runtime property defines a base directory, setting it to the HOME environment variable:

       ```
       BaseDir=${env("HOME")}
       ```
3.  In the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`, configure the **runtime.conf.location** property to point to the relative location of the separate runtime properties file.

    For example, the following separate runtime properties file is located in a `runtime` directory that is relative to the `$SDC_CONF` directory:

    ```
    runtime.conf.location=runtime/test-runtime.properties
    ```
4. Restart BigConnect Pipelines to enable the changes.

**Separate runtime properties file for Cloudera Manager**\
\
Use the following steps to define runtime properties in a separate runtime properties file for a Cloudera Manager installation:

1. Create a text file and define the runtime properties in the text file using either of the following formats:
   *   To use a value for a runtime property, use the following format:

       ```
       <property name>=<value>
       ```

       For example, the following runtime property defines a Hadoop FS directory template:

       ```
       HDFSDirTemplate=/HDFS/DirectoryTemplate
       ```
   *   To use an environment variable for a runtime property, use the following format:

       ```
       <property name>=${env("<environment_variable>")}
       ```

       For example, the following runtime property defines a base directory, setting it to the HOME environment variable:

       ```
       BaseDir=${env("HOME")}
       ```
2. Save the text file in the same directory on every node that runs BigConnect Pipelines.
3. In Cloudera Manager, select the BigConnect Pipelines service and click Configuration.
4.  On the Configuration page, in the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc-env.sh field, add the following line to define the runtime properties file directory:

    ```
    ln -sf /<directory>/runtime.properties "${CONF_DIR}/runtime.properties"
    ```

    For example:

    ```
    ln -sf /opt/sdc-runtime/runtime.properties "${CONF_DIR}/runtime.properties"
    ```
5.  In the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc.properties field, configure the runtime.conf.location property to point to the separate runtime properties file by adding the following line:

    ```
    runtime.conf.location=runtime.properties 
    ```
6. Restart BigConnect Pipelines to enable the changes.

For more information, see Configuring BigConnect Pipelines.

#### Step 2. Call the Runtime Property <a href="#ariaid-title12" id="ariaid-title12"></a>

Use the `runtime:conf` function to call a runtime property. You can use runtime properties to represent any stage or pipeline property that allows the use of the expression language.

Call a runtime property based on where the property is defined:

*   To call a runtime property defined in the BigConnect Pipelines configuration file, use the following syntax:

    ```
    ${runtime:conf(<property name>)}
    ```
*   To call a runtime property defined in a runtime properties file, use the following syntax:

    ```
    ${runtime:conf(runtime.conf_<property name>)}
    ```

For example, to use the `HDFSDirTemplate` runtime property defined in the BigConnect Pipelines configuration file to provide the directory template for the Hadoop FS destination, enter the following syntax for the Directory Template property:

```
${runtime:conf('HDFSDirTemplate')}
```

You can use a runtime property to represent a part of a property. For example, you can use the `RootDir` runtime property defined in a separate runtime properties file and append the rest of the directory, as follows:

```
${runtime:conf('runtime.conf_RootDir')}/logfiles
```

### Using Runtime Resources <a href="#ariaid-title13" id="ariaid-title13"></a>

Similar to runtime properties, runtime resources are values that you define in a file local to the BigConnect Pipelines and call from within a pipeline. But with runtime resources, you can restrict the permissions for the files to secure information.

Use runtime resources to store common configuration properties for multiple pipelines, like URLs for external systems. Note that any user who can create a pipeline can access the data stored in the resource files.

**Tip**: To more securely define sensitive values, use credential stores.\
\
To implement runtime resources, perform the following steps:

1. Define each runtime resource.
2. Use an expression in the pipeline to call a runtime resource.

#### Step 1. Define Runtime Resources <a href="#ariaid-title14" id="ariaid-title14"></a>

Use the following steps to define runtime resources:

1.  For each resource, create a text file and save it in the `$SDC_RESOURCES` directory.

    A file must contain one piece of information to be used when the resource is called.
2.  Optionally, restrict the permissions for the file.

    Generally, anyone can read a file. To restrict permissions, configure the file so only the owner has read or write permissions for the file - in octals, that's 600 or 400. And the owner must be the system user that runs the BigConnect Pipelines.

    When you use the resource in the pipeline, you specify whether the file is restricted.

#### Step 2. Call the Runtime Resource <a href="#ariaid-title15" id="ariaid-title15"></a>

Use the `runtime:loadResource` or `runtime:loadResourceRaw` function to call a runtime resource. You can use runtime resources to represent information in any stage or pipeline property that allows the use of the expression language.

Note: In most cases, you'll use the `runtime:loadResource` function which trims any leading or trailing whitespace characters from the file. However, if needed, you can also use the `runtime:loadResourceRaw` function which includes any leading or trailing whitespace characters in the file.

To call a runtime resource, use the following syntax:

```
${runtime:loadResource(<file name>, <restricted: true | false>)}
```

For example, the following expression returns the contents of the `JDBC.txt` file, trimming any leading or trailing whitespace characters. The file contains a connection string and is restricted so only the owner can read the file:

```
${runtime:loadResource("JDBC.txt", true)}
```
