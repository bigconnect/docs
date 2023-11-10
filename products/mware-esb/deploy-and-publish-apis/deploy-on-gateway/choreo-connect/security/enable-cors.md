# Enable CORS

#### Enable CORS configuration for API resources (API level)[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/enabling-cors/#enable-cors-configuration-for-api-resources-api-level) <a href="#enable-cors-configuration-for-api-resources-api-level" id="enable-cors-configuration-for-api-resources-api-level"></a>

If you are following the developer first approach, ([deploy the API via CLI tool](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/#choreo-connect-as-a-standalone-gateway/)). You can add CrossOrigin Resource Sharing (**CORS**) configurations for each API (at API level) using the OpenAPI vendor extension **x-wso2-cors** in the API definition. The following code snippet depicts the usage of the `x-wso2-cors` extension. For more information, see the [detailed sample OpenAPI definition with CORS level configuration](https://github.com/wso2/product-microgateway/blob/master/samples/cors\_sample.yaml).

```java
x-wso2-basePath: /petstore/v1
x-wso2-production-endpoints:
  urls:
  - https://petstore.swagger.io/v2
x-wso2-cors:
  accessControlAllowOrigins:
    - test.com
    - example.com
  accessControlAllowHeaders:
    - Authorization
    - Content-Type
  accessControlAllowMethods:
    - GET
    - PUT
    - POST
  accessControlAllowCredentials: true
```

If you are following the [Deploy API via API-M](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect/) approach, you can add **CORS** configurations for each API using the [WSO2 API-M](https://apim.docs.wso2.com/en/4.2.0/design/advanced-topics/enabling-cors-for-apis/#EnablingCORSPerAPI).

#### Enable CORS configurations globally[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/enabling-cors/#enable-cors-configurations-globally) <a href="#enable-cors-configurations-globally" id="enable-cors-configurations-globally"></a>

Follow the instructions below to enable CORS globally. Once this is enabled, it will apply this configurations through all endpoints and APIs deployed in Choreo Connect.

1. Open the Choreo Connect configuration file according to the deployment type you are using.
2.  Locate the following configuration set and make the `enabled` attribute to `true` with the required CORS attributes there.

    ```yml
    [router.cors]
        enabled = true
        allowOrigins = ["*"]
        allowMethods = ["GET","PUT","POST","DELETE","PATCH","OPTIONS"]
        allowHeaders = ["authorization","Access-Control-Allow-Origin","Content-Type","SOAPAction","apikey", "testKey", "Internal-Key"]
        exposeHeaders = []
        allowCredentials = false
    ```

\
