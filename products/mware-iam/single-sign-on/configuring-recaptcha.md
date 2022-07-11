# Configuring reCaptcha

This topic guides you through configuring reCaptcha for the single sign on flow. By configuring reCaptcha, you can mitigate or block brute force attacks.

1. Start the MWARE IAM and login to the management console.
2. Click **List** under **Identity Providers** found in the **Main** tab.
3. Click **Resident Identity Provider** and expand the **Login Attempts Security** tab. Then, expand the **reCaptcha for SSO Login** tab.
4. Select the relevant option according to your requirement:
   *   **Always prompt reCaptcha:**

       Select this option to prompt users for reCaptcha with every SSO login attempt.
   *   **Prompt reCaptcha after max failed attempts:**

       Select this option to prompt reCaptcha only after the number of max failed attempts has been exceeded.

       If this option is selected, enter a value for the **Max failed attempts for reCaptcha** field as well. For example, if you enter 3, reCaptcha will be re-enabled after 3 failed attempts.\
       ![configure-captcha-for-sso](https://is.docs.wso2.com/en/latest/assets/img/learn/recaptcha-sso.png)

       Note the following when selecting this option:

       * Account locking must be enabled to enable **Prompt reCaptcha after max failed attempts**.
       *   The **Max failed attempts for reCaptcha** value must be lower than the **Maximum failed login attempts** value configured under the **Account Lock** tab.

           <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/configure-account-locking.png" alt="configure-account-locking" data-size="original">
5.  You have now successfully configured reCaptcha for the single sign on flow. If the number of failed attempts reaches the maximum configured value, the following reCaptcha window appears.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/tutorials/captcha-login-failed.png" alt="captcha-login-failed" data-size="original">
