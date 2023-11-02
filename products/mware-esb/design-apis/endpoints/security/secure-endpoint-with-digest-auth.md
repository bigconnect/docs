# Secure Endpoint with Digest Auth

A secured endpoint is where we have access-protected resources. You have to specify the username and the password when a request is sent to a secured endpoint. The endpoint authentication mechanism can either be Basic Authentication or Digest Authentication. They differ on how the credentials are communicated and how access is granted by the backend server.

Digest Authentication applies a hash function to the username and the password before sending them over the network. It is a process of applying MD5 cryptographic hashing with the usage of nonce values to prevent replay attacks. It is a simple challenge-response authentication mechanism that may be used by a server to challenge a client request and by a client to provide authentication information for the secured endpoint.

Digest Authentication is safer than Basic Authentication, which uses unencrypted base64 encoding instead of a hashing mechanism.

When you [create an API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api) using the API Publisher, you can specify the endpoints of the API backend implementation via the **Endpoints** page as Production and Sandbox endpoints respectively.

Follow the instructions below to use Digest Auth as the endpoint authentication type when using a secured endpoint:

1. Click **Endpoints** in the API Publisher.
2.  Click the Endpoint Security symbol of the endpoint you want to secure with Digest Auth.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/endpoints/endpoint-security/endpoint-security-symbol.png" alt=""><figcaption></figcaption></figure>
3.  Select **Digest Auth** as the endpoint authentication type from the drop-down menu.

    ![digest-auth-dropdown](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/digest-auth-dropdown.png)
4.  After entering your credentials, click **Submit** to confirm the details of the respective endpoint and then click **Save and deploy** in the Endpoints page to save all the changes made in the **Endpoints** page.

    <img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/digest-auth-submit-button.png" alt="digest-auth-submit-button" data-size="original">



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/endpoints/endpoint-security/endpoints-save-button.png" alt=""><figcaption></figcaption></figure>
