# Manage Certificate

If your API backend is secured with a self-signed certificate (or a certificate which is not signed by a CA) you need to import the backend certificate to the API manager (Gateway) client-truststore and restart the server. This feature enables you to upload the backend certificate through API Publisher while creating or editing your API **without restarting the server**.

Follow the steps below to add a certificate to an endpoint:

## Configurations (Optional) <a href="#configurations-optional" id="configurations-optional"></a>

*   Modify default certificate loading time by adding the following configuration to `<API-M_HOME>/repository/conf/deployment.toml` file.

    Format

    ```
    [transport.passthru_https.sender.ssl_profile]
    interval = "<time in milliseconds>"
    ```

    Example

    <table><thead><tr><th width="233">Configuration Parameter</th><th>Description</th></tr></thead><tbody><tr><td>interval</td><td>The time taken to load the newly added certificate in <strong>milliseconds</strong>. Default 10 mins. (600000ms) Minimum interval : 60000ms (1 min)</td></tr></tbody></table>
*   If you are using a different default truststore/keystore configuration and defined it in the `[transport.passthru_https.sender]` or `[keystore.primary]` sections within the `deployment.toml` file, make sure to modify the keystore and truststore configurations for **default SSL profile** in `<API-M_HOME>/repository/conf/deployment.toml` as follows.

    Format

    ```
    [keystore.tls]
    file_name = "<Keystore file location>"
    type = "<Keystore type>"
    password = "<Keystore password>"
    key_password = "<Private Key password>"

    [truststore]
    file_name = "<Truststore file location>"
    type = "<Truststore type>"
    password = "<Truststore password>"
    ```

    Example
*   If you are using a separate keystore and truststore pair (per custom SSL profile) for each endpoint domain, add the configuration for each **custom SSL profile** in `<API-M_HOME>/repository/conf/deployment.toml` as follows.

    Format

    ```
    [[keystore.ssl_profile.custom]]
    servers = "<Endpoint hostname>"
    [keystore.ssl_profile.custom.keystore]
    location = "<Keystore file location>"
    type = "<Keystore type>"
    password = "<Keystore password>"
    key_password = "<Private Key password>"
    [keystore.ssl_profile.custom.truststore]
    location = "<Truststore file location>"
    type = "<Truststore type>"
    password = "<Truststore password>"
    ```

    Example

    Note

    This feature currently supports only the following keystore and certificate types.

    * Keystore : `.jks`
    * Certificate : `.crt`

    If you need to use a certificate in any other format, you can convert it to `.crt/ .cert` using a standard tool before uploading.

    Info

    The certificate will be added to the Gateway nodes which are defined under the `[[apim.gateway.environment]]` section in the `deployment.toml` file.

    In a clustered setup, as the Gateway configurations are identical, sync the `<API-M_HOME>/repository/resources/security/sslprofiles.xml` file and the `<API-M_HOME>/repository/resources/security/client-truststore.jks` file among the Gateway nodes in the cluster. After the configured interval, the Synapse transport will be reloaded in all the Gateway nodes.

## Adding a Certificate for an Endpoint <a href="#adding-a-certificate-for-an-endpoint" id="adding-a-certificate-for-an-endpoint"></a>

1. Sign in to the API Publisher.
2. [Create a new API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api/) or click on an existing API.
3.  Click **Endpoints** and click **General Endpoint Configuration** to expand that section.&#x20;

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/open-general-endpoint-configuration.png" alt=""><figcaption></figcaption></figure>
4.  Click **+ Add Certificate** in the Certificates section.&#x20;

    The Upload Certificate dialog box appears.



    *   Enter the following information and click **Save**.

        | Name        | Description                                                                                       |
        | ----------- | ------------------------------------------------------------------------------------------------- |
        | Alias       | Enter a name for your certificate.                                                                |
        | Endpoint    | Select an endpoint from the dropdown list.                                                        |
        | Certificate | Drag and drop the certificate file or click on the drop zone to select the certificate via the UI |



        The uploaded certificate will be displayed.



        <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/certificate-inputs-provided.png" alt=""><figcaption></figcaption></figure>

        <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/certificate-added.png" alt=""><figcaption></figcaption></figure>

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/click-add-certificate.png" alt=""><figcaption></figcaption></figure>

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/upload-certificate-open.png" alt=""><figcaption></figcaption></figure>
5. If required, repeat step 3 onwards to add certificates to the other endpoints.

### Check Certificate Information[¶](https://apim.docs.wso2.com/en/latest/design/endpoints/certificates/#check-certificate-information) <a href="#check-certificate-information" id="check-certificate-information"></a>

You can check the information of the certificate, (i.e., other APIs that may use this certificate).

Click on the info icon that corresponds to the respective certificate to view the certificate information.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/certificate-info-click.jpg" alt=""><figcaption></figcaption></figure>

The selected certificate details appear.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/certificate-details.png" alt=""><figcaption></figcaption></figure>

### Deleting a certificate[¶](https://apim.docs.wso2.com/en/latest/design/endpoints/certificates/#deleting-a-certificate) <a href="#deleting-a-certificate" id="deleting-a-certificate"></a>

Click on the delete icon that corresponds to the respective certificate to delete a certificate.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/certificate-delete-btn-select.jpg" alt=""><figcaption></figcaption></figure>
