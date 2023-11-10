# Immutable Gateway

When deploying APIs as an immutable Gateway, the APIs are deployed during startup. After startup, no APIs can be added using apictl. For that, the apictl projects needs to be mounted to the `docker-compose/resources/adapter/artifacts/apis` directory (default location) of the adapter. At the startup of the adapter, it picks the projects and deploys them. After startup, APIs should be added using apictl.

For deploying APIs as an immutable Gateway using Helm chart, please refer [Deploy APIs as Immutable Gateway in Production Deployment](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/production-deployment-guideline/#deploy-apis-as-immutable-gateway).

### Step 1 - Download apictl and set the path variable[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-apis-as-immutable-gateway/#step-1-download-apictl-and-set-the-path-variable) <a href="#step-1-download-apictl-and-set-the-path-variable" id="step-1-download-apictl-and-set-the-path-variable"></a>

First download [apictl](https://github.com/wso2/product-apim-tooling/releases) locally and extract it into a folder of your choice. Then, add its location to your PATH variable.

```
export PATH=$PATH:<directory-of-the-apictl-executable>
```

### Step 2 - Create an API Project using apictl[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-apis-as-immutable-gateway/#step-2-create-an-api-project-using-apictl) <a href="#step-2-create-an-api-project-using-apictl" id="step-2-create-an-api-project-using-apictl"></a>

Let's create your first project "petstore" using an OpenAPI definition. The following `apictl init` command will generate a project folder containing all the necessary files.

Warning

If you have used a previous version of apictl before, remember to delete the directories `.wso2apictl` and `.wso2apictl.local` that are located in your home directory. Deleting them will make the newer apictl create them again with content compatible with the current version.

```bash
apictl init petstore --oas https://petstore.swagger.io/v2/swagger.json
```

Note

Instead of initializing the API project, you can [export the APIs from API Manager](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/api-controller/managing-apis-api-products/migrating-apis-to-different-environments) as well.

### Step 3 - Copy the project to the resources/enforcer/artifacts/apis directory[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-apis-as-immutable-gateway/#step-3-copy-the-project-to-the-resourcesenforcerartifactsapis-directory) <a href="#step-3-copy-the-project-to-the-resourcesenforcerartifactsapis-directory" id="step-3-copy-the-project-to-the-resourcesenforcerartifactsapis-directory"></a>

The created apictl project needs to be copied to `docker-compose/resources/adapter/artifacts/apis` directory. These apictl projects could be copied either in directory format or in zipped format. The apictl project can be the API artifact exported from the API Manager as well. The docker-compose file mounts that directory to the `/home/wso2/artifacts/apis`. All the apictl projects in the directory will be deployed during the startup.

### Step 4 - update config.toml[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-apis-as-immutable-gateway/#step-4-update-configtoml) <a href="#step-4-update-configtoml" id="step-4-update-configtoml"></a>

```
[adapter.server]
    enabled = false
```

Note

If this configuration is set to true, the mounted API projects are deployed during startup. However, they will not have the immutable nature as the APIs can be added after startup using apictl.

Note

If the control plane is enabled, the mounted API projects are not deployed.

### Step 5 - Invoke the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-apis-as-immutable-gateway/#step-5-invoke-the-api) <a href="#step-5-invoke-the-api" id="step-5-invoke-the-api"></a>

After the APIs are exposed via Choreo Connect, you can invoke an API with a valid access token.

Let's use the following command to generate a JWT to access the API, and set it to the variable `TOKEN`.

```
TOKEN=$(curl -X POST "https://localhost:9095/testkey" -d "scope=read:pets" -H "Authorization: Basic YWRtaW46YWRtaW4=" -k -v)
```

Execute the following cURL command to Invoke the API using the access token.Format

```bash
curl -X GET "https://<CHOREO-CONNECT_ROUTER_HOST>:<CHOREO-CONNECT_ROUTER_PORT>/<API-context>/<API-resource>" -H "accept:application/xml" -H "Authorization:Bearer $TOKEN" -k
```

