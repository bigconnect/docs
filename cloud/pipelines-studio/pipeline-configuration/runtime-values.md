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

You define runtime properties in a file local to the DataFabric, and then you call the properties from within a pipeline. At runtime, DataFabric loads the property values from the file. A runtime properties file can contain multiple properties.

Runtime properties are defined for the entire DataFabric - any pipeline can call them.

**Runtime resources**

Use runtime resources when you want to store common pipeline configuration properties in files with restricted permissions.

You define runtime resources in a file local to the DataFabric, and then you call the resources from within a pipeline. You can restrict the permissions for the resource files, but any user who can create a pipeline can access the data stored in the files.

{% hint style="info" %}
To more securely define sensitive values, use credential stores.
{% endhint %}

## Using Runtime Parameters

Runtime parameters are parameters that you define in a pipeline and then call from within that same pipeline. When you start the pipeline, you specify the parameter values to use. Use runtime parameters to specify values for pipeline properties when you start the pipeline.

Use runtime parameters to define values for stage and pipeline properties. For example, you can define an error directory parameter that points to different directories on a test machine and a production machine. Or you can define a connection parameter that points to different database connections for an origin in the test and production environments.

When you define a runtime parameter, you enter the default value to use. When you start the pipeline, you can specify another value to override the default. When the pipeline runs, the value replaces the name of the runtime parameter.

{% hint style="info" %}
If you shut down and then restart DataFabric without stopping the pipeline, the pipeline continues running with the last set of parameter values.
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

You can use runtime parameters to represent any stage or pipeline property that allows the use of the DataFabric expression language, including properties that display as text boxes, checkboxes, or drop-down menus. You can also call a runtime parameter in the code developed for a scripting processor.

### Calling from Text Boxes

To call a runtime parameter in a stage or pipeline property that displays as a text box, use the following syntax:

`${<parameter name>}`

For example, to use the JDBCConnectionString runtime parameter for the JDBC Multitable Consumer origin, enter the following syntax for the JDBC Connection String property:

`${JDBCConnectionString}`

You can call a runtime parameter from within an expression language function by simply entering the parameter name. For example, the following expression returns the value of the JDBCConnectionString runtime parameter:

 `${record:value(JDBCConnectionString)}`

You can use a runtime parameter to represent a part of a property. For example, you could use a RootDir runtime parameter and append the rest of the directory in the property as follows:

`${RootDir}/logfiles`

### Calling from Checkboxes and Drop-Down Menus

To call a runtime parameter in a stage or pipeline property that displays as a checkbox or drop-down menu, you first must convert the property to a text box.

Click the Use Parameter icon \(![](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_UseParam.png)\) next to the checkbox or drop-down menu to convert the property to a text box, and then call the parameter using the required syntax.

For example, the following image shows that the Delivery Guarantee property that displays as a drop-down menu has been converted to a text box so that a parameter can be called from the property:

![](../../../.gitbook/assets/image%20%28108%29.png)

The parameter must evaluate to a valid option for the property type:

**Checkboxes**

Parameters called from properties that display as checkboxes must evaluate to true or false.

**Drop-down menus**

Parameters called from properties that display as drop-down menus must evaluate to a valid key value. Each option in the menu has an associated key value.

For example, to use a parameter for the Delivery Guarantee property that displays as a drop-down menu, the parameter must evaluate to one of the valid key values, AT\_LEAST\_ONCE or AT\_MOST\_ONCE, and not to one of the menu options, At Least Once or At Most Once.

To view a valid key value, select the desired option from the menu, then click the **Use Parameter** icon. The text box displays the key value for the selected option. 

For example, after selecting At Least Once for the Delivery Guarantee property and then clicking the Use Parameter icon, the text box displays AT\_LEAST\_ONCE as the key value for the At Least Once option:

![](../../../.gitbook/assets/image%20%28500%29.png)

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

1. From the pipeline canvas, click the More icon, and then click **Start with Parameters**.If Start with Parameters is not enabled, the pipeline is not valid.

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

![](../../../.gitbook/assets/image%20%28327%29.png)

The Runtime Parameters chart does not display when you start a pipeline without parameters.

