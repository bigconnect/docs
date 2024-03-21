# Account Users

Once you’ve created an account, you can associate existing users with it manually. Alternatively, you can create and associate users with an account simultaneously using the Account Users application. Here you can also manage all users associated with accounts.

NOTE

For version 7.4 and earlier versions, you can only create users for accounts via the Account Users application. You cannot associate users created in the Users and Organizations application with accounts.

### Adding Account Users <a href="#adding-account-users" id="adding-account-users"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Account Users_.
2. Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3. Select an _account_ to associate with a user.
4.  Fill in the necessary information for this new user.

    ![Fill in the account user's information.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/account-users/images/01.png)
5. Click _Save_.

#### Setting a Password for an Account User <a href="#setting-a-password-for-an-account-user" id="setting-a-password-for-an-account-user"></a>

When you create a user account, the portal generates a password and sends it to the user via email using the configured mail server.

To set user passwords manually,

1. In Account Users, click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired user and select _Edit_.
2. In the General tab, click _Password_.
3. Enter a password.
4. Determine whether to require password reset.
5. Click _Save_.

#### Associating an Existing User to a Business Account <a href="#associating-an-existing-user-to-a-business-account" id="associating-an-existing-user-to-a-business-account"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Accounts_.
2. Pick an account to edit and click its _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Manage Users_.
3. Click _Add_ and select a user to assign to the account.

#### Associating an Existing User to a Personal Account <a href="#associating-an-existing-user-to-a-personal-account" id="associating-an-existing-user-to-a-personal-account"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Accounts_.
2. Pick an account to edit and click its _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Edit_.
3. Under the User section, click _Assign_ and select a user. Click _Choose_.

### Inviting a New User by Email <a href="#inviting-a-new-user-by-email" id="inviting-a-new-user-by-email"></a>

Available 7.4 +

You can invite users to an account by email. The portal sends an automated invitation email to the person you specify to create a new user and be associated with an account.

1. Make sure you have already configured mail for your portal instance.
2. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Accounts_.
3. Select the _account_ you want to invite users to and go to the _Users_ tab.
4.  Click _New_ and select _Invite Users_.

    NOTE

    Version 2023.Q4+`A new permission called`Invite User`was added at the Account Entry level to separate the ability to invite users and assign them roles. A user with only the`Invite User permission cannot view the _Assign Users_ option after clicking the _New_ button. A user with the `Manage Users` permission can view both options.
5. Enter the email address of the person you want to invite and press enter. If desired, you can add multiple addresses.
6.  (Optional) Enter and select the account roles you want to assign to the user (e.g., Account Administrator, Buyer, Order Manager, etc.).

    TIP

    The portal applies selected roles to all emails in the invite entry. If desired, you can click _Add Entry_ to create additional invite entries for assigning different roles to emails.
7.  Click _Invite_.

    ![Input an email address and role for the new user.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/account-users/images/02.png)

The portal sends an invitation to the included email addresses. Each recipient can click Create Account in the email to register for a new user on the portal instance. Upon creation, the user is associated with the account and has the assigned account role.

![The person receives an email invitation to create a new user account.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/account-users/images/03.png)

### Editing an Account User <a href="#editing-an-account-user" id="editing-an-account-user"></a>

1. In Account Users, pick a user and click its _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)).
2. Click _Edit_.

The following options are available:

| General Tab | Description                                     |
| ----------- | ----------------------------------------------- |
| Information | Edit or change the user’s personal information. |
| Accounts    | View or modify the user’s associated accounts.  |
| Password    | Add or change the user’s password.              |

| Contact Tab         | Description                                  |
| ------------------- | -------------------------------------------- |
| Addresses           | Add or change the user’s addresses.          |
| Contact Information | Add or change the user’s contact information |

| Preferences Tab                  | Description                                        |
| -------------------------------- | -------------------------------------------------- |
| Alert and Announcements Delivery | Change the user’s notification settings.           |
| Display Settings                 | Change the user’s time zone and greeting settings. |

### Deactivating and Deleting an Account User <a href="#deactivating-and-deleting-an-account-user" id="deactivating-and-deleting-an-account-user"></a>

Deleting an account user is a two-step process that involves deactivating the user and then deleting it. This is because you may want to deactivate a user and still have time to review its data before deletion (e.g., due to legal requirements).

#### Deactivating an Account User <a href="#deactivating-an-account-user" id="deactivating-an-account-user"></a>

Deactivating prevents the account user from logging in, but preserves the user’s data and information.

1. In Account Users, pick a user and click its _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)).
2. Click _Deactivate_ and click _OK_ to confirm your change.

The account user is now deactivated.

To see a deactivated user, click the _Filter and Order_ drop-down menu and select _Inactive_ under Filter by Status. You can click a user’s _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and click _Activate_ to reactivate it.

#### Deleting an Account User <a href="#deleting-an-account-user" id="deleting-an-account-user"></a>

1. In Account Users, click the _Filter and Order_ drop-down menu and click _Inactive_ under Filter by Status.
2.  Click the inactive user’s _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Delete_

    ![Select the inactive user and click Delete.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/account-users/images/04.png)
3. Click _OK_ on the pop-up form to confirm your decision. The account user is now deleted.

### Impersonating an Account User <a href="#impersonating-an-account-user" id="impersonating-an-account-user"></a>

You can impersonate account users to view the system as they would see it. This can be used to diagnose permission issues an administrator can’t see, such as making sure a user doesn’t have access to restricted data. Only users with this permission can impersonate a different user.

1. In Account Users, click the user’s _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)).
2. Select _Impersonate User_.

A new window opens with you logged in as the selected account user.

\
