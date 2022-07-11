# Configuring Email OTP

This section provides the instructions to configure multi-factor authentication (MFA) using Email One Time Password (Email OTP) in MWARE IAM Server. The Email OTP enables a one-time password (OTP) to be used at the second step of MFA.

Follow the instructions in the sections below to configure MFA using Email OTP:

## Configure the email OTP provider <a href="#configure-the-email-otp-provider" id="configure-the-email-otp-provider"></a>

You can use IAM Server as the email OTP provider or you can configure Gmail or SendGrid as the email OTP provider using Gmail or SendGrid APIs. Follow the instructions in **one** of **Option1** or **Option2** to set up the email OTP provider.

#### Option1: Configure IAM Serfver as the email OTP provider <a href="#option1-configure-wso2-is-as-the-email-otp-provider" id="option1-configure-wso2-is-as-the-email-otp-provider"></a>

Follow the steps below to configure IAM Server to send emails once the Email OTP is enabled.

1. Enable the email sending configurations of the IAM Server.
2.  Add the following configuration to the `deployment.toml` file in the `<IAM_HOME>/repository/conf/` directory.

    ```toml
    [authentication.authenticator.email_otp]
    name ="EmailOTP"
    enable=true
    [authentication.authenticator.email_otp.parameters]
    EMAILOTPAuthenticationEndpointURL = "https://localhost:9443/authenticationendpoint/email_otp.do"
    EmailOTPAuthenticationEndpointErrorPage = "https://localhost:9443/authenticationendpoint/email_otp_error.do"
    EmailAddressRequestPage = "https://localhost:9443/authenticationendpoint/email_capture.do"
    usecase = "local"
    secondaryUserstore = "primary"
    EMAILOTPMandatory = false
    sendOTPToFederatedEmailAttribute = false
    federatedEmailAttributeKey = "email"
    EmailOTPEnableByUserClaim = true
    CaptureAndUpdateEmailAddress = true
    showEmailAddressInUI = true
    useEventHandlerBasedEmailSender = true
    emailAddressRegex = '(?&lt;=.{1}).(?=.*@)'
    tokenExpirationTime = 300000
    ```
3. Start the IAM Server.

#### Option2: Configure Gmail as the email OTP provider <a href="#option2-configure-gmail-as-the-email-otp-provider" id="option2-configure-gmail-as-the-email-otp-provider"></a>

Follow the steps below to send the One Time Password (OTP) using Gmail APIs or using SendGrid."

1. Create a Google account at [https://gmail.com](https://gmail.com/).
2. Go to [https://console.developers.google.com](https://console.developers.google.com/) and click **ENABLE APIS AND SERVICES**.
3. Search for Gmail API and click on it.
4. Click **Enable** to enable the Gmail APIs.
5. Click **Credentials** and click **Create** to create a new project.
6. Click **Credentials** and click the **Create credentials** drop-down.
7.  Select **OAuth client ID** option.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/oauth-client-id.png" alt="oauth-client-id" data-size="original">
8. Click **Configure consent screen**.\
   ![configure-consent-screen](https://is.docs.wso2.com/en/latest/assets/img/tutorials/configure-consent-screen.png)
9. Enter the Product name that needs to be shown to users, enter values to any other fields you prefer to update, and click **Save**.
10. Select the **Web application** option.

    Enter `https://localhost:9443/commonauth` as the **Authorize redirect URIs** and click **Create**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/authorize-redirect-uris.png" alt="authorize-redirect-uris" data-size="original">

    The `client ID` and the `client secret` are displayed.\
    Copy the client ID and secret and keep it in a safe place as you will require it for the next step.\
    ![client-id-client-secret](https://is.docs.wso2.com/en/latest/assets/img/tutorials/client-id-client-secret.png)
11. Copy the URL below and replace the `<CLIENT_ID>` placeholder tag with the generated `Client ID` . This is required to generate the authorization code.

    Format

    ```java
    https://accounts.google.com/o/oauth2/auth?redirect_uri=https%3A%2F%2Flocalhost%3A9443%2Fcommonauth&response_type=code&client_id=<CLIENT_ID>&scope=http%3A%2F%2Fmail.google.com&approval_prompt=force&access_type=offline
    ```

    Example
12. Paste the updated URL into your browser.
    1. Select the preferred Gmail account with which you wish to proceed.
    2. Click **Allow**.
    3.  Obtain the `authorization code` using a SAML tracer on your browser.

        <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/authorization-code.png" alt="authorization-code" data-size="original">
13. To generate the access token, copy the following cURL command and replace the following place holders:

    1. `<CLIENT-ID>` : Replace this with the `client ID` obtained in Step 10 above.
    2. `<CLIENT_SECRET>` : Replace this with the `client secret` obtained in Step 10 above.
    3. `<AUTHORIZATION_CODE>` : Replace this with the authorization code obtained in Step 12 above.



    ```java
    curl -v -X POST --basic -u <CLIENT-ID>:<CLIENT_SECRET> -H "Content-Type: application/x-www-form-urlencoded;charset=UTF-8" -k -d "grant_type=authorization_code&code=<AUTHORIZATION_CODE>&redirect_uri=https://localhost:9443/commonauth" https://www.googleapis.com/oauth2/v3/token
    ```


14. Update the following configurations in the `<IAM_HOME>/repository/conf/deployment.toml` file.

## Configure the Identity Provider <a href="#configure-the-identity-provider" id="configure-the-identity-provider"></a>

Follow the steps below to add an identity provider:

1. Click **Add** under **Main > Identity > Identity Providers**.
2. Provide a suitable name for the identity provider.\
   ![name-the-identity-provider](https://is.docs.wso2.com/en/latest/assets/img/tutorials/name-the-identity-provider.png)
3. Expand the **EmailOTPAuthenticator Configuration** under **Federated Authenticators**.
4. Select the **Enable** and **Default** check boxes(If you are using Gmail or Sendgrid as the email OTP provider, provide values for Email API and Email fields as well).
5.  Click **Register**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/config-idp-email-otp.png" alt="registering-an-identity-provider" data-size="original">

You have now added the identity provider.

## Configure the Service Provider <a href="#configure-the-service-provider" id="configure-the-service-provider"></a>

Follow the steps below to modify the same service provider in order to configure Email OTP as second authentication step.

1. Return to the Management Console home screen.
2. Click **List** under **Main > Identity > Service Providers** and locate the above created service provider and click **Edit**.
3.  Go to **Claim Configuration** and select the **http://wso2.org/claims/emailaddress** claim.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/claim-configuration.png" alt="claim-configuration" data-size="original">
4.  Go to **Local and Outbound Authentication Configuration** section.

    1. Select the **Advanced configuration** radio button option.
    2. Creating the first authentication step:
       1. Click **Add Authentication Step**.
       2. Click **Add Authenticator** that is under Local Authenticators of Step 1 to add the basic authentication as the first step.\
          Adding basic authentication as a first step ensures that the first step of authentication will be done using the user's credentials that are configured with the IAM Server
    3. Creating the second authentication step:
       1. Click **Add Authentication Step**.
       2. Click **Add Authenticator** that is under Federated Authenticators of Step 2 to add the EMAIL OTP identity provider you created as the second step.\
          EMAIL OTP is a second step that adds another layer of authentication and security.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/add-authenticator.png" alt="add-authenticator" data-size="original">
5.  Click **Update**.

    You have now added and configured the service provider.
