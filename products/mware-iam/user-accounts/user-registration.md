# User Registration

MWARE IAM enables user registration using the following methods:

* [User Registration with Password Entry](user-registration.md#user-registration-with-password-entry)
* [User Registration with Email Verification](user-registration.md#user-registration-with-email-verification)
* [User Self-Registration](user-registration.md#self-registration)

## Registration with Password Entry <a href="#user-registration-with-password-entry" id="user-registration-with-password-entry"></a>

In this approach, an administrator first creates a user account in the system upon which a confirmation link will be emailed to the user’s registered email Id. Once the user clicks on the confirmation link, the user will be taken into a screen prompting to enter the password. Once the user enters the correct password, the user account gets activated.

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. While **Robert** is an administrator at Pickup, **Jane** is a new recruit.

Robert wants to register a user account for Jane and get it activated by requesting Jane to enter a password.

![Scenario Diagram](https://is.docs.wso2.com/en/latest/assets/img/learn/pickup-user-registration-with-password.png)

Let's learn how it's done!

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure MWARE IAM to enable password entry for account confirmation.

1. Open the `deployment.toml` file in the `<IAM_HOME>/repository/conf` directory.
   1.  To request password entry from the users who are registered via Management Console, add the following configurations.

       ```toml
       [identity_mgt.user_onboarding]
       ask_password_from_user= true
       ```
   2.  To configure the email server to send emails requesting password entry, add the following configurations.

       * **from\_address**: This is the email address from which the confirmation email will be sent.
       * **username**: This is the user name of the given email address.
       * **password**: This is the password of the given email address.



       ```toml
       [output_adapter.email]
       from_address= ""
       username= ""
       password= ""
       hostname= "smtp.gmail.com"
       port= 587
       enable_start_tls= true
       enable_authentication= true
       ```

{% hint style="info" %}
**If you are using a Google email account**

Google has restricted third-party applications and less secure applications from sending emails by default. As MWARE IAM acts as a third-party application when sending emails for password entry, follow the steps below to enable your Google email account to provide access to third-party applications.

1. Access [https://myaccount.google.com/security](https://myaccount.google.com/security).
2. Under **Signing in to Google** section, turn off the **2-step Verification** option.
3. Enable **Less secure app access** in the Google Account security section.
{% endhint %}

1. Restart MWARE IAM.
2. Sign in to the MWARE IAM Management Console at `https://<SERVER_HOST>:9443/carbon` as an administrator.
3.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident menu-item" data-size="original">
4.  Under the **Account Management Policies** section, click **User Onboarding > Ask Password**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-onboarding-option.png" alt="User Onboarding Option" data-size="original">
5.  Select the **Enable User Email Verification** check box.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/ask-password-enable-email-verification.png" alt="Ask password code expiry time text box" data-size="original">
6.  You may enter the password entry validity period (in minutes) in the **Ask password code expiry time** text box.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/ask-password-expiry-time.png" alt="Ask password code expiry time text box" data-size="original">
7.  Click **Update**.

    You have now configured MWARE IAM to send the user account confirmation email to the registered user. Let's try it out!

### Try out <a href="#try-out" id="try-out"></a>

1. To create the user account for Jane:
   1.  On the **Main** menu of the Management Console, click **Identity > Users and Roles > Add**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-users-and-roles-menu-item.png" alt="Add Users and Roles menu-item" data-size="original">
   2.  Click **Add New User**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-user-option.png" alt="Add New User option" data-size="original">
   3.  Enter the required data as follows.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-user-ask-password.png" alt="Add New User screen" data-size="original">

       * **Domain**: `Primary`
       * **Username**: `Jane`
       * **Define password here**: Unselected
       * **Ask Password from user**: Selected
       * **Email Address**: Enter the email address to which you wish to receive the account confirmation email.
   4. Click **Finish**.
   5. A confirmation email requesting to enter the password is sent to the given email address.
   6. Click **Create Password**. The **Reset Password** screen appears.
   7. Enter a preferred password and click **Submit**.

## Registration with Email Verification <a href="#user-registration-with-email-verification" id="user-registration-with-email-verification"></a>

In this approach, an administrator first creates a user account with a default password that will be emailed to the user's registered email Id for confirmation.

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. While **Robert** is an administrator at Pickup, **Mark** is a new recruit.

Robert wants to register a user account with a default password for Mark and get it activated by requesting Mark to verify.

![Scenario Diagram](https://is.docs.wso2.com/en/latest/assets/img/learn/pickup-user-registration-with-email-verification.png)

Let's learn how it's done!

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure MWARE IAM to enable password entry for account confirmation.

1. Open the `deployment.toml` file in the `<IAM_HOME>/repository/conf` directory.
   1.  To request email verification from the users who are registered with a default password, add the following configurations.

       ```toml
       [identity_mgt.user_onboarding]
       enable_email_verification = true 
       lock_on_creation=true
       ```
   2.  To configure the email server to send emails requesting a password entry, add the following configurations.

       * **from\_address**: This is the email address from which the confirmation email will be sent.
       * **username**: This is the user name of the given email address.
       * **password**: This is the password of the given email address.



       ```toml
       [output_adapter.email]
       from_address= ""
       username= ""
       password= ""
       hostname= "smtp.gmail.com"
       port= 587
       enable_start_tls= true
       enable_authentication= true
       ```


2. Restart MWARE IAM.
3. Sign in to the MWARE IAM Management Console at `https://<SERVER_HOST>:9443/carbon` as an administrator.
4.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident menu-item" data-size="original">
5.  Under the **User Onboarding** section, click **Ask Password**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-onboarding-option.png" alt="User Onboarding Option" data-size="original">
6.  Select the **Enable User Email Verification** check box.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/ask-password-enable-email-verification.png" alt="Ask password code expiry time text box" data-size="original">
7. You may enter the confirmation email validity period (in minutes) in the **Email verification code expiry time** text box.
8. Click **Update**.

You have now configured MWARE IAM to send the user account confirmation email to the registered user. Let's try it out!

### Try out <a href="#try-out" id="try-out"></a>

1.  To create the user account for Mark, execute the following cURL.

    ```
    curl -v -k --user admin:admin --data '{"schemas":[],"name":{"familyName":"Mark","givenName":"Roe"},"userName":"Mark","password":"password","emails":[{"primary":true,"value":"<USER_EMAIL>"}],"urn:ietf:params:scim:schemas:extension:enterprise:2.0:User":{verifyEmail:"true"}}' --header "Content-Type:application/json" https://localhost:9443/scim2/Users
    ```
2. An email requesting to confirm the user creation is sent to the given email address.
3. To verify account creation, click **Confirm Account**.

## Self-Registration

In this approach, when a user self-registers, a user account gets created and an email requesting for account confirmation is sent to the user's registered email ID. The account remains locked for a predefined duration during which the user should confirm the account creation. If the user does not confirm the account creation before the predefined duration, the user account gets locked. Such accounts can be deleted by administrators to manage resources effectively.

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. While **Robert** is an administrator at Pickup, **Mark** is a new recruit who wishes to use the self-registration option to create a user account.

When Mark self-registers, an email requesting for account creation confirmation is sent to Mark's registered email account. Mark should respond to the confirmation email before the confirmation acceptance window expires. If Mark fails to confirm the account creation, the user account will be locked and Robert will be able to delete this user account.

![Scenario Diagram](https://is.docs.wso2.com/en/latest/assets/img/learn/self-registration.png)

Let's learn how it's done!

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure MWARE IAM to enable password entry for account confirmation.

1. Open the `deployment.toml` file in the `<IAM_HOME>/repository/conf` directory.
   1.  Make sure the following listener configs are in place.

       ```
       [event.default_listener.identity_mgt]
       priority= "50"
       enable = false
       [event.default_listener.governance_identity_mgt]
       priority= "95"
       enable = true
       [event.default_listener.governance_identity_store]
       priority= "97"
       enable = true
       ```
   2.  To send user account creation confirmation email, add the following configurations.

       ```toml
       [identity_mgt.user_onboarding]
       enable_email_verification = true 
       lock_on_creation=true
       ```
   3.  To configure the email server to send emails requesting a password entry, add the following configurations.

       * **from\_address**: This is the email address from which the confirmation email will be sent.
       * **username**: This is the user name of the given email address.
       * **password**: This is the password of the given email address.



       ```toml
       [output_adapter.email]
       from_address= ""
       username= ""
       password= ""
       hostname= "smtp.gmail.com"
       port= 587
       enable_start_tls= true
       enable_authentication= true
       ```


2. Restart MWARE IAM.
3. Sign in to the Management Console at `https://<SERVER_HOST>:9443/carbon` as an administrator.
4.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident menu-item" data-size="original">
5.  Under the **User Onboarding** section, click **Self Registration**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-self-registration-option.png" alt="User Self-Registration Option" data-size="original">
6.  Enter the required information as given below.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-self-registration-form.png" alt="User Self-Registration form" data-size="original">

    1. To enable self-registration, select the **Enable Self User Registration** check box.
    2. To keep the user account that is created through self-registration locked until the user responds to the account confirmation email, select the **Enable Account Lock On Creation Enabled** check box.
    3. To enables sending confirmation emails to the user, select the **Enable Notification Internally Management** check box. If the client application handles notifications, unselect.
    4. You may enter the account confirmation email validity period (in minutes) in the **User self registration code expiry time** text box.
    5. To set attributes that the user will consent to share:
       1. Click **Click here** under **Manage Self-Sign-Up purposes**.
       2.  Click **Add New Purpose**.

           <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/consent-purpose-form.png" alt="Consent Purpose form" data-size="original">

           The **Add New Purpose** screen appears.

           <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-purpose-form.png" alt="Add New Consent Purpose form" data-size="original">
       3. Enter `To send marketing material` in the **Purpose Name** text box.
       4. Enter `To send product marketing material via email and text messages` in the **Description** text area.
       5. Under **Select PII Categories**, click **Add PII Category**.
       6.  Select the following options.

           * `http://wso2.org/claims/givenname`
           * `http://wso2.org/claims/emailaddress`
           * `http://wso2.org/claims/mobile`

           <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/pii-category-entry.png" alt="PII Category entry" data-size="original">
       7. Set the **First Name** and **Email** display names to mandatory.
       8. Click **Finish**. Note that the Consent Purposes screen appears.
       9. Click **Finish**. Note that the User Self Registration screen appears.
7. Click **Update**.

You have now configured MWARE IAM to enable self-registration. Let's try it out!

### Try out <a href="#try-out" id="try-out"></a>

1. Access MWARE IAM User Portal at `https://localhost:9443/myaccount/`.
2.  Click **Create Account**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/register-now-option.png" alt="Register Now option" data-size="original">
3.  Enter the user name as `Mark` and click **Proceed to Self Register**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/start-signing-up-form.png" alt="Start Signing Up form" data-size="original">



    The **Create New Account** screen appears.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/create-new-account-screen.png" alt="Create New screen" data-size="original">

    1. Enter `Mark` in the **First Name** text box.
    2. Enter a preferred password in the **Password** and **Confirm password** text boxes.
    3. Enter your email address in the **Email** text box. Note that as per the PII configurations, this text box appears mandatory.
    4. You may enter other details in the given text boxes. Note that as per the PII configurations, those text boxes appear optional.
    5. Read the Privacy Policy and select the **Privacy Policy** check box.
    6. Click **Register**.
4. An email requesting to confirm the user account creation is sent to the given email address.
5.  To respond to the account confirmation email, click **Confirm Account**. A confirmation message appears.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-confirmation-notification.png" alt="Account Confirmation notification" data-size="original">
6. Click **Close**. The MWARE IAM User Portal Login screen appears.
7. Log in with Mark's credentials and navigate to the Home screen.
