# Username Recovery

MWARE IAM enables recovering user accounts where the user has forgotten the username. This is done by matching the user claims that are related to user attributes. The user will be prompted to enter values for these user attributes. If the value entered by the user matches with the claims, the corresponding username will be emailed to the user’s registered email ID.

### Scenario <a href="#scenario" id="scenario"></a>

**Pickup** is a cab company that has many employees who use different credentials to sign in to different internal enterprise applications. **Larry** who is a new recruit at Pickup has forgotten the username.

Let's learn how Larry can recover the password by providing the corresponding profile information!

### Set up <a href="#set-up" id="set-up"></a>

Follow the steps below to configure MWARE IAM Server to enable username recovery.

1. Open the `deployment.toml` file in the `<IS_HOME>/repository/conf` directory.
   1.  Check whether the following listener configs are in place.

       ```
       [event.default_listener.identity_mgt]
       priority= "50"
       enable = false
       [event.default_listener.governance_identity_mgt]
       priority= "95"
       enable = true
       ```
   2.  To configure the email server to send emails requesting password reset, add the following configurations.

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
3.  On the **Main** menu of the Management Console, click **Identity > Identity Providers > Resident**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/resident-menu-item.png" alt="Resident menu-item" data-size="original">
4.  Under the **Account Management** section, click **Account Recovery**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/account-recovery-option.png" alt="Account Recovery Option" data-size="original">
5.  Select the following check boxes:

    * **Username recovery**
    * **Manage notifications sending internally**

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-name-recovery-options.png" alt="User Name Recover options" data-size="original">
6. Click **Update**.

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
3. To add Larry's email Id:
   1.  Click **User Profile** option of Larry.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-profile-option.png" alt="User Profile option" data-size="original">
   2.  Enter an email address to which Larry's password recovery emails will be sent. Enter the first name as `Larry`.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/user-email.png" alt="User Email option" data-size="original">
   3. Click **Update**.
4. To mimic a forgotten username:
   1.  On the Sign In screen of My Account at `https://localhost:9443/myaccount`, click **Username**.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/forgotten-user-name-option.png" alt="Sign In form" data-size="original">
   2.  Enter the first name as `Larry`.

       <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/recover-user-name-screen.png" alt="Consent form" data-size="original">
   3. Click **Submit**.
   4. An email with the username is sent to the given email address.
