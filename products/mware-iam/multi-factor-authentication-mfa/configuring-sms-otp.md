# Configuring SMS OTP

The SMSOTP authenticator allows you to authenticate user via SMS through MWARE IAM. This document explains how to configure the SMS OTP connector with IAM Server.

## Configuring the identity provider <a href="#configuring-the-identity-provider" id="configuring-the-identity-provider"></a>

First you have to configure IAM Server by adding a new identity provider.

1. Start the Server.
2. Download the certificate of the SMS provider by going to the SMS providers website on your browser, and clicking the HTTPS trust icon on the address bar.
3.  Navigate to the `<IAM_HOME>/repository/resources/security` directory via the terminal and import the downloaded certificate into the IAM Server client keystore.

    ```java
    keytool -importcert -file <CERTIFICATE_FILE_PATH> -keystore client-truststore.jks -alias "Vonage" 
    ```
4. You are prompted to enter the keystore password. The default `client-truststore.jks` password is **`wso2carbon`**.
5. Log into the management console as an administrator.
6. Navigate to **Main** tab -> **Identity** -> **Identity Providers** -> **Add**.
7. Give a suitable name (e.g., SMSOTP) as the **Identity Provider Name**.
8. Go to the **SMS OTP Configuration** under **Federated Authenticators**.
9. Select both check-boxes to **Enable SMSOTP Authenticator** and to make it the **Default**.
10. Enter the SMS URL, the HTTP Method used (e.g., GET or POST), and the headers and payload if the API uses any.
11. Click **Register**.

## Configuring the service provider <a href="#configuring-the-service-provider" id="configuring-the-service-provider"></a>

The next step is to configure the service provider.

1. Navigate to **Main** tab -> **Identity** -> **Service Providers** -> **Add**.
2. Give a suitable name (e.g., saml-dispatch) as the **Service Provider Name**.
3. Configure Inbound Authentication for the service provider.
4.  Go to **Claim configuration** and select the **`http://wso2.org/claims/mobile`** claim for the **Subject Claim URI**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/subject-claim-uri.png" alt="subject-claim-uri" data-size="original">
5. Go to **Local and Outbound Authentication Configuration** section.
   1. Select the **Advanced configuration** radio button option.
   2. Creating the first authentication step:
      1. Click **Add Authentication Step**.
      2. Click **Add Authenticator** that is under **Local Authenticators** of Step 1 to add the basic authentication as the first step.\
         Adding basic authentication as a first step ensures that the first step of authentication will be done using the user's credentials that are configured with the WSO2 Identity Server
   3.  Creating the second authentication step:

       1. Click **Add Authentication Step**.
       2. Click **Add Authenticator** that is under **Federated Authenticators** of Step 2 to add the SMSOTP identity provider you created as the second step.\
          SMSOTP is a second step that adds another layer of authentication and security.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/creating-the-second-authentication.jpeg" alt="creating-the-second-authentication" data-size="original">
6. Click **Update** to save the changes.

You have now added and configured the service provider.
