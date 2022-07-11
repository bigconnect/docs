# Account Locking

MWARE IAM enables locking user accounts using the following methods:

* [Admin-Initiated Account Locking](account-locking.md#configuring-admin-initiated-account-locking)
* [Account Locking due to Failed Login Attempts](account-locking.md#configuring-account-locking-due-to-failed-login-attempts)

## Admin-Initiated <a href="#configuring-admin-initiated-account-locking" id="configuring-admin-initiated-account-locking"></a>

MWARE IAM enables the privileged users to temporarily lock suspicious user accounts and prevent the users from logging in. These locked user accounts can only be unlocked by privileged users.

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. While **Sam** is an administrator at Pickup, **Alex** is a new recruit. Due to suspicious user activity, Sam wants to lock Alex's account.

Let's learn how Sam can lock Alex's user account!

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure admin-initiated account locking in MWARE IAM.

1. Open the `deployment.toml` file in the `<IAM_HOME>/repository/conf` directory.
   1.  Check whether the following listener configs are in place.

       ```
       [event.default_listener.identity_mgt]
       priority= "50"
       enable = false
       [event.default_listener.governance_identity_mgt]
       priority= "95"
       enable = true
       ```
   2.  To configure the email server to send emails requesting password reset, add the following configurations:

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
3. To configure the account locking requirements:
   1.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident menu-item" data-size="original">
   2.  Under the **Login Policies** section, click **Account Locking**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-locking-option.png" alt="Account Locking Option" data-size="original">
   3.  Select the **Account Lock Enabled** check box.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-locking-form-admin-initiated.png" alt="Account Locking form" data-size="original">
   4. Click **Update**.
4. To enable the account locking claim:
   1.  On the **Main** menu of the Management Console, click **Identity > Claims > List**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/claims-list-menu-item.png" alt="Claims List option" data-size="original">
   2.  Click **http://wso2.org/claims**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/claim-list-wso2.png" alt="WSO2 claim dialect" data-size="original">
   3.  Under **Account Locked**, click **Edit**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-lock-claim-edit.png" alt="Account Locked claim edit option" data-size="original">
   4.  Select **Supported by Default**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-locked-claim-supported-by-default.png" alt="Account Locked claim&#x27;s Suppported by Default option" data-size="original">
   5. Click **Update**.

### Try out <a href="#try-out" id="try-out"></a>

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
3. To lock Alex's user account:
   1.  Click **User Profile** option of Alex.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-profile-option.png" alt="User Profile option" data-size="original">
   2.  Enter an email address to which Alex's account locking emails will be sent and select the **User Locked** check box.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-locked.png" alt="User Email option" data-size="original">
   3. Click **Update**.
   4. An email that informs about the account locking is sent to the given email address.
   5.  Access **My Account** at `https://localhost:9443/myaccount`.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/userportal-login-screen.png" alt="Sign In form" data-size="original">
   6. Try logging in with Alex's credentials. Note that an error message appears.
   7. Wait for 15 minutes and try to log in again. The **My Account** home screen appears.
4. To unlock Alex's user account:
   1. Click **User Profile** option of Alex.
   2. Unselect the **User Locked** check box.
   3. Click **Update**
   4. An email that informs about the account unlocking is sent to the given email address.
   5. Try logging in to **My Account** with Alex's credentials. The **My Account** home screen appears.

## Failed Login Attempts <a href="#configuring-account-locking-due-to-failed-login-attempts" id="configuring-account-locking-due-to-failed-login-attempts"></a>

MWARE IAM enables privileged users to define the maximum number of failed login attempts the system accepts. When a user exceeds the maximum number of failed login attempts defined in the system, the user account will be automatically locked. The privileged users can also define for how long the account should be locked.

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. While **Sam** is an administrator at Pickup, **Alex** is a new recruit. To prevent unauthorized access, Sam wants to temporarily lock accounts to which more than 3 failed login attempts were made.

Let's learn how Sam implements this!

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure account locking due to failed login attempts.

1. Open the `deployment.toml` file in the `<IAM_HOME>/repository/conf` directory.
   1.  To request password entry from the users who are registered via Management Console, add the following configurations:

       ```toml
       [event.default_listener.identity_mgt]
       priority= "50"
       enable = false
       [event.default_listener.governance_identity_mgt]
       priority= "95"
       enable = true
       ```
   2.  To configure the email server to send emails requesting password entry, add the following configurations:

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
3. To configure the account locking requirements:
   1.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident menu-item" data-size="original">
   2.  Under the **Login Policies** section, click **Account Locking**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-locking-option.png" alt="Account Locking Option" data-size="original">
   3.  Enter the required information as given below:

       * **Maximum Failed Login Attempts**: `3`
       * **Account Unlock Time**: `15`

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-locking-form-failed-attempts.png" alt="Account Locking form" data-size="original">
   4. Click **Update**.

### Try out <a href="#try-out" id="try-out"></a>

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
3. To configure Alex's email account:
   1.  Click **User Profile** option of Alex.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-profile-option.png" alt="User Profile option" data-size="original">
   2.  Enter an email address to which Alex's account locking emails should be sent.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-email.png" alt="User Email option" data-size="original">
   3. Click **Update**.
4. To mimic account locking:
   1.  Access **My Account** at `https://localhost:9443/myaccount/`.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/userportal-login-screen.png" alt="Sign In form" data-size="original">
   2. To mimic three consecutive erroneous login attempts, log in with Alex's user name and the following as passwords sequentially:
      * `test123`
      * `test234`
      * `test345`
   3. An email that informs about the account locking is sent to the given email address.
   4. Wait for 15 minutes and try to log in again with the correct credentials. The **User Portal** home screen appears.
