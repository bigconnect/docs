# Account Disabling

MWARE IAM enables the privileged users to disable user accounts for longer durations. These disabled user accounts can only be unlocked by privileged users.

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. **Sam** is an administrator at Pickup. **Larry** is an employee who had served Pickup for 7 years and is entitled for sabbatical leave. As Larry is going on sabbatical leave for 2 years, Sam wants to disable Larry's account.

Let's learn how Sam can disable Larry's user account!

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure account disabling in MWARE IAM.

1. Open the `deployment.toml` file in the `<IAM_HOME>/repository/conf` directory.
   1.  Check whether the following listener configs are in place:

       ```toml
       [event.default_listener.identity_mgt]
       priority= "50"
       enable = false
       [event.default_listener.governance_identity_mgt]
       priority= "95"
       enable = true
       ```
   2.  To configure the email server to send emails that informs about the disabled account to the user, add the following configurations:

       * **from\_address**: This is the email address from which the confirmation email will be sent.
       * **username**: This is the username of the given email address.
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
   2.  Under the **Account Management** section, click **Account Disable**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-disabling-option.png" alt="Account Disabling Option" data-size="original">
   3.  Select the **Enable Account Disabling** check box.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-disabling-form.png" alt="Account Disabling form" data-size="original">
   4. Click **Update**.
4. To enable the account disabling claim:
   1.  On the **Main** menu of the Management Console, click **Identity > Claims > List**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/claims-list-menu-item.png" alt="Claims List option" data-size="original">
   2.  Click **http://wso2.org/claims**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/claim-list-wso2.png" alt="WSO2 claim dialect" data-size="original">
   3.  Under **Account Disabled**, click **Edit**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-disabled-edit.png" alt="Account Disabled claim edit option" data-size="original">
   4.  Select **Supported by Default**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-disabled-claim-supported-by-default.png" alt="Account Disabled claim&#x27;s Suppported by Default option" data-size="original">
   5. Click **Update**.

### Try out <a href="#try-out" id="try-out"></a>

1. To create the user account for Larry:
   1.  On the **Main** menu of the Management Console, click **Identity > Users and Roles > Add**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-users-and-roles-menu-item.png" alt="Add Users and Roles menu-item" data-size="original">
   2.  Click **Add New User**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-user-option.png" alt="Add New User option" data-size="original">
   3.  Enter the required data as follows.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/add-new-user-screen.png" alt="Add New User screen" data-size="original">

       * **Domain**: `Primary`
       * **Username**: `Larry`
   4. Click **Finish**.
2. To assign login permissions to the user:
   1.  Click the **View Roles** option of Larry.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/view-roles-option.png" alt="View Roles option" data-size="original">
   2.  Click **Permissions**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/role-permissions-option.png" alt="Role Permissions option" data-size="original">
   3.  Select **Login** and click **Update**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/login-permission.png" alt="Login permission" data-size="original">
3. To disable Larry's user account:
   1.  Click **User Profile** option of Larry.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-profile-option.png" alt="User Profile option" data-size="original">
   2.  Enter an email address to which Larry's account disabling emails will be sent and select the **Account Disabled** check box.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-disabled.png" alt="User Disabled option" data-size="original">
   3. Click **Update**.
   4. An email that informs about the disabled account is sent to the given email address.
   5.  Access **My Account** at `https://localhost:9443/myaccount`.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/userportal-login-screen.png" alt="Sign In form" data-size="original">
   6. Try logging in with Larry's credentials. Note that an error message appears.
4. To revoke the account disabling:
   1. Click **User Profile** option of Larry.
   2. Unselect the **Account Disabled** check box.
   3. Click **Update**.
   4. An email that informs about the enabled account is sent to the given email address.
   5. Trying logging in to the **User Portal** with Larry's credentials.
