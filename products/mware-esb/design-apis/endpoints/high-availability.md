# High Availability

## Configuring Load Balancing Endpoints <a href="#configuring-load-balancing-endpoints" id="configuring-load-balancing-endpoints"></a>

When using WSO2 API Manager, you can configure load balancing endpoints via the API Publisher.

1. When you [create an API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api), to configure load balancing endpoints go to the edit view of the API, click **Endpoint** to view the Endpoint page.
2.  Expand **Load balance and Failover Configurations**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/load-balance-and-fail-over.png" alt=""><figcaption></figcaption></figure>
3.  Select **Load Balanced** as the endpoint type.

    ![load-balanced](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/load-balanced-configurations.png)
4.  Configure the endpoint.

    ![load-balanced](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/load-balanced-configured.png)

    The following are the other configurations that you need to define in order to specify a load balancing endpoint.

    <table data-header-hidden><thead><tr><th width="235"></th><th></th></tr></thead><tbody><tr><td>Production Endpoints</td><td>Specify the set of production endpoints here where the requests need to be load balanced. If required, you can specify more than one endpoint by clicking <strong>+</strong>, and can delete the endpoints by clicking on the <strong>bin</strong> icon.</td></tr><tr><td>Sandbox endpoints</td><td>The set of sandbox endpoints can be specified here where the requests need to be load balanced. If required, you can specify more than one endpoint by clicking <strong>+</strong>, and can delete the endpoints by clicking on the <strong>bin</strong> icon.</td></tr><tr><td>Algorithm</td><td><p>The load balancing algorithm is specified here.</p><p>Click on the cogwheel icon to define the algorithm.</p><p>The default value is the <strong>Round-Robin</strong> Algorithm, which has the <a href="https://synapse.apache.org/apidocs/org/apache/synapse/endpoints/algorithms/RoundRobin.html">org.apache.synapse.endpoints.algorithms.RoundRobin</a> className. If you select another algorithm, you need to specify the class name of the algorithm. Class names of other algorithms can be found <a href="https://synapse.apache.org/apidocs/org/apache/synapse/endpoints/algorithms/package-summary.html">here</a>.</p></td></tr><tr><td>Session Management</td><td><p>Click on the cogwheel icon to configure session management.</p><p>This refers to a session management method from the load balancing group. The possible values are as follows:</p><ul><li><strong>None</strong> - If this is selected, session management is not used.</li><li><strong>Transport</strong> - If this is selected, session management is done on the transport level using HTTP cookies.</li><li><strong>SOAP</strong> - If this is selected, session management is done using SOAP sessions.</li><li><strong>Client ID</strong> - If this is selected, session management is done using an ID sent by the client.</li></ul></td></tr><tr><td>Session Timeout</td><td><p>The number of milliseconds after which the session would time out.</p><p>Click on the cogwheel icon to set up session timeout</p></td></tr></tbody></table>
5. Click **SAVE**.

## Configuring Failover Group of Endpoints <a href="#configuring-failover-group-of-endpoints" id="configuring-failover-group-of-endpoints"></a>

When using WSO2 API Manager, you can configure failover group endpoints via the API Publisher.

1. When you [create an API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api), to configure failover endpoints go to the edit view of the API, click **Endpoint** to view the Endpoint page.
2.  Expand **Load balance and Failover Configurations**.

    ![failover](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/failover.png)
3. Select **Failover** as the endpoint type.
4.  Configure the endpoint.

    ![failover](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/failover-configured.png)

    You need to add at least one failover endpoint as the production and sandbox (based on the endpoints that you have specified) endpoints.

    If required, you can specify more than one endpoint by clicking **+**, and can delete the endpoints by clicking on the **bin** icon.
5. Click **SAVE**.
