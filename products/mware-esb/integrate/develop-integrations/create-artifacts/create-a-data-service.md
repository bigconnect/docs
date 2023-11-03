# Create a Data Service

Follow the instructions given below to create a new data service artifact.

Tip

You can also use a sample template to create your data service.

1. Open the **Getting Started** view of WSO2 Integration Studio (**Menu -> Help -> Getting Started**).
2. In the Getting Started view, go to the **Data Service** tab and select the **REST Data Service** example.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#instructions) <a href="#instructions" id="instructions"></a>

#### Create the data service artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#create-the-data-service-artifact) <a href="#create-the-data-service-artifact" id="create-the-data-service-artifact"></a>

Follow the steps given below to create the data service file:

1.  Right-click the **Data Service Config** module in the project explorer and go to **New -> Data Service**.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/new-data-service.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/new-data-service.png)
2.  In the **New Data Service** wizard that opens, select **Create New Data Service** and click **Next**.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/119130577/119130578.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/119130577/119130578.png)
3. Enter a name for the data service and click **Finish**.

A data service file (DBS file) will now be created in your data service project as show below.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/data-service-project-structure.png)

#### Adding a datasource[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#adding-a-datasource) <a href="#adding-a-datasource" id="adding-a-datasource"></a>

You can configure the datasource connection details using this section.

1.  Click **Data Sources** to expand the section.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add-datasource-1.png)
2.  Click **Add New** to open the **Create Datasource** page.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add-datasource-2.png)
3. Enter the datasource connection details.
4.  Click **Test Connection** to expand the section.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/test\_connection.png)
5. Click the **Test Connection** button to verify the connectivity between the MySQL datasource and the data service.
6. Save the data service.

#### Creating a query[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#creating-a-query) <a href="#creating-a-query" id="creating-a-query"></a>

You can configure the main query details using this section.

1.  Click **Queries** to expand the section.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/query\_expanded.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/query\_expanded.png)
2.  Click **Add New** to open the **Add Query** page.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add\_query.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add\_query.png)
3.  Enter the following query details.

    | Parameter  | Description                                                                                                 |
    | ---------- | ----------------------------------------------------------------------------------------------------------- |
    | Query ID   | Give a unique name to Identify the Query.                                                                   |
    | Datasource | All the datasources created for this data service are listed. Select the required datasource from the list. |
    | SQL Query  | You can enter the SQL query in this text box.                                                               |

**Input mapping**[**¶**](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#input-mapping)

You can configure input parameters for the query using this section.

1.  Click **Input Mappings** to expand the section.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/input\_mapping\_expanded.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/input\_mapping\_expanded.png)
2. There are two ways to create the mapping:
   * You can click **Generate** to automatically generate the input mappings from the SQL query.
   * If you want to add a new input mapping:
     1.  Click **Add New** to open the **Add Input Mapping** page.

         [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add\_input\_mappings.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add\_input\_mappings.png)
     2.  Enter the following input mapping details:

         | Parameter      | Description                  |
         | -------------- | ---------------------------- |
         | Mapping Name   | Give a name for the mapping. |
         | Parameter Type | The parameter type.          |
         | SQL Type       | The SQL type.                |
     3. Save the input mapping.

Shown below is an example query with input mapping:

[![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/input\_mappings.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/input\_mappings.png)

**Result (Output Mappings)**[**¶**](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#result-output-mappings)

You can configure output result parameters for the query using this section.

1.  Click **Result (Output Mappings)** to expand the section.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/out\_mapping\_expanded.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/out\_mapping\_expanded.png)
2.  Enter the following details:

    | Property           | Description |
    | ------------------ | ----------- |
    | Grouped by Element | Employees   |
3. There are two ways to create the output mapping:
   * You can click **Generate** to automatically generate the output mappings from the SQL query.
   * Alternatively, you can manually add the mappings:
     1.  Click **Add New** to open the **Add Output Mapping** page.

         [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add\_output\_mappings.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add\_output\_mappings.png)
     2.  Enter the following output element details.

         | Property               | Description    |
         | ---------------------- | -------------- |
         | Datasource Type        | column         |
         | Output Field Name      | EmployeeNumber |
         | Datasource Column Name | EmployeeNumber |
         | Schema Type            | String         |
     3. Save the element.
     4. Follow the same steps to create the remaining output elements.

Shown below is an example query with output mappings:

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/output\_mapings.png)

**Advanced properties**[**¶**](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#advanced-properties)

Click **Advanced Properties** to expand the section and add the required parameter values.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/advances\_properties\_expanded.png)

The data service should now have the query element added.

#### Adding a SOAP operation[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#adding-a-soap-operation) <a href="#adding-a-soap-operation" id="adding-a-soap-operation"></a>

Use this section to configure a SOAP operation for invoking the data service.

1.  Click **Operations** to expand the section.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/new-operataion.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/new-operataion.png)
2.  Click **Add New** to add a SOAP Operation for your data service.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add-operation.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/add-operation.png)
3.  Enter the following information:

    | Parameter            | Description                                           |
    | -------------------- | ----------------------------------------------------- |
    | Operation Name       | Give a name to the SOAP Operation.                    |
    | Query ID             | Select the Query from the listed queries.             |
    | Operation Parameters | Click Add New to add new parameters to the operation. |

#### Adding a Resource[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#adding-a-resource) <a href="#adding-a-resource" id="adding-a-resource"></a>

Use this section to configure a REST resource for invoking the data service.

1.  Click **Resources** to expand the section.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/resource\_expanded.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/resource\_expanded.png)
2.  Click **Add New** to add a new resource.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/create\_resource.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/create\_resource.png)
3.  Give the following details to create the REST resource.

    | Parameter     | Description                                                                             |
    | ------------- | --------------------------------------------------------------------------------------- |
    | Resource Path | Give the HTTP REST resource path you need to expose.                                    |
    | Query ID      | Select the Query ID from the drop down list that you need to expose as a REST resource. |
4. Click **Save** to add the resource to the data service.

The data service should now have the resource added.

#### Generate Data Service from a Datasource[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-data-services/#generate-data-service-from-a-datasource) <a href="#generate-data-service-from-a-datasource" id="generate-data-service-from-a-datasource"></a>

Follow the steps given below to automatically create a data service using a given datasource structure. When generating a data service, the server takes its table structure according to the structure specified in the datasource and automatically creates the SELECT, INSERT, UPDATE, and DELETE operations.

1. Create a datasource project and add a datasource in the current workspace. You can refer [Creating a Datasource](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/data-services/creating-datasources) for more information.
2. In the **New Data Service** wizard that opens, select **Generate Data Service from Datasource** and click **Next**. [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/generate\_dataservice.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/generate\_dataservice.png)
3.  From the wizard, select the datasource that you have configured in step 1.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/generate\_dataservice\_select\_datasource.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/generate\_dataservice\_select\_datasource.png)
4.  Select the driver to connect to the datasource. You need to browse and upload a driver from your file system.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/select\_driver\_file\_system.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/select\_driver\_file\_system.png)

    Then click **Fetch Table** to list down all avaialble tables in the selected datasource.
5.  From the list of tables, select the tables and the REST resource methods that you want in the generated data service.

    Note

    1. The **POST** REST method is enabled only when the database is not in read-only mode.
    2. The **PUT** and **DELETE** REST methods are enabled only when a primary key is defined on the table.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/select\_tables.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/select\_tables.png)
6. You can select a service generation mode from the following two options:
   * Single Service: Creates a single data service for resources of all tables. If this option is selected, you need to provide a name for the Data Service you are creating.
   * Multiple Services: Creates a service per table, which will contain isolated resources for each table.
7. Click **Finish** to generate the services and add to the dataservices project.

\
