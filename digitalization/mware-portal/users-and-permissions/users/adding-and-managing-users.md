# Adding and Managing Users

Core user management activities include adding, editing, and deleting users. These activities are typically restricted to Administrative users.

### Adding Users <a href="#adding-users" id="adding-users"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Users and Organizations_.
2.  In the Users tab, click the _New_ button.

    ![Add users from the Users and Organizations section of the control panel.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/users/adding-and-managing-users/images/01.png)
3.  Fill out the Add User form and click _Save_. At a minimum, provide a screen name, first name, last name, and email address for the user.

    NOTE

    Screen names and email addresses are not interchangeable. A screen name cannot contain an `@` symbol because it is used in the URL to a user’s pages.

    The add user functionality is split over several independent forms. Save the first form to create the user, and then you’ll see a success message saying `Success. Your request completed successfully.`

You can add additional information after creating the user.

#### Setting Passwords for New Users <a href="#setting-passwords-for-new-users" id="setting-passwords-for-new-users"></a>

When you create users, the portal generates their passwords. If a mail server was set up, the portal sends an email message with the users’ new passwords.

If you haven’t set up a mail server, click the _Password_ item from the general menu and manually set a password for your new user. Enter the new password twice.

![Enter the password twice to manually set the password for a user. If the password policy you're using is configured to allow it, select whether to require the user to reset their password the first time they sign in to the portal.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/users/adding-and-managing-users/images/03.png)

### Editing Users <a href="#editing-users" id="editing-users"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Users and Organizations_.
2.  Click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to a user.

    The following options are available:

    | Option               | Description                                                     |
    | -------------------- | --------------------------------------------------------------- |
    | Edit                 | Modify any aspect of the User account.                          |
    | Permissions          | Define which Roles have permissions to edit the User.           |
    | Manage Pages         | Configure the personal pages of a User.                         |
    | Impersonate User     | Browse the Site in another window as though you were that User. |
    | Deactivate           | Disable the user’s account.                                     |
    | Erase Personal Data  | Delete the User’s personal data.                                |
    | Export Personal Data | Download the User’s personal data.                              |

### Deactivating and Deleting Users <a href="#deactivating-and-deleting-users" id="deactivating-and-deleting-users"></a>

Removing users is a two-step process for several reasons:

* You or the user may decide you really wanted that account.
* Users are often tied to content with possible legal ramifications in your country.
* You may need time to review a user’s account activity before removing all evidence of that user’s existence.

For these and more reasons, you must first _deactivate_ a user before _deleting_ them.

NOTE

A user with the `Delete` permission can activate, deactivate, and delete users. Starting from version 2023.Q4+, two new permissions, `Activate` and `Deactivate`, have been added to give more granular control over who can activate and deactivate other users.

#### Deactivating Users <a href="#deactivating-users" id="deactivating-users"></a>

Deactivating a user prevents the user from logging in, but preserves the user. You can toggle between active and inactive users in the users view. If all the users are active, this filtering option doesn’t appear.

To deactivate a user,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Users and Organizations_.
2. Locate the user to deactivate and click the _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) button next to the user.
3. Select _Deactivate_.

The user is now deactivated and can no longer log in. Users can be reactivated by finding them in the users table (be sure you’re filtering the table results by deactivated users), clicking _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)), and selecting _Activate_. There’s no confirmation window for activation: users are automatically restored to their former status after activation.

#### Deleting Users <a href="#deleting-users" id="deleting-users"></a>

To guard against accidental deletion of users, users must be deactivated first, and then can be deleted.

1. Follow the steps above to deactivate the user.
2.  Click _Filter and order_ at the top of the table to display a selector. Click _Inactive_ to view the user you just deactivated.

    ![To delete users, first filter to show inactive or deactivated users.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/users/adding-and-managing-users/images/05.png)
3. Click the _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) button, and click _Delete_ to delete the user.

The user is now deleted. There is no way to recover the user once deleted besides restoring from a prior backup.

### Impersonating Users <a href="#impersonating-users" id="impersonating-users"></a>

You can impersonate users to view the system as the user would see it. This helps to diagnose permission issues an administrator can’t see, such as making sure a user doesn’t have access to sensitive data. Only users with the administrator role can impersonate.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Users and Organizations_.
2. Locate the user to impersonate and click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the user.
3. Click _Impersonate User_.

This opens another browser window logged in as that user.

### Resetting a User Password <a href="#resetting-a-user-password" id="resetting-a-user-password"></a>

The Add User functionality includes a _Require Password Reset_ check box at the bottom of the password form. The default password policy prevents administrators from de-selecting this option. You can, however, modify the default password policy to make this box usable.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _Password Policies_.
2. Click on the _Default Password Policy_.
3. De-select the _Change Required_ switcher in the _Password Changes_ section. Now you can decide whether users you add must reset their passwords.

See Password Policies for more information on editing the default policy or creating your own.

\
