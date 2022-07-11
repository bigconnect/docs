# Configuring TOTP

The TOTP authenticator allows you to authenticate a user using **Time-Based One Time Password** (TOTP) through IAM Server. It uses the TOTP specification to calculate the access tokens based on the time and the shared secret key between the user and the identity provider.

TOTP is an **algorithm-generated temporary passcode** that is used for strong authentication. The algorithm that generates each passcode uses the current time of day as one of its factors, ensuring that each password is unique.

### Configuring the service provider[¶](https://is.docs.wso2.com/en/latest/learn/configuring-totp/#configuring-the-service-provider) <a href="#configuring-the-service-provider" id="configuring-the-service-provider"></a>

The next step is to configure the service provider.

1. Locate the sample app provider and navigate to the **Local and Outbound Authentication Configuration** section.
2. Select the **Advanced configuration** radio button option.
3. To create the first authentication step:
   1. Click **Add Authentication Step**.
   2. Click **Add Authenticator** that is under **Local Authenticators** of Step 1 to add the `basic` authentication as the first step.\
      Adding basic authentication as a first step ensures that the first step of authentication will be done using the user's credentials that are configured with the WSO2 Identity Server
4. To create the second authentication step:
   1. Click **Add Authentication Step**.
   2. Click **Add Authenticator** that is under **Local Authenticators** of Step 2 to add the `totp` from the drop down as the second step.\
      ![creating-the-second-authentication-step-totp](https://is.docs.wso2.com/en/latest/assets/img/tutorials/creating-the-second-authentication-step-totp.png)

Now \`totp' is a second step that adds another layer of authentication and security during the authentication for this application.

### Configuring the user profile[¶](https://is.docs.wso2.com/en/latest/learn/configuring-totp/#configuring-the-user-profile) <a href="#configuring-the-user-profile" id="configuring-the-user-profile"></a>

1. Login to the My Account user portal and go to **Personal info**.
2. Update your email address in **Profile**(this email address is used to send the token). ![my-profile-gadget](https://is.docs.wso2.com/en/latest/assets/img/tutorials/update-email-in-profile.png)
3.  You can use the [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2\&hl=en) Application to generate the one-time passwords (tokens). Click on **Security** and go to **Multi-factor authentication**. Click on the eye icon to scan the QR-Code using the Google Authenticator mobile app.

    ![my-profile-gadget](https://is.docs.wso2.com/en/latest/assets/img/tutorials/totp-authenticator-app.png) ![my-profile-gadget](https://is.docs.wso2.com/en/latest/assets/img/tutorials/totp-authenticator-qr-code.png)
