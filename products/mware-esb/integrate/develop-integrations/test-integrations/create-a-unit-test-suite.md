# Create a Unit Test Suite

Once you have developed an integration solution, WSO2 Integration Studio allows you to build unit tests for the following:

* Test [mediation sequences](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/creating-reusable-sequences), [proxy services](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/creating-a-proxy-service), and [REST apis](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/creating-an-api) with multiple test cases
* Test the artifacts with [registry resources](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/creating-registry-resources).
*   Test the artifacts with [Connectors](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/adding-connectors).

    Note

    [Scheduled Tasks](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/creating-scheduled-task) are not supported by the Unit Testing framework.

### Create Unit Test Suite[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-unit-test-suite/#create-unit-test-suite) <a href="#create-unit-test-suite" id="create-unit-test-suite"></a>

1. Open WSO2 Integration Studio.
2. Open an existing project with your integration solution.
3.  Right-click the **test** folder, which is parallel to the **src** folder and go to **New** -> **Unit Test Suite** as shown below.

    ![Create Unit Test Suite](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/create-test-suite.png)

    The **New Unit Test Suite** wizard opens.
4.  Select **Create a New Unit Test Suite** and click **Next**.

    ![Select Create Method](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/select-create-method.png)
5.  Specify a name for the unit test suite. Then, select the artifact file that you want to test from the file list and click **Next**.

    Note

    You can only select one sequence, proxy service, or API artifact per unit test suite.

    ![Fill Unit Test Suite Details](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/select-main-artifact.png)
6.  Select the supporting artifacts from the list as shown below and click **Next**.

    ![Select Supportive Artifacts](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/select-supportives.png)
7.  You can use a mock service to simulate the actual endpoint. If you have an already created Mock Service, select the mock service files from the list as shown below. You can also [create a new Mock Service](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-unit-test-suite/#create-mock-service) for this purpose.

    ![Select Mock Services](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/select-mock-services.png)
8. Click **Finish**.

### Update the Unit Test Suite[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-unit-test-suite/#update-the-unit-test-suite) <a href="#update-the-unit-test-suite" id="update-the-unit-test-suite"></a>

Once you have created a Unit Test Suite in WSO2 Integration Studio, you can find it inside the test folder. You can update the Unit Test Suite by adding test cases and changing the supporting artifacts and mock-services.

1.  Open Unit Test Suite from the project explorer. You can use either the design view or the source view to update the unit test suite.

    ![Unit Test Form](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/unit-test-form.png)
2.  In design view, click the '+' button under the **Test Artifact, Test Cases and Assertion Details** section to add a new **test case** to the unit test suite.

    ![Add Test Case](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/add-test-case.png)
3. Enter the following information:
   1. Enter a name for the test case.
   2.  Update the **Input Payload and Properties** section:

       * **Input Payload**: The input payload of the test case. This can be **JSON**, **XML**, or **plain text**.
       * **Input properties**: The input properties of the test case. There are three types of properties allowed in unit testing: **Synapse(ctx), Axis2(axis2)**, and **Transport($trp)** properties.

       For sequences, the test suite allows to add all type of properties with the value. For APIs and proxy services, you are only allowed to add transport properties.

       Note

       For APIs, you also need to specify the **Request Path** and **Request Method** in the this section. The **Request Path** indicates the URL mapping of the API resource. If the URL mapping consists some parameter(s), replace those with values. Also the **Request Method** indicates the REST method of the resource.
   3.  In the **Assertions** section, you can add multiple assertion belonging to two types: **AssertEquals** check the whether the mediated result and expected values are equal. **AssertNotNull** checks whether the mediated result is not null.

       ![Add Assertions](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/add-assertion.png)

       * **Assertion Type**: Type of the assertion.
       * **Actual Expression**: Expression that you want to assert.
         * **$body**: assert the payload\

         * **$ctx:**: assert synapse property
         * **$axis2:**: assert axis2 property
         * **$trp:**: assert transport property
         * **$statusCode**: assert status code of the service
         * **$httpVersion**: assert http version of the service
       * **Expected Value**: Expected value for the actual expression. Type can be a **JSON**, **XML** or a **plain text**.
       * **Error Message**: Error message to print when the assertion is failed.
         1. Once you have added at least one assertion, click **Add**.
       * Save the unit test suite.

### Run Unit Test Suites[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-unit-test-suite/#run-unit-test-suites) <a href="#run-unit-test-suites" id="run-unit-test-suites"></a>

Run the Unit Test Suite(s) in the unit testing server of the embedded Micro Integrator or a remote unit testing server.

1.  Right-click the **test** directory and click **Run Unit Test** to run all the unit test suites at once. Alternatively, right-click the particular unit test suite and click **Run Unit Test** to run a selected unit test suite.

    ![Run Unit Test Suite](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/run-test.png)

    The **Unit Test Run Configuration** wizard opens.
2.  Select the specific unit testing server (embedded server or remote server) to run the tests.

    ![Run Configuration](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/run-configuration.png)

    **Local Server Configuration**

    If you select this option, you are running the tests in the unit test server of the embedded Micro Integrator. Specify the following details:

    * **Executable Path**: Path to the unit testing server.
    * **Server Test Port**: Port of the unit testing server.

    **Remote Server Configuration**

    Note

    **Before you begin** Be sure that your remote Micro Integrator is started along with its Unit Testing server. Note that you need to pass the `-DsynapseTest` property with your product startup script as shown below. This property is required for starting the Unit Testing server.

    On MacOS/Linux/CentOS

    ```bash
    sh micro-integrator.sh -DsynapseTest
    ```

    On Windows

    To change starting port of the unit testing server, you can use `-DsynapseTestPort=<PORT>` system property with above command. The default port is 9008.

    If you select this option, you are running the tests in the unit testing server of a remote Micro Integrator. Specify the following details:

    * **Server Remote Host**: Host IP of the remote unit testing server. This is the host on which the remote Micro Integrator is running.
    * **Server Test Port**: Port of the remote unit testing server. The default port is 9008.
3.  Click **Run** to start the unit test. It will start the unit testing server in the console and prints the summary report for the given unit test suite(s) using the response from the unit testing server.

    ![Output Console](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/console-log.png)

### Create Mock Service[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-unit-test-suite/#create-mock-service) <a href="#create-mock-service" id="create-mock-service"></a>

Mock services give the opportunity to simulate the actual endpoint.

1. Open an existing project that has your integration solution.
2.  Right-click the **test** folder parallel to the **src** folder, and go to **New** -> **Mock Service** as shown below.

    ![Create Mock Service](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/create-mock.png)
3.  Select **Create a New Mock Service** and click **Next**.

    ![Select Create Mock Service Method](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/select-mock-method.png)
4.  In the **Create a new Mock Service** page, enter the following details:

    ![Mock Service Details](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/mock-details.png)

    * **Name of the Mock Service**: A name for the mock service.
    * **Mocking Endpoint Name**: Endpoint name which wants to mock.
    * **Mock Service Port**: Port for the mock service.
    * **Mock Service Context**: Main context for the service starts with '/'.
5. Add multiple resources for the mock service as needed. To add multiple resources click the '+' icon on top of the resources table.
   * **Service Sub Context**: Sub context of the resource starts with '/'.
   * **Service Method**: REST method type of the resource.
6.  Fill the **Expected Request to the Mock Service Resource** section if you want to mock an endpoint based on the coming request headers or payload.

    ![Mock Service Resource Request Details](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/resource-request.png)

    * **Header Name**: Expected request header name.
    * **Header Value**: Expected request header value.
    * **Expected Request Payload**: Expected request payload to the service.

    **Note**: Entered request headers/payload must me matched with the request to send the response a this mock service.
7.  Fill the **Response Send Out from the Mock Service Resource** section to get a response from the service.

    ![Mock Service Resource Response Details](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/resource-response.png)

    * **Response Status Code**: Response status code of the mock service.
    * **Header Name**: Response request header name.
    * **Header Value**: Response request header value.
    * **Send Out Response Payload**: Expected response payload from the service.

    Note

    Please note that the mock service **should have a sub context** with '/' defined, and additional sub contexts should be defined after that.

    * If you are trying to mock an endpoint `http://petstore.com/pets`, the wizard should look like below now.

    ![Mock Service with one sub context](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/mockservice-context-sample.png)

    * If you are going to mock the same endpoint with an additional sub context (e.g., `http://petstore.com/pets/id` ), you can add it to the same mock service as shown below.

    ![Mock Service with additional sub Contexts](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/mockservice-subcontext-sample.png)

Once you have entered the required details, click **Add**. It will list the resource under the **Add Service Resource** table with **Sub Context** and **Method**. After that click **Finish** to create a Mock Service. It will locate under the **test → resources → mock-services** directory.

![Mock Service Form](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/mock-service-form.png)

### Debug the Unit Test Suite[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-unit-test-suite/#debug-the-unit-test-suite) <a href="#debug-the-unit-test-suite" id="debug-the-unit-test-suite"></a>

If you encounter errors with the unit testing framework, you can debug the framework for troubleshooting as follows:

1. Select the WSO2 Integration Studio and go to **Run -> Run Configurations..** to open the Run Configurations dialog box.
2. Expand Maven Build and click Run Unit Test Internal Configuration.
3.  In the Goals field, add `-X` to the end of the command as shown below.

    Tip

    This enables maven debug for the testing framework.

    ![troubleshooting unit testing](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/synapse\_unit\_test/debug-unit-testing.png)
4. Click Apply and then click Run.
5. Return to the Console and see that the unit tests are running on debug mode.
