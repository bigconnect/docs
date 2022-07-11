# WS-Trust

The **Security Token Service** component of MWARE IAM enables you to configure the generic STS to issue claim-based security tokens. A claim-based security token is a common way for applications to acquire and authenticate the identity information they need about users inside their organization, in other organizations, and on the Internet. This Security Token Service is capable of issuing SAML 1.1 and SAML 2.0 tokens as recommended in WS-Trust and SAML Web Service Token Profile specifications.

The WSDL of this service can be accessed by clicking the URL having the format: `https://(hostname):(port)/services/wso2carbon-sts?wsdl`. For instance, with the default configuration, the URL is [https://localhost:9443/services/wso2carbon-sts?wsdl](https://localhost:9443/services/wso2carbon-sts?wsdl).

Both SAML 1.1 and SAML 2.0 token types are supported by default. The issued token type is decided based on the type of token defined in the RST (Request Security Token). Currently, the Bearer Subject Confirmation and Holder-Of-Key subject confirmation methods, which are explained below, are supported. With Holder-Of-Key, both Symmetric and Asymmetric key types are supported. You can obtain tokens containing claims that hold certain information about the subject. These claims can be extracted from the profiles or through custom claim callbacks which can be registered to the Carbon runtime.

**Understanding different confirmation methods**

Subject confirmation methods are how a relying party (or the end service) can make sure that a particular security token issued by a Security Token Service (STS), is brought by the legitimate subject. If this is not done, a third party can take the token from the wire and send any request it wants including that token. As a result, the relying party may trust that illegitimate third party.

The following are the three methods of confirmation.

* **Bearer** : This is actually not a confirmation method as subject confirmation is not really needed. The relying party simply trusts whoever brings the token.
* **Holder of Key (HoK)** :
  * STS includes the public key of the client inside the security token and signs it.
  * Before sending the token, the client itself signs the request.
  * When the relying party receives the token, it first validates the STS signature and then validates the client's signature with the public key embedded inside the token.
* **Sender Vouches** :
  * Rather than authenticating with the STS, the client authenticates with an intermediate service.
  * The intermediary gets the security token from the STS and signs the request before sending it to the relying party.
  * The relying party trusts both the intermediary and the STS. So, it validates both of them.

## Configuring WS-Trust Security Token Service <a href="#configuring-ws-trust-security-token-service" id="configuring-ws-trust-security-token-service"></a>

MWARE IAM uses the security token service (STS) as the WS-Trust implementation. The STS is capable of issuing SAML 1.1 and 2.0 security tokens and has a SOAP/XML API for token issuance. This API can be secured with the `UserNameToken` or with any other WS-Security mechanism as explained below.

### **Securing the Security Token Service**

According to the Trust Brokering model defined in the WS-Trust specification, the users should authenticate themselves to the STS before obtaining a token. STS may use this authentication information when constructing the security token. For example, STS may populate the required claims based on the user name provided by the subject. Therefore, the STS service needs to be secured.

STS is configured under the **Resident Identity Provider** section of the Management Console.

To secure the Security Token Service:

1. On the **Main** tab, click **Identity > Identity Providers > Resident**.\
   ![main-tab-resident](https://is.docs.wso2.com/en/latest/assets/img/tutorials/main-tab-resident.png)\
   The Resident Identity Provider page appears.\
   ![resident-identity-provider](https://is.docs.wso2.com/en/latest/assets/img/tutorials/resident-identity-provider.png)
2. Enter the required values
3. Under the **Inbound Authentication Configuration** section, click **Security Token Service Configuration > Apply Security Policy** .\
   ![security-token-service-config](https://is.docs.wso2.com/en/latest/assets/img/tutorials/security-token-service-config.png)
4. Select **Yes** in the **Enable Security?** drop down and  select a pre-configured security scenario according to your requirements. For this tutorial, use **UsernameToken** under the **Basic Scenarios** section.\
   ![enable-security](https://is.docs.wso2.com/en/latest/assets/img/tutorials/enable-security.png)
5. Click **Next**. The user domain and user group selection appears.
6. Provide the required details as follows:
   1. Select **ALL-USER-STORE-DOMAINS**.
   2. Select the role you created to grant permission to access secured service. In this example, the admin role is used. Next, click **Finish**.\
      ****![](<../../../.gitbook/assets/image (724).png>)
7. Click **Finish**.
8. Click **Ok** on the confirmation dialog window that appears.
9. Click **Update** to complete the process.

Now STS is configured and secured with a username and password. Only users with the Admin role can consume the service.

The next step is to add a service provider to consume the STS.

### **Adding a service provider for the STS client**

Do the following steps if you are using a Holder of Key **subject confirmation method**.&#x20;

{% hint style="info" %}
The **Subject confirmation methods** define how a relying party (RP), which is the end service can make sure a particular security token issued by an STS is brought by the legitimate subject. If this is not done, a third party can take the token from the wire and send any request it wants including that token. The RP trusts that illegitimate party.
{% endhint %}

1. Under the **Inbound Authentication Configuration** section, click **WS-Trust Security Token Service Configuration** **>** **Configure**. The STS Configuration page appears.\
   ![sts-config](https://is.docs.wso2.com/en/latest/assets/img/tutorials/sts-config.png)
2. Enter the required details.\
   ![](<../../../.gitbook/assets/image (711).png>)
3. Click **Update** to save the changes made to the service provider.
