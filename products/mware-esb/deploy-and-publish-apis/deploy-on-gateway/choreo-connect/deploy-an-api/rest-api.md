# REST API

You can deploy a REST API in the following ways depending on the Choreo Connect **mode** you have chosen.

### Via the API Manager Publisher Portal <a href="#via-wso2-api-manager-publisher-portal" id="via-wso2-api-manager-publisher-portal"></a>

Follow the instructions below to deploy a REST API in a Choreo Connect instance run with WSO2 API Manager as the Control Plane.

#### Step 1 - Create an API in API Manager[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-1-create-an-api-in-api-manager) <a href="#step-1-create-an-api-in-api-manager" id="step-1-create-an-api-in-api-manager"></a>

Create a REST type API using one of the following methods:

* [Create a REST API via the WSO2 API Manager Publsiher Portal](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api/)
*   [Import a REST API to WSO2 API Manager using WSO2 API Controller (apiclt)](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/api-controller/managing-apis-api-products/importing-apis-via-dev-first-approach/)

    You need to initially initialize the API project and thereafter import the API project in order to be able to import the API to WSO2 API Manager using the apiclt.

For this example, let's create an API via the WSO2 API Manager Publsiher Portal.

#### Step 2 - Deploy the API in API Manager[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-2-deploy-the-api-in-api-manager) <a href="#step-2-deploy-the-api-in-api-manager" id="step-2-deploy-the-api-in-api-manager"></a>

The guide [here](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/deploy-api/deploy-an-api) will explain how you could easily deploy the API you just created.

That's it! To invoke the API follow the steps [here](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-4-invoke-the-api).

Info

During the startup, Choreo Connect will check the `config.toml` to see if the `controlPlane` configuration has been enabled. If so, it will start fetching all the necessary artifacts that belong to the Gateway environment given in `environmentLabels`. These artifacts include deployed APIs, Applications, Subscriptions, Policies, information related to Key Managers, etc.

Whenever a new event occurs in API Manager such as an API being deployed, API Manager will notify Choreo Connect via Event Hub. Choreo Connect will then start fetching all the new artifacts related to its environment.

Tip

To be able to invoke an API via the Developer Portal TryOut Console, make sure at least one of the certificates used by the enforcer is same as the certificate used by the Key Manager configured in API-M. In Choreo Connect, the certs for enforcer are located at `<CHOREO-CONNECT_HOME>/docker-compose/resources/enforcer/security/truststore`. In API-M, Key Managers can be configured from the API-M Admin Portal.

#### Step 3 - Invoke the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-3-invoke-the-api) <a href="#step-3-invoke-the-api" id="step-3-invoke-the-api"></a>

After the APIs are exposed via Choreo Connect, you can invoke an API with a valid access token.

Let's use the following command to generate a JWT to access the API, and set it to the variable `TOKEN`.

```
TOKEN=$(curl -X POST "https://localhost:9095/testkey" -d "scope=read:pets" -H "Authorization: Basic YWRtaW46YWRtaW4=" -k -v)
```

Execute the following cURL command to Invoke the API using the access token.Format

```bash
curl -X GET "https://<CHOREO-CONNECT_ROUTER_HOST>:<CHOREO-CONNECT_ROUTER_PORT>/<API-context>/<API-resource>" -H "accept:application/xml" -H "Authorization:Bearer $TOKEN" -k
```

Example

Refer to [Generate a Test JWT](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/generate-a-test-jwt) for more details.

### Via apictl for Standalone Mode[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#via-apictl-for-standalone-mode) <a href="#via-apictl-for-standalone-mode" id="via-apictl-for-standalone-mode"></a>

Follow the instructions below to deploy a REST API via WSO2 API Controller (apictl), which is a CLI Tool used when Choreo Connect is run as a Standalone Gateway.

Before you begin

This guide assumes that you already have a Choreo Connect instance that is up and running. If not, checkout the [Using Choreo Connect Deployed on Docker with WSO2 API Controller Guide](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/getting-started/deploy/cc-as-a-standalone-gateway-on-docker) on how to install and run Choreo Connect. To learn more about Choreo Connect, have a look at the [Overview of Choreo Connect](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/getting-started/choreo-connect-overview).

#### Step 1 - Download apictl and Set the Path Variable[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-1-download-apictl-and-set-the-path-variable) <a href="#step-1-download-apictl-and-set-the-path-variable" id="step-1-download-apictl-and-set-the-path-variable"></a>

First download [apictl](https://github.com/wso2/product-apim-tooling/releases) locally and extract it into a folder of your choice. Then, add its location to your PATH variable.

```
export PATH=$PATH:<directory-of-the-apictl-executable>
```

#### Step 2 - Create an API Project using apictl[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-2-create-an-api-project-using-apictl) <a href="#step-2-create-an-api-project-using-apictl" id="step-2-create-an-api-project-using-apictl"></a>

Let's create your first project "petstore" using an OpenAPI definition. The following `apictl init` command will generate a project folder containing all the necessary files.

Warning

If you have used a previous version of apictl before, remember to delete the directories `.wso2apictl` and `.wso2apictl.local` that are located in your home directory. Deleting them will make the newer apictl create them again, with content compatible with the current version.

```bash
apictl init petstore --oas https://petstore.swagger.io/v2/swagger.json
```

#### Step 3 - Add a Choreo Connect Environment to apictl[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-3-add-a-choreo-connect-environment-to-apictl) <a href="#step-3-add-a-choreo-connect-environment-to-apictl" id="step-3-add-a-choreo-connect-environment-to-apictl"></a>

To use apictl for Choreo Connect, a Choreo Connect environment needs to be added to apictl. This environment will hold the adapter URL for further commands.

```bash
apictl mg add env dev --adapter https://localhost:9843
```

Info

Note `mg` in the above command. The apictl commands that starts as `apictl mg` are Choreo Connect specific. If a command does not have `mg` after `apictl` then the command could probably be common to both Choreo Connect and API Manager, but it could also be API Manager specific.

Tip

The apictl commands here onwards are executed with the -k flag to avoid SSL verification with the Choreo Connect.

To communicate via HTTPS without skipping SSL verification (without -k flag), add the cert in the Choreo Connect truststore into the `<USER_HOME>/.wso2apictl/certs` folder.

#### Step 4 - Log in to the Choreo Connect Environment in apictl[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-4-log-in-to-the-choreo-connect-environment-in-apictl) <a href="#step-4-log-in-to-the-choreo-connect-environment-in-apictl" id="step-4-log-in-to-the-choreo-connect-environment-in-apictl"></a>

You can use the following command to log in to the above Choreo Connect cluster (in other words log in to the Choreo Connect adapter). When you log in, an access token will be retrieved from Choreo Connect and saved in apictl.

```bash
apictl mg login dev -k
```

or

```bash
apictl mg login dev -u admin -p admin -k
```

#### Step 5 - Deploy the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-5-deploy-the-api) <a href="#step-5-deploy-the-api" id="step-5-deploy-the-api"></a>

Now let's deploy the API to Choreo Connect by executing the following command.

```bash
apictl mg deploy api -f <path_to_the_API_project_just_created>/petstore -e dev -k
```

#### Step 6 - Invoke the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#step-6-invoke-the-api) <a href="#step-6-invoke-the-api" id="step-6-invoke-the-api"></a>

After the APIs are exposed via Choreo Connect, you can invoke an API with a valid access token.

Let's use the following command to generate a JWT to access the API, and set it to the variable `TOKEN`.

```
TOKEN=$(curl -X POST "https://localhost:9095/testkey" -d "scope=read:pets" -H "Authorization: Basic YWRtaW46YWRtaW4=" -k -v)
```

Execute the following cURL command to Invoke the API using the access token.Format

```bash
curl -X GET "https://<CHOREO-CONNECT_ROUTER_HOST>:<CHOREO-CONNECT_ROUTER_PORT>/<API-context>/<API-resource>" -H "accept:application/xml" -H "Authorization:Bearer $TOKEN" -k
```
