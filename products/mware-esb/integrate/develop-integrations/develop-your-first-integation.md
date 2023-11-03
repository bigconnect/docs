# Develop Your First Integation

tegration developers need efficient tools to build and test all the integration use cases required by the enterprise before pushing them into a production environment. The following topics will guide you through the process of building and running an example integration use case using WSO2 Integration Studio. This tool contains an embedded WSO2 Micro Integrator instance as well as other capabilities that allows you to conveniently design, develop, and test your integration artifacts before deploying them in your production environment.

### What you'll build[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#what-youll-build) <a href="#what-youll-build" id="what-youll-build"></a>

We are going to use the same use case we considered in the [Quick Start Guide](https://apim.docs.wso2.com/en/4.2.0/get-started/integration-quick-start-guide). In the quick start guide, we just executed the already-built integration scenario. Here, we are going to build the integration scenario from scratch. Let’s recall the business scenario:

![Integration Scenario](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/dev-first-integration-0.png)

The scenario is about a basic healthcare system where WSO2 Micro Integrator is used as the integration middleware. Most healthcare centers use a system to help patients book doctor appointments. To check the availability of doctors, patients will typically use every online system that is dedicated to a particular healthcare center or personally visit the healthcare centers.

We will simplify this process of booking doctor appointments by building an integration solution that orchestrates the isolated systems in each healthcare provider and exposes a single interface to the users.

Both the Grand Oak service and Pine Valley service are exposed over the HTTP protocol.

*   The Grand Oak service accept GET requests in the following service endpoint URL:

    ```bash
    http://<HOST_NAME>:<PORT>/grandOak/doctors/<DOCTOR_TYPE>
    ```
*   The Pine Valley service accepts POST requests in the following service endpoint URL:

    ```bash
    http://<HOST_NAME>:<PORT>/pineValley/doctors
    ```

    The expected payload should be in the following JSON format:

    ```bash
    {
            "doctorType": "<DOCTOR_TYPE>"
    }
    ```

Let’s implement a simple Rest API that can be used to query the availability of doctors for a particular category from all the available healthcare centers.

### Step 1 - Set up the workspace[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#step-1-set-up-the-workspace) <a href="#step-1-set-up-the-workspace" id="step-1-set-up-the-workspace"></a>

Download the relevant [WSO2 Integration Studio](https://wso2.com/integration/integration-studio/) based on your operating system. For more information, see [Installing WSO2 Integration Studio](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/installing-wso2-integration-studio).

### Step 2 - Develop the integration artifacts[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#step-2-develop-the-integration-artifacts) <a href="#step-2-develop-the-integration-artifacts" id="step-2-develop-the-integration-artifacts"></a>

#### Create the integration project[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#create-the-integration-project) <a href="#create-the-integration-project" id="create-the-integration-project"></a>

Let's create an integration project with the required modules (to store artifacts) in WSO2 Integration Studio.

1. Open WSO2 Integration Studio and click **New Integration Project** in the **Getting Started** view as shown below. ![New Integration Project](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/new-project/new-integration-project.png)
2.  In the **New Integration Project** dialog box that opens, enter `Healthcare` as the project name. This is a maven multi module project.

    Be sure to leave the Create ESB Configs and Create Composite Exporter check boxes selected as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/2-dev-get-started-integration-proj.png)
3. Click **Finish**.

The integration project with the ESB Config module (`HealthcareConfigs`) and Composite Exporter module (`HealthcareCompositeExporter`) are created as shown below.

[![project folder](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/3-dev-get-started-proj-explorer.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/3-dev-get-started-proj-explorer.png)

#### Create Endpoints[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#create-endpoints) <a href="#create-endpoints" id="create-endpoints"></a>

The actual back-end services (healthcare services) are logically represented in the integration solution as **Endpoint** artifacts.

Let's create two Endpoint artifacts for the two healthcare services:

1.  Right-click `HealthcareConfigs` and go to **New** → **Endpoint** to open the **New Endpoint Artifact** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/4-dev-get-started-select-endpoint.jpg)
2. In the New Endpoint Artifact dialog box that opens, select **Create a New Endpoint** and click **Next**.
3.  For the ‘Grand Oak hospital service’, let’s use the following values:

    | Parameter     | Value                                                       |
    | ------------- | ----------------------------------------------------------- |
    | Endpoint Name | GrandOakEndpoint                                            |
    | Endpoint Type | HTTP Endpoint                                               |
    | URI Template  | http://localhost:9090/grandOak/doctors/{uri.var.doctorType} |
    | Method        | GET                                                         |

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/5-dev-get-started-grandoaks-endpoint.png)
4. Click **Finish** to save the endpoint configuration.
5.  Follow the same steps to create an endpoint for ‘Pine Valley Hospital’. Use the following parameter values:

    | Parameter     | Value                                    |
    | ------------- | ---------------------------------------- |
    | Endpoint Name | PineValleyEndpoint                       |
    | Endpoint Type | HTTP Endpoint                            |
    | URI Template  | http://localhost:9091/pineValley/doctors |
    | Method        | POST                                     |

#### Create the REST API[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#create-the-rest-api) <a href="#create-the-rest-api" id="create-the-rest-api"></a>

We are orchestrating multiple services and exposing a single API to the clients. The main integration artifact is going to be a REST API.

1. Right-click `HealthcareConfigs` in the project explorer and go to **New** → **REST API** to open the **API Artifact Creation Options** dialog box.
2. Select **Create A New API Artifact** and click **Next**.
3.  Specify values for the required REST API properties:

    | Parameter | Value         |
    | --------- | ------------- |
    | Name      | HealthcareAPI |
    | Context   | /healthcare   |

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/6-dev-get-started-rest-api.png)
4. Click **Finish**. The REST API is created in the `src/main/synapse-config/api` folder under `HealthcareConfigs`.
5.  Open the new artifact from the project explorer. You will see the graphical view of the `HealthcareAPI` with its default **API Resource**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/7-dev-get-started-api-canvas.png)

    To the right of the editor, you will see the **Mediators** palette containing various mediators that can be dragged and dropped into the canvas of the **API Resource**.
6.  Double-click the API resource to open the **Properties** view:

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/8-dev-get-started-api-properties-view.png)

    Specify values for the required resource properties:

    | Parameter    | Value                                                                                                                                                                                                                                                                                  |
    | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Url Style    | URL\_TEMPLATE                                                                                                                                                                                                                                                                          |
    | Uri Template | <p><code>/doctor/{doctorType}</code><br><br>Note that '{doctorType}' is a uri variable that gets resolved to the path parameter value in the runtime. We can access the value of the uri variable in the mediation flow using the variable (property) called ‘uri.var.doctorType’.</p> |
    | Methods      | Get                                                                                                                                                                                                                                                                                    |

#### Create the mediation logic[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#create-the-mediation-logic) <a href="#create-the-mediation-logic" id="create-the-mediation-logic"></a>

1.  Create two parallel message flows:

    In this scenario, the Healthcare API receives an HTTP GET request, which should be delivered to two different back-end services. That is, we need to clone the message into two branches and process them in parallel. To do that, we can use the **Clone Mediator**.

    Drag the **Clone** mediator from the mediator palette and drop it into the request path (in sequence) of the API Resource canvas.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/9-dev-get-started-clone-mediator.png)

    Right-click the Clone mediator and select **Add/Remove Target..**. In the **Add Target Branches** window, set the number of branches to 2. You will now see two branches inside the **Clone** mediator.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/10-dev-get-started-clone-mediator-branches.png)
2.  Invoke the GrandOak Endpoint:

    The **Call** mediator is used to invoke a back-end service. In [Step 2](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#step-2-create-endpoints), we have already created an Endpoint to represent the GrandOak endpoint.

    Drag the Call mediator from the mediator palette into one branch of the Clone mediator.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/11-dev-get-started-call-mediator.png)

    Then, drag the already-defined GrandOak endpoint artifact, which is available under the **Defined Endpoints** section of the palette, into the Call mediator.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/12-dev-get-started-call-grandoaks.png)
3.  Construct message payload for the PineValley Endpoint:

    Unlike the GrandOAK endpoint, which accepts a simple GET request, the PineValley endpoint requires a POST request with the following JSON message:

    ```bash
    {
        "doctorType": "<DOCTOR_TYPE>"
    }
    ```

    Therefore, we need to first construct the required message payload. There are several Transformation mediators for constructing messages. Let's use the **PayloadFactory** mediator. Drag the PayloadFactory mediator into the 2nd branch of the **Clone** mediator as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/13-dev-get-started-payloadfactory-mediator.png)

    Specify values for the required PayloadFactory properties:

    | Parameter      | Value                   |
    | -------------- | ----------------------- |
    | Payload Format | Inline                  |
    | Media Type     | json                    |
    | Payload        | { "doctorType": "$1" }  |
    | Args           | $ctx:uri.var.doctorType |

    Note the `$1` in the Payload format. It denotes a parameter that can get a value assigned dynamically. The value for the parameters needs to be assigned using Arguments **(Args)**. **Args** can be added using the **PayloadFactoryArgument** dialog box, which appears when you click the (![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/common/plus-icon.png)) sign.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/14-dev-get-started-payloadfactory-expression.jpg)

    In the `PayloadFactoryArgument` dialog box, select **Expression** as the **Argument Type**, and click **Argument Expression**. You will then see the **Expression Selector** dialog box. Enter **$ctx:uri.var.doctorType** as the value for the expression.
4.  Invoke the PineValley Endpoint:

    Use the Call mediator to invoke the PineVallery Endpoint. Follow the same steps you used under ‘Invoke GrandOak Endpoint’.
5.  Aggregating response messages:

    Since we are cloning the messages and delivering them into two different services, we will receive two responses. So we need to aggregate those two responses and construct a single response. To do that, we can use the **Aggregate** mediator.

    Drag the Aggregate mediator and drop it next to the Clone mediator as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/15-dev-get-started-aggregate-mediator.jpg)

    Specify values for the required Aggregate mediator properties.

    | Parameter              | Value                       |
    | ---------------------- | --------------------------- |
    | Aggregation Expression | json-eval($.doctors.doctor) |
6.  Send a response back to the client :

    To send the response back to the client, we can use the **Respond** mediator. Add the Respond mediator as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/16-dev-get-started-drop-mediator.jpg)

The final mediation configuration looks similar to the above diagram.\
Following is what you will see in the **Source View** of WSO2 Integration Studio.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<api context="/healthcare" name="HealthcareAPI" xmlns="http://ws.apache.org/ns/synapse">
<resource methods="GET" uri-template="/doctor/{doctorType}">
    <inSequence>
  <clone>
      <target>
    <sequence>
        <call>
      <endpoint key="GrandOakEndpoint"/>
        </call>
    </sequence>
      </target>
      <target>
    <sequence>
        <payloadFactory media-type="json">
      <format>{
            "doctorType": "$1"
               }
      </format>
      <args>
          <arg evaluator="xml" expression="$ctx:uri.var.doctorType"/>
      </args>
        </payloadFactory>
        <call>
      <endpoint key="PineValleyEndpoint"/>
        </call>
    </sequence>
      </target>
  </clone>
  <aggregate>
      <completeCondition>
    <messageCount max="-1" min="-1"/>
      </completeCondition>
      <onComplete expression="json-eval($.doctors.doctor)">
    <respond/>
      </onComplete>
  </aggregate>
    </inSequence>
    <outSequence/>
    <faultSequence/>
</resource>
</api>
```

### Step 3 - Build and run the artifacts[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#step-3-build-and-run-the-artifacts) <a href="#step-3-build-and-run-the-artifacts" id="step-3-build-and-run-the-artifacts"></a>

There are several ways to deploy and run the integration scenario.

#### Option 1: Using WSO2 Integration Studio[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#option-1-using-wso2-integration-studio) <a href="#option-1-using-wso2-integration-studio" id="option-1-using-wso2-integration-studio"></a>

1.  Right-click `HealthcareCompositeExporter` and click **Export Project Artifacts and Run**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/17-dev-get-started-build-and-run-1.jpg)
2.  You will see the following dialog box. Select the `HealthcareConfigs` folder in the artifact list and click **Finish**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/18-dev-get-started-build-and-run-2.png)

The embedded Micro Integrator starts with the deployed artifacts. You will see the server startup log in the Console tab, and the endpoints of the deployed services in the Runtime Services tab as shown below.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/19-dev-get-started-embedded-server-startup.jpg)

#### Option 2: Using a local Micro Integrator instance[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#option-2-using-a-local-micro-integrator-instance) <a href="#option-2-using-a-local-micro-integrator-instance" id="option-2-using-a-local-micro-integrator-instance"></a>

**Before you begin**, be sure to install the Micro Integrator on your machine:

1. Go to the [WSO2 Micro Integrator web page](https://wso2.com/integration/micro-integrator/), click **Download**, and then click **Zip Archive** to download the Micro Integrator distribution as a ZIP file.
2. Extract the ZIP file. This will be the `<MI_HOME>` folder.

Once you have downloaded and set up the Micro Integrator locally, follow the steps given below.

1. **Export the artifacts as a deployable CAR file**: Right-click `HealthcareCompositeExporter` in WSO2 Integration Studio and select **Export Composite Application Project**.
2. **Deploy the Healthcare service**: Copy the exported CAR file of the Healthcare service to the `MI_HOME/repository/deployment/server/carbonapps` directory.
3. **Start the Micro Integrator**:
   1. Open a terminal and navigate to the `<MI_HOME>/bin` folder.
   2.  Execute one of the commands given below.

       On MacOS/Linux

       ```bash
       ./micro-integrator.sh
       ```

       On Windows

### Step 4 - Observe deployed artifacts[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#step-4-observe-deployed-artifacts) <a href="#step-4-observe-deployed-artifacts" id="step-4-observe-deployed-artifacts"></a>

Once you have deployed the artifacts and started the Micro Integrator server, you can [install](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/install/installing-the-product/installing-mi-dashboard) and [start the Dashboard](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/install/installing-the-product/running-the-mi-dashboard) to observe details of the deployed artifacts.

If you are running the embedded Micro Integrator, click Open Monitoring Dashboard in the Runtime Services tab as shown below.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/20-dev-get-started-open-dashboard.jpg)

You will be directed to the sign-in screen of the Dashboard from your default browser as shown below. Sign in using `admin` as the user name and password.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/21-dev-get-started-dashboard-login.png)

Once you sign in, click the required artifact type to view details.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/developing-first-integration/22-dev-get-started-dashboard-artifact-view.png)

### Step 5 - Test the use case[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#step-5-test-the-use-case) <a href="#step-5-test-the-use-case" id="step-5-test-the-use-case"></a>

Now, let's test the integration service.

#### Start back-end services[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#start-back-end-services) <a href="#start-back-end-services" id="start-back-end-services"></a>

Let's start the mock back-end services for this use case:

1. Download the [`DoctorInfo-JDK11.jar` file](https://apim.docs.wso2.com/en/4.2.0/assets/attachments/developing-first-integration/doctorinfo-jdk11.jar). This contains two healthcare services.
2.  Open a terminal, navigate to the location of the downloaded `DoctorInfo-JDK11.jar` file, and execute the following command to start the services:

    ```bash
    java -jar DoctorInfo-JDK11.jar
    ```

#### Invoke the Healthcare service[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/integration-development-kickstart/#invoke-the-healthcare-service) <a href="#invoke-the-healthcare-service" id="invoke-the-healthcare-service"></a>

There are two ways to invoke the service:

*   **Option 1: Using Postman**

    Let's invoke the API from Postman as follows:

    1. Open the Postman application. If you do not have the application, download it from here : [Postman](https://www.postman.com/downloads/)
    2. Create a collection with appropriate name. Ex : 'IntegrationStudio collection'.
    3. Add a new request to this collection and name it appropriately. Ex: 'Healthcare request'.
    4. In the 'Enter request URL' section paste our endpoint URL : `http://localhost:8290/healthcare/doctor/Ophthalmologist`
    5. Select 'GET' as http method and click 'Send' button.\
       \
       \
       \

*   **Option 2: Using your terminal**

    If you want to send the client request from your terminal:

    1. Install and set up [cURL](https://curl.haxx.se/) as your REST client.
    2.  Open a terminal and execute the following curl command to invoke the service:

        ```bash
        curl -v http://localhost:8290/healthcare/doctor/Ophthalmologist
        ```

    You will receive the following response:

    ```bash
    [
        [
            {
                "name": "John Mathew",
                "time": "03:30 PM",
                "hospital": "Grand Oak"
            },
            {
                "name": "Allan Silvester",
                "time": "04:30 PM",
                "hospital": "Grand Oak"
            }
        ],
        [
            {
                "name": "John Mathew",
                "time": "07:30 AM",
                "hospital": "pineValley"
            },
            {
                "name": "Roma Katherine",
                "time": "04:30 PM",
                "hospital": "pineValley"
            }
        ]
    ]
    ```
