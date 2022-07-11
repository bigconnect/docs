# Password Policies

MWARE IAM enables configuring password policies to enhance user password strength using the following features:

* [Password Patterns](password-policies.md#configuring-password-patterns)
* [Password History](password-policies.md#validating-password-history)
* Password Expiry
* [Password Reset](password-policies.md#configuring-password-reset)

## Password Patterns <a href="#configuring-password-patterns" id="configuring-password-patterns"></a>

MWARE IAM enables defining custom password policies and enforcing them at the point of user creation. This enables complying with password policies that are deemed necessary through various regulations.

* Minimum password length: 6
* Maximum password length: 12
* Must contain at least 1 lowercase alphabetical character
* Must contain at least 1 uppercase alphabetical character
* Must contain at least 1 numeric character
* Must contain at least one special character out of the `!@#$%&*` characters

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. Sam, who is an administrator at Pickup, wants to enforce the following password pattern:

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. Sam, who is an administrator at Pickup, wants to enforce the following password pattern:

* Minimum password length: 6
* Maximum password length: 12
* Must contain at least 1 lowercase alphabetical character
* Must contain at least 1 uppercase alphabetical character
* Must contain at least 1 numeric character
* Must contain at least one special character out of the `!@#$%&*` characters

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to define the password policy that Sam wants to enforce.

1.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident Identity Provider menu-item" data-size="original">
2.  Under **Password Policies**, click **Password Patterns**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/password-patterns-option.png" alt="Passwrod Patterns option" data-size="original">
3.  Enter the required values as given below.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/password-patterns-form.png" alt="Passwrod Patterns form" data-size="original">

    | Field                          | Description                                                           | Sample Value                                                                                                                                                               |
    | ------------------------------ | --------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Enable Password Policy Feature | This enables password patterns in WSO2 Identity Server.               | Selected                                                                                                                                                                   |
    | Password Policy Min Length     | This is the minimum password length allowed.                          | `6`                                                                                                                                                                        |
    | Password Policy Max Length     | This is the maximum password length allowed.                          | `12`                                                                                                                                                                       |
    | Password Policy Pattern        | This is the allowed password pattern.                                 | `^((?=.`_`\d)(?=.`_`[a-z])(?=.`_`[A-Z])(?=.`_`[!@#%&*])).{0,100}`                                                                                                          |
    | Password Policy Error Message  | This appears when the password violates the defined password pattern. | `'Password pattern policy violated! The password should contain a digit [0-9], a lower case letter [a-z], an upper case letter [A-Z], and one of the !@#$%&* characters.'` |

### Try out <a href="#try-out" id="try-out"></a>

1. On the **Main** menu of the Management Console, click **Identity > Users and Roles > Add**.
2. Click **Add New User**.
3. Enter `Alex` as the user name and `test123` as the password.
4.  Click **Finish**. Note that the error message that you added for the password pattern appears.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/password-patter-violation-error-message.png" alt="Password Pattern Viloated error message" data-size="original">

## Password History <a href="#validating-password-history" id="validating-password-history"></a>

MWARE IAM enables restricting the user from re-entering recently used passwords as a new password. For example, if we have set this configuration to 5, the user will not be able to re-use any of the five most recently used passwords when setting a new password.

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. Sam, who is an administrator at Pickup, wants to restrict users from re-using the three most recently used passwords when setting a new password.

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to define the password policy that Sam wants to enforce.

1.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident Identity Provider menu-item" data-size="original">
2.  Under **Password Policies**, click **Password History**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/password-history-option.png" alt="Passwrod History option" data-size="original">
3.  Enter the required values as given below.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/password-history-form.png" alt="Passwrod History form" data-size="original">

    | Field                             | Description                                                                       | Sample Value |
    | --------------------------------- | --------------------------------------------------------------------------------- | ------------ |
    | Enable Password History Feature   | This enables password history validation.                                         | Selected     |
    | Password History validation count | This defines after how many password updates the user can re-use an old password. | `3`          |

### Try out <a href="#try-out" id="try-out"></a>

1. To create the user:
   1. On the **Main** menu of the Management Console, click **Identity > Users and Roles > Add**.
   2. Click **Add New User**.
   3. Enter `Alex` as the user name and `testiam`as the password.
   4. Click **Finish**.
2. To assign login permissions to the user:
   1.  Click the **View Roles** option of Alex.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/view-roles-option.png" alt="View Roles option" data-size="original">
   2.  Click **Permissions**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/role-permissions-option.png" alt="Role Permissions option" data-size="original">
   3.  Select **Login** and click **Update**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/login-permission.png" alt="Login permission" data-size="original">
3. To change the password:
   1. Access MWARE IAM user portal at `https://localhost:9443/user-portal/`.
   2.  Log in with the credentials of the user account that you created.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/sign-in-form.png" alt="Sign In form" data-size="original">
   3.  Under **Security**, click **Change your password**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/userportal-change-password-option.png" alt="Change Passwrod option" data-size="original">
   4.  Enter `testiam` in **Current Password**, **New Password**, and **Confirm Password** text boxes.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/change-password-form.png" alt="Change Password form" data-size="original">
   5.  Click **Update**. An error message appears.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/passwore-history-error-message.png" alt="Passwrod History Validation error message" data-size="original">
   6.  To mimic three consecutive password changes, change Alex's password to the following sequentially.

       * `test123`
       * `test234`
       * `test345`

       Note that these passwords get successfully added to the system.
   7. Now, change Alex's password back to `testiam`. Note that the password gets successfully changed.

## Password Expiry <a href="#password-expiry" id="password-expiry"></a>

MWARE IAM supports configuring a password expiry policy to prompt users to change their password after a defined time period. This is supported via a connector.&#x20;

## Password Reset <a href="#configuring-password-reset" id="configuring-password-reset"></a>

MWARE IAM enables users to reset their forgotten passwords using the following methods:

* [Password Reset with Email](password-policies.md#configuring-password-recovery-with-email)
* [Password Reset with Challenge Question](password-policies.md#configuring-password-reset-with-challenge-questions)

### With Email <a href="#configuring-password-recovery-with-email" id="configuring-password-recovery-with-email"></a>

MWARE IAM enables resetting user passwords by emailing a password reset link to the user’s registered email Id.

#### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. **Alex** who is a new recruit at Pickup has forgotten the password.

Let's learn how Alex can recover the password via email!

#### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure MWARE IAM to enable password reset via email notifications.

1. Open the `deployment.toml` file in the `<IAM_HOME>/repository/conf` directory.
   1.  Check whether the following listener configs are in place.

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
   2.  To configure the email server to send emails requesting password reset, add the following configurations.

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
5.  Under the **Account Management** section, click **Account Recovery**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-recovery-option.png" alt="Account Recovery Option" data-size="original">
6.  Select **Notification Based Password Recovery** check box.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/notification-based-password-recovery-option.png" alt="Notification-Based Password Recovery Option" data-size="original">
7. Click **Update**.

#### Try out <a href="#try-out" id="try-out"></a>

1. To create the user account for Alex:
   1.  On the **Main** menu of the Management Console, click **Identity > Users and Roles > Add**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-users-and-roles-menu-item.png" alt="Add Users and Roles menu-item" data-size="original">
   2.  Click **Add New User**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-user-option.png" alt="Add New User option" data-size="original">
   3.  Enter the required data as follows.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-user-screen.png" alt="Add New User screen" data-size="original">

       * **Domain**: `Primary`
       * **Username**: `Alex`
   4. Click **Finish**.
2. To assign login permissions to the user:
   1.  Click the **View Roles** option of Alex.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/view-roles-option.png" alt="View Roles option" data-size="original">
   2.  Click **Permissions**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/role-permissions-option.png" alt="Role Permissions option" data-size="original">
   3.  Select **Login** and click **Update**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/login-permission.png" alt="Login permission" data-size="original">
3. To add Alex's email Id:
   1.  Click **User Profile** option of Alex.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-profile-option.png" alt="User Profile option" data-size="original">
   2.  Enter an email address to which Alex's password recovery emails will be sent.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-email.png" alt="User Email option" data-size="original">
   3. Click **Update**.
4. To mimic a forgotten password:
   1. Access User Portal at `https://localhost:9443/user-portal/`.
   2.  Click **Password**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/forgotten-password-option.png" alt="Sign In form" data-size="original">
   3. Enter the user name as `Alex` and select the **Recover with Mail** option.
   4. Click **Submit**.

### With Challenge Questions <a href="#configuring-password-reset-with-challenge-questions" id="configuring-password-reset-with-challenge-questions"></a>

MWARE IAM enables resetting user passwords by correctly responding to predefined challenge questions (also known as security questions).

#### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. **Alex** who is a new recruit at Pickup has forgotten the password.

Let's learn how Alex can recover the password by answering to a challenge question!

#### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure MWARE IAM to enable password reset by answering to a challenge question.

1.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident menu-item" data-size="original">
2.  Under the **Account Management Policies** section, click **Account Recovery**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-recovery-option.png" alt="Account Recovery Option" data-size="original">
3.  Enter the required values as given below:

    * **Enable Notification Based Password Recovery**: Selected
    * **Number of Questions Required for Password Recovery**: `2`

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/security-question-based-password-recovery-option.png" alt="Security-Question-Based Password Recovery Option" data-size="original">
4. Click **Update**.

#### Try out <a href="#try-out" id="try-out"></a>

1. To create the user account for Alex:
   1.  On the **Main** menu of the Management Console, click **Identity > Users and Roles > Add**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-users-and-roles-menu-item.png" alt="Add Users and Roles menu-item" data-size="original">
   2.  Click **Add New User**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-user-option.png" alt="Add New User option" data-size="original">
   3.  Enter the required data as follows.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-user-screen.png" alt="Add New User screen" data-size="original">

       * **Domain**: `Primary`
       * **Username**: `Alex`
   4. Click **Finish**.
2. To assign login permissions to the user:
   1.  Click the **View Roles** option of Alex.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/view-roles-option.png" alt="View Roles option" data-size="original">
   2.  Click **Permissions**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/role-permissions-option.png" alt="Role Permissions option" data-size="original">
   3.  Select **Login** and click **Update**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/login-permission.png" alt="Login permission" data-size="original">
3. To configure the challenge questions:
   1. Access the User Portal at `https://localhost:9443/user-portal/`.
   2.  Log in with the credentials of the user account that you created.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/userportal-login-screen.png" alt="Sign In form" data-size="original">
   3.  Under **Account Recovery**, click **+** to add or update security questions.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/userportal-account-recovery-option.png" alt="Change Passwrod option" data-size="original">
   4.  Configure the challenge questions as given below:

       * **Challenge Question 1**: `Name of your first pet?`
       * **Your Answer**: `Tom`
       * **Challenge Question 2**: `Favourite food?`
       * **Your Answer**: `Pizza`

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/userportal-account-recovery-form.png" alt="Challenge Question form" data-size="original">
   5. Click **Update**.
   6. Sign out.
4. To mimic a forgotten password:
   1.  On the Sign In screen of the User Portal at `https://localhost:9443/user-portal/`, click **Password**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/forgotten-password-option.png" alt="Sign In form" data-size="original">
   2. Enter the user name as `Alex` and select the **Recover with Security Questions** option.
   3. Click **Submit**.
   4.  Enter the first challenge question answer as `Tom` and click **Submit**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/first-security-question.png" alt="First Security Question" data-size="original">
   5.  Enter the second challenge question answer as `Pizza` and click **Submit**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/second-security-question.png" alt="First Security Question" data-size="original">
   6.  Enter the new password and click **Submit**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/password-reset-form.png" alt="Password Reset form" data-size="original">
   7. Enter the user name and new password and click **Sign In**. The User Portal home screen appears.

