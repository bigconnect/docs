# Adaptive Authentication

Adaptive authentication is a secure and flexible form of authentication. It enables validating multiple factors to determine the authenticity of a login attempt before granting access to a resource. The factors that are used for validation can depend on the risk probability associated with the particular user access request. This enables adjusting the authentication strength based on the context at hand.

![adaptive authentication](https://is.docs.wso2.com/en/latest/assets/img/tutorials/adaptive-authentication.png)

MWARE IAM supports script-based adaptive authentication, which allows you to use a script to set up appropriate authentication factors depending on your scenario. This enables ensuring security without impacting usability at the time of authentication.

The following section provides a detailed description of how MWARE IAM supports adaptive authentication.

#### Adaptive Authentication with MWARE IAM <a href="#adaptive-authentication-with-wso2-identity-server" id="adaptive-authentication-with-wso2-identity-server"></a>

The Management Console provides an **authentication script editor** that allows you to define authentication scripts using JavaScript . The script editor provides a set of **predefined templates** that you can use to easily set up adaptive authentication for some of the most common authentication scenarios. You can define scripts that can consider the following evaluation criteria:

* User attributes
* User behaviour
* Level of assurance of the access request
* Risk analysis statistics
* Machine learning algorithms

You can define dynamic authentication sequences that can perform actions similar to the following:

* Control the authentication step selection
* Change user attributes
* Send email notifications
* Redirect users to an error page etc.

If necessary you can use the script editor to introduce new functions and fields to an authentication script based on your requirement, and then engage the script to the service provider’s authentication step configuration.

Following is a sample authentication script.

```java
var onLoginRequest = function(context) {
    // Some possible initializations...
    executeStep(1, {
        onSuccess: function (context) {
            // Logic to execute if step 1 succeeded
            executeStep(2, {
                onSuccess: function (context){
                    // Logic to execute if step 2 succeeded
                },
                onFail: function (context){
                    // Logic to execute if step 2 failed
                }
            });
        }
        onFail: function(context){
            // Logic to execute if step 1 failed
            executeStep(3);
        }
    });
}

function someCommonFunction(context) {
    // Do some common things
}
```

## Configuring a Service Provider for Adaptive Authentication <a href="#configuring-a-service-provider-for-adaptive-authentication" id="configuring-a-service-provider-for-adaptive-authentication"></a>

Adaptive authentication enables an identity provider to prompt multi-factor authentication steps based on a user's risk profile or user behavior, i.e., the authentication adapts to the situation or the user during the authentication process.

1. On the **Main** tab, click **Service Providers > Add** and add a new service provider called `saml2-web-app-pickup-dispatch.com` .
2. Expand the **Inbound Authentication configuration> SAML2 Web SSO configuration** section, and click **Configure**\
   ****![](<../../.gitbook/assets/image (709).png>)****
3. Fill in the following fields.
   * **Issuer:** `saml2-web-app-pickup-dispatch.com`
   * **Assertion Consumer URL:** `http://localhost.com:8080/saml2-web-app-pickup-dispatch.com/consumer`
4. Leave the rest of the default configurations as it is and click **Register**.\
   ![](<../../.gitbook/assets/image (742) (1).png>)
5. Expand **Local and Outbound Authentication Configuration** and click **Advanced Configuration**.\
   ![](<../../.gitbook/assets/image (744) (1).png>)
6. You can add authentication steps or use a template to configure adaptive authentication depending on your requirement. For example, add Demo HardwareKey Authenticator.\
   ![](<../../.gitbook/assets/image (749) (1).png>)
7.  Add the following configuration to the `<IAM_HOME>/repository/conf/deployment.toml` file.

    ```toml
    [[resource.access_control]]
    context = "/sample-auth(.*)"
    secure = false
    http_method = "all"Restart the server
    ```
8. Restart the server
