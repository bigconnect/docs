# Endpoint Timeouts

WSO2 API Manager allows you to control the production and sandbox endpoints using the Advanced Endpoint Configurations.

Configure the Advanced Endpoint Configurations as follows:

1.  Sign in to the API Publisher Portal.

    `https://<hostname>:9443/publisher`

    Example: `https://localhost:9443/publisher`

    Use your username and password to sign in.
2. Click on an API in the API Publisher Portal listing page.
3. Click **Endpoints** under **API Configurations**.
4.  Click on the cogwheel, which is inline with the endpoint that you need to configure, and update the endpoint related configurations as required.



    The Advanced Endpoint Configurations page appears.



    <div align="left">

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/advance-ep-config-cogwheel.png" alt=""><figcaption></figcaption></figure>

    </div>

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/advance-config-dialog.png" alt=""><figcaption></figcaption></figure>

The following are Advanced Endpoint Configurations that you can configure for both the production and sandbox endpoints.

<table data-header-hidden><thead><tr><th width="221"></th><th></th></tr></thead><tbody><tr><td>Endpoint Suspend State</td><td><p>If you want to configure the suspension of an endpoint, you can use the Endpoint Suspension State section to define the error codes, maximum suspension time, and suspension factors.</p><ul><li><strong>Error Codes:</strong> The error codes that are associated with suspended endpoints. If the backend endpoint receives the selected error codes, the endpoint will be marked as suspended. Specify the <a href="https://apim.docs.wso2.com/en/4.2.0/troubleshooting/error-handling/#transport-error-codes">transport error codes</a> to define when the endpoint suspension should be triggered. You can select a single error code or multiple error codes.</li><li><p><strong>Initial duration:</strong> The time duration for which the endpoint will be suspended when one or more suspend error codes are received from the endpoint for the first time.</p><p>Note</p><p>When creating (or updating) failover endpoints via the Publisher Portal, you need to specify a set of Error Codes for the endpoint to failover on and also remove the Initial Duration by setting its value to -1.</p></li><li><strong>Max duration:</strong> The maximum time duration for which the endpoint will be suspended.</li><li><strong>Factor:</strong> The duration to suspend can vary from the first time suspension to suspension on subsequent occasions. This factor value defines the suspense duration variance between subsequent suspensions.</li></ul></td></tr><tr><td>Endpoint Timeout state</td><td><p>You can define configurations related to retries, error codes, and delays in terms of endpoint timeouts in the Endpoint Timeout State section.</p><ul><li><strong>Error Codes:</strong> You can select a single error code or multiple error codes that you need to associate with endpoint timeouts. If these error codes are received from the endpoint, the request will be subjected to a timeout.</li><li><strong>Retries Before Suspension:</strong> The number of retries that are performed by the Gateway when any of the selected error codes are received before the endpoint timeout takes place.</li><li><strong>Retry Delay:</strong> The delay between retries in milliseconds.</li></ul></td></tr><tr><td>Connection Timeout</td><td><p>Duration and the response actions after a connection timeout can be configured here in the Advanced Endpoint Configuration.</p><ul><li><p><strong>Action:</strong> Response action to be performed after the connection timeout. You can select from one of the following options.</p><ul><li><strong>Discard Message</strong></li><li><strong>Execute Fault Sequence</strong></li></ul><p>The default value is <strong>Execute Fault Sequence</strong>.</p></li><li><p><strong>Duration:</strong> The time duration of the connection timeout in milliseconds.</p><p>Note</p><p>If you want to change the endpoint connection timeout duration globally to affect all the APIs, carry out the following instructions.</p><ol><li>Open the <code>&#x3C;APIM_HOME>/repository/conf/deployment.toml</code> file.</li><li><p>Add or change the value of the timeout as follows:</p><pre><code>[synapse_properties]
'synapse.global_timeout_interval'=3000
</code></pre></li><li><p>Add or change the socket timeout value as follows:</p><pre><code>[passthru_http]
'http.socket.timeout'=3000
</code></pre></li></ol><p>Note that the <strong>socket timeout value</strong> should be greater than both the Synapse global timeout and any endpoint timeouts given for your API.</p></li></ul></td></tr></tbody></table>

For more information on endpoints and how to add, edit, or delete them, see [Endpoint Properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/endpoint-properties).
