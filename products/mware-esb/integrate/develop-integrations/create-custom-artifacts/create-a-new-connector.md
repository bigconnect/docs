# Create a New Connector

You can write a new connector for a specific requirement that cannot be addressed via any of the existing connectors that can be downloaded from the [connector store](https://store.wso2.com/store/pages/top-assets).

Follow the steps given below to write a new connector to integrate with the **Google Books** service. You can then use the connector inside a mediation sequence to connect with Google Books and get information.

### Writing a new connector[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#writing-a-new-connector) <a href="#writing-a-new-connector" id="writing-a-new-connector"></a>

Follow the steps given below to write the new connector.

#### Prerequisites[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

Download and install Apache Maven.

#### Step 1: Creating the Maven project template[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#step-1-creating-the-maven-project-template) <a href="#step-1-creating-the-maven-project-template" id="step-1-creating-the-maven-project-template"></a>

We will use the [maven archetype](https://github.com/wso2-extensions/archetypes/tree/master/esb-connector-archetype) to generate the Maven project template and sample connector code.

1.  Open a terminal, navigate to the directory on your machine where you want the new connector to be created, and run the following command:

    ```xml
    mvn org.apache.maven.plugins:maven-archetype-plugin:2.4:generate -DarchetypeGroupId=org.wso2.carbon.extension.archetype -DarchetypeArtifactId=org.wso2.carbon.extension.esb.connector-archetype -DarchetypeVersion=2.0.4 -DgroupId=org.wso2.carbon.esb.connector -DartifactId=org.wso2.carbon.esb.connector.googlebooks -Dversion=1.0.0 -DarchetypeRepository=http://maven.wso2.org/nexus/content/repositories/wso2-public/
    ```

    2\. When prompted, enter a name for the connector. For example, **googleBooks**. 3. When prompted for confirmation, enter **y**.

The `org.wso2.carbon.esb.connector.googlebooks` directory is now created with a directory structure consisting of a `pom.xml` file, `src`tree, and `repository` tree.

#### Step 2: Adding the new connector resources[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#step-2-adding-the-new-connector-resources) <a href="#step-2-adding-the-new-connector-resources" id="step-2-adding-the-new-connector-resources"></a>

Now, let's configure files in the `org.wso2.carbon.esb.connector.googlebooks/src/main/resources` directory:

1. Create a directory named **googlebooks\_volume** in the `/src/main/resources` directory.
2.  Create a file named `listVolume.xml` with the following content in the **googlebooks\_volume** directory:

    ```xml
    <template name="listVolume" xmlns="http://ws.apache.org/ns/synapse">
    <parameter name="searchQuery" description="Full-text search query string." />
        <sequence>
          <property name="uri.var.searchQuery" expression="$func:searchQuery" />
            <call>
                <endpoint>
                    <http method="get" uri-template="https://www.googleapis.com/books/v1/volumes?q={uri.var.searchQuery}" />
                </endpoint>
            </call>
        </sequence>
    </template>
    ```
3.  Create a file named `component.xml` in the **googlebooks\_volume** directory and add the following content:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <component name="googlebooks_volume" type="synapse/template">
        <subComponents>
            <component name="listVolume">
                <file>listVolume.xml</file>
                <description>Lists volumes that satisfy the given query.</description>
            </component>
        </subComponents>
    </component>
    ```
4.  Edit the `connector.xml` file in the `src/main/resources` directory and replace the contents with the following dependency:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <connector>
        <component name="googleBooks" package="org.wso2.carbon.connector" >
          <dependency component="googlebooks_volume"/>
            <description>wso2 sample connector library</description>
        </component>
    </connector>
    ```
5.  Create a folder named **icon** in the `/src/main/resources` directory and add two icons.

    Tip

    You can download icons from the following location: [icons](http://svn.wso2.org/repos/wso2/scratch/connectors/icons/)

You are now ready to build the connector.

#### Step 3: Building the connector[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#step-3-building-the-connector) <a href="#step-3-building-the-connector" id="step-3-building-the-connector"></a>

Open a terminal, navigate to the `org.wso2.carbon.esb.connector.googlebooks` directory and execute the following maven command:

```bash
mvn clean install
```

This builds the connector and generates a ZIP file named `googleBooks-connector-1.0.0.zip` in the `target` directory.

### Using the new connector[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#using-the-new-connector) <a href="#using-the-new-connector" id="using-the-new-connector"></a>

Now, let's look at how you can use the new connector in a mediation sequence.

#### Step 1: Adding the connector to your mediation sequence[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#step-1-adding-the-connector-to-your-mediation-sequence) <a href="#step-1-adding-the-connector-to-your-mediation-sequence" id="step-1-adding-the-connector-to-your-mediation-sequence"></a>

1. [Set up WSO2 Integration Studio](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/installing-wso2-integration-studio).
2.  [Create an ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project) and [import the connector](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/adding-connectors/#importing-connectors) to your project.

    Tip

    Be sure to select the new `googleBooks-connector-1.0.0.zip` file from your `org.wso2.carbon.esb.connector.googlebooks/target` directory.
3. [Create a custom proxy service](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/creating-a-proxy-service) named **googlebooks\_listVolume**. In the **Design View**, you will note that the new connector is added to the tool palette. ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new-connector-pallet.png)
4.  Now, update the proxy service as shown below. You will be defining a mediation logic using the **Property** mediator, the new **googleBooks** connector, and the **Respond** mediator:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="googlebooks_listVolume"
           transports="https,http"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <inSequence>
             <property name="searchQuery" expression="json-eval($.searchQuery)"/>
             <googleBooks.listVolume>
                <searchQuery>{$ctx:searchQuery}</searchQuery>
             </googleBooks.listVolume>
             <respond/>
          </inSequence>
       </target>
       <description/>
    </proxy>
    ```

#### Step 2: Packaging all the artifacts[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#step-2-packaging-all-the-artifacts) <a href="#step-2-packaging-all-the-artifacts" id="step-2-packaging-all-the-artifacts"></a>

You need to package the new connector file and the proxy service separately.

1.  Create a **Connector Exporter project** and add the connector.

    See the instructions on [packaging a new connector file](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/adding-connectors/#packaging-connectors).
2.  Create a new **Composite Application project** and add the proxy service as well as the connector as dependencies.

    Tip

    Note that you need to add both the **Connector Exporter project** as well as the **ESB Config project** as dependencies because the connector is referred from the proxy service.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new-capp-project-with-depenencies.png)

    See the instructions on [packaging ESB artifacts](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/packaging-artifacts/#creating-a-new-composite-application).

#### Step 3: Deploying the artifacts[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#step-3-deploying-the-artifacts) <a href="#step-3-deploying-the-artifacts" id="step-3-deploying-the-artifacts"></a>

1.  Open the POM file for the composite application project and ensure that the **Connector Exporter** project as well as the **ESB Config** project are selected as dependencies.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/capp-dependencies.png)
2. Right-click the Composite Application project and click **Export Project Artifacts and Run**.

The embedded Micro Integrator will now start and deploy the artifacts.

#### Step 4: Testing the connector[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-new-connector/#step-4-testing-the-connector) <a href="#step-4-testing-the-connector" id="step-4-testing-the-connector"></a>

Post a request to the proxy service using Curl as shown below.

```bash
curl -v -X POST -d "{"searchQuery":"rabbit"}" -H "Content-Type: application/json"  http://localhost:8290/services/googlebooks_listVolume 
```

This performs a search and displays a list of volumes that meet the specified search criteria.
