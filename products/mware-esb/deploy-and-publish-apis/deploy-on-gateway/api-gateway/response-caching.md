# Response Caching

he API Manager uses [WSO2 ESB's cache mediator](https://wso2docs.atlassian.net/wiki/spaces/EI650/pages/35554177/Cache+Mediator) to cache response messages for each API. Caching improves performance, because the backend server does not have to process the same data for a request multiple times. You need to set an appropriate timeout period to offset the risk of stale data in the cache.

### Enabling Response Caching for an API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/response-caching/#enabling-response-caching-for-an-api) <a href="#enabling-response-caching-for-an-api" id="enabling-response-caching-for-an-api"></a>

You need to enable response caching when creating a new API or editing an existing API using the API Publisher.

Please follow below steps to enable response caching for an API.

1. [Create an API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api/).
2.  Navigate to the **Runtime** tab under **API Configurations** where you find the response caching configuration. Then turn on Response caching and give a timeout value. This enables the default response caching settings.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/enable-response-caching.png)

    Note

    When creating a new API by using a Swagger or Open API definition, response caching can be set up by defining an API-M supported Open API extension **“x-wso2-response-cache”**.

    Example

    ```yaml
     x-wso2-response-cache: 
        enabled: true
        cacheTimeoutInSeconds: 400
    ```
3.  If you want to change the default response caching settings, edit the following cache mediator properties in the `<API-M_HOME>/repository/resources/api_templates/velocity_template.xml` file:

    | Property          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | `collector`       | <ul><li><code>true</code> : specifies that the mediator instance is a response collection instance.</li><li><code>false</code> : specifies that the mediator instance is a cache serving instance.<br></li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | `max MessageSize` | Specifies the maximum size of a message to be cached in bytes. An optional attribute, with the default value set to `unlimited` .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | `maxSize`         | Defines the maximum number of elements to be cached.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | `hashGenerator`   | <p>Defines the hash generator class.</p><p>When caching response messages, a hash value is generated based on the request's URI, transport headers and the payload (if available). WSO2 has a default <code>HttpRequestHashGenerator</code> class written to generate the hash value. See sample <a href="https://apim.docs.wso2.com/en/4.2.0/assets/attachments/103333424/103333429.java">here</a> . Please note that prior to API-M 4.0.0, a different hash generator implementation <code>REQUESTHASHGenerator</code> was used by default and from API-M 4.0.0, this new hash generator implementation is being used.</p><p>If you want to change this default implementation (for example, to exclude certain headers), you can write a new hash generator implementation by extending the <code>HttpRequestHashGenerator</code> and overriding its <code>getDigest()</code> method. Once done, add the new class as the <code>hashGenerator</code> attribute of the <code>&#x3C;cache></code> element in the <code>velocity_template.xml</code> file and <code>api_product_template.xml</code> file.</p> |

Note

When running a distributed deployment, you need to enable the JSON stream builders on the API Gateway and maintain the standard builders on the API Developer Portal node.

Follow the instructions below to enable the stream builders in the API gateway:

1. Open the `<API-M_HOME>/repository/conf/deployment.toml` file.
2.  Add following configuration.

    ```java
        [message_builder]
        json = "org.apache.synapse.commons.json.JsonStreamBuilder"
    ```

### Invalidating Cached Responses Remotely[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/response-caching/#invalidating-cached-responses-remotely) <a href="#invalidating-cached-responses-remotely" id="invalidating-cached-responses-remotely"></a>

You can invalidate all cached response remotely by using any [JMX monitoring tool such as Jconsole](https://ei.docs.wso2.com/en/latest/micro-integrator/administer-and-observe/jmx\_monitoring) using the exposed MBeans. You can use the `InvalidateMediatorCache()` operation of the `org.wso2.carbon.mediation` MBean for this as shown below.

![JMX monitoring through JConsole](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/jmx-monitoring-through-jsoncole.png)
