# Secure Endpoint with Basic Auth

A secured endpoint is when there are access-protected resources. You have to specify the username and the password when a request is sent to a secured endpoint. The endpoint authentication mechanism can either be Basic Authentication or Digest Authentication. They differ on how the credentials are communicated and how access is granted by the backend server.

Basic Authentication is the simplest mechanism used to enforce access controls to web resources. Here, the HTTP user agent provides the username and the password when making a request. The string containing the username and the password separated by a colon is Base64 encoded and sent in the authorization header when calling the backend when authentication is required.

When you [create an API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api) using the API Publisher, you can specify the endpoints of the API backend implementation via the **Endpoints** page as Production and Sandbox endpoints respectively.

Follow the instructions below to use Basic Auth as the endpoint authentication type when using a secured endpoint:

1. Click **Endpoints** in API Publisher.
2.  Click the Endpoint Security symbol of the endpoint that you want to secure with Basic Auth.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/endpoints/endpoint-security/endpoint-security-symbol.png" alt=""><figcaption></figcaption></figure>
3.  Select **Basic Auth** as the endpoint authentication type from the drop-down menu.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/basic-auth-dropdown.png" alt=""><figcaption></figcaption></figure>
4.  After entering your credentials, click **Submit** to confirm the details of the respective endpoint and then click **Save and deploy** in the Endpoints page to save all the changes made in the **Endpoints** page.





    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/endpoints/endpoint-security/endpoints-save-button.png" alt=""><figcaption></figcaption></figure>

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/basic-auth-submit-button.png" alt=""><figcaption></figcaption></figure>

\
