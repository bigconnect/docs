# Introduction to the Admin Account

The Admin User account is created by default in any new installation of MWARE Portal and has full permissions to modify every facet of a Portal instance. The Admin User can create, modify, and delete Sites, Users, pages, and perform all Site administrative tasks.

### Accessing the portal as the Admin User

1. Open a browser to `http://localhost:8080`.
2. Click the _Sign In_ link.
3.  Enter the default admin credentials to log in.

    * Email address: `test@portal.com`
    * Password: `test`

    Note, if you used the setup wizard and set a different admin user email, enter that email address instead. The default password is still `test`.
4. Upon first sign in, you are required to change your password. Enter a new password (e.g. _learn_) and click _Save_.

{% hint style="warning" %}
In a production environment, you should not use the default admin account. Instead, create admin accounts specific to your business. See the steps below.
{% endhint %}

### Configuring the Admin User <a href="#configuring-the-admin-user" id="configuring-the-admin-user"></a>

#### Changing the Login Password <a href="#changing-the-login-password" id="changing-the-login-password"></a>

1. Click the _User Avatar_ and navigate to _Account Settings_ → _Password_.
2.  Enter your _Current Password_ and provide a new password.

    ![Setting a Password](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/introduction-to-the-admin-account/images/02.png)

#### Changing Account Information <a href="#changing-account-information" id="changing-account-information"></a>

1.  Click _User Avatar_ → _Account Settings_.

    ![Account Settings](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/introduction-to-the-admin-account/images/03.png)
2.  In the _Information_ tab, change or add User information as needed.

    ![Account Information](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/introduction-to-the-admin-account/images/04.png)

### Signing Out <a href="#signing-out" id="signing-out"></a>

To log out of your account:

1.  Click _User Avatar_ → _Sign Out_.

    ![Signing Out](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/introduction-to-the-admin-account/images/05.png)

### Creating a New Admin User <a href="#creating-a-new-admin-user" id="creating-a-new-admin-user"></a>

Now that you know how to log in, we strongly recommend that you create a new admin account for everyday use.

To create a new admin account follow these steps:

1.  See configuring mail to set up the portal with your email service. The portal sends an email to the new user’s email address upon account creation.

    To skip this step (for example, if you are just testing the portal), disable the feature to require strangers to verify their email address. Navigate to _Control Panel_ → _Instance Settings_ → _User Authentication_. Uncheck the box for _Require strangers to verify their email address?_. Note, this is not recommended to be disabled for a production environment.

    ![Uncheck require strangers to verify their email address.](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/introduction-to-the-admin-account/images/07.png)
2. Create a new User.&#x20;
3.  Click the _Roles_ link in the left navigation pane (on the Edit User page under General).

    ![Click the Roles link to edit the User's Roles.](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/introduction-to-the-admin-account/images/06.png)
4. Click _Select_ beside _Regular Roles_.
5. Select the _Administrator_ Role from the list (click _Choose_). The dialog box disappears and the Role is added to the list of Roles associated with your account. Scroll to the bottom and click _Save_.
6. (Optional) If you did not configure mail in the step above, set an initial password for this new user. Click the _Password_ link in the left navigation pane. Enter a password for the user, reenter the password, and click _Save_.

The user is now a portal administrator. Log out and then log back in with your new user account.

\
