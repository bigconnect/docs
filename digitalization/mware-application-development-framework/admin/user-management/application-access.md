# Application access

To access user management in the builder, click on the **Users** button in the header:

![](https://files.readme.io/a6f6b616add1b420ae5e21d5f156f40cbe3ae8868cca75af878f93f0840a876a-Screenshot\_2024-08-29\_at\_14.30.26.png)

**Admin** users can manage user access to their application from within the application builder.

\


### Who has access to my application?

The user management side-bar will display an overview of all the current users and groups in the system that have access to your application.

<figure><img src="https://files.readme.io/21496a3-user_snapshot.png" alt="Users side panel" width="300"><figcaption><p>Users side panel</p></figcaption></figure>

> ### 📘
>
> By default, the first **50** application users will appear in the **Users** section, followed by all members of the **Groups**(🔒).
>
> If you can't see a user in the list, use the search bar at the top.

#### Admins

Admin tenant users have global access to _any_ application. They will always be listed here with **Admin** access and cannot be changed:

<figure><img src="https://files.readme.io/2f85813-admin.png" alt="Admin and developer users will always have the &#x27;Admin&#x27; app role" width="300"><figcaption><p>Admin users will always have the 'Admin' app role</p></figcaption></figure>

#### Creators 🔒

**Creator** users can build and develop some applications, while being an End User for other applications.

This provides fine-grain control, allowing you to give admin like permissions for certain applications to your team, while ensuring they cannot access all your applications.

<figure><img src="https://files.readme.io/b1f3857-Screenshot_2023-09-08_at_09.46.09.png" alt="Application Creator permissions" width="300"><figcaption><p>Application Creator permissions</p></figcaption></figure>

#### Application users

App users only have access to published applications on which they been granted at least **Basic** access permission. You can update an End Users access permissions at any time. If necessary you may also `Remove` their access entirely.

<figure><img src="https://files.readme.io/a732324-app_user.png" alt="Remove user option" width="300"><figcaption><p>Remove user option</p></figcaption></figure>

If you remove an End Users access within the user side-panel, that user will no longer be able to access the application when they log in.

#### User groups&#x20;

Any user groups with access to your application will be displayed along with its total membership count. Updates to the group access level will be applied to all member users. Selecting the `Remove` option in the access dropdown will remove the group from the application and thereby revoke access of any members.

<figure><img src="https://files.readme.io/3c3ccd4-groups.png" alt="Group access levels" width="300"><figcaption><p>Group access levels</p></figcaption></figure>

#### Group user access&#x20;

Users who have been indirectly granted access to an application via membership of a **User group** will appear muted in the **Users** list. Clicking on the access dropdown will outline where they inherited their access from.

<figure><img src="https://files.readme.io/4444d81-groupaccess.png" alt="Note showing the user group that provided this user basic access" width="300"><figcaption><p>Note showing the user group that provided this user basic access</p></figcaption></figure>

Even though the above user has been granted **Basic** access to the application by their group, you can apply an override that will apply only to that app user:

<figure><img src="https://files.readme.io/dee9401-override.png" alt="Overriding the group permission" width="300"><figcaption><p>Overriding the group permission</p></figcaption></figure>

As you can see above, the user `s.donaghy@email.com` was initially only granted **Basic** access to the application as a member of the **Press Team**. Now they have been upgraded to **Power** user.

Removing an app user access override will revert them to their group access level.\


### Finding and inviting users

The access panel will display only users or groups who _currently_ have access to your application. If you wish to add any new users or groups to the application you can use the search bar.

<figure><img src="https://files.readme.io/19397d3-search.png" alt="Searching for groups and users" width="300"><figcaption><p>Searching for groups and users</p></figcaption></figure>

**Users** can be found by entering their _email address_ and **Groups** can be found by entering their _name_.

#### Inviting new users

If your user doesn't appear when searching the system, you will be presented with the option to invite them to your tenant.

<figure><img src="https://files.readme.io/e70041d-invite_user.png" alt="Invite a new user"><figcaption><p>Invite a new user</p></figcaption></figure>

After clicking the `Invite user` button, you will be able to provide the user invite email address, assign them a User role, and an App Access Level.

<figure><img src="https://files.readme.io/53119e2-Add_new_user.png" alt="Add user"><figcaption><p>Add user</p></figcaption></figure>

Click the `Add user` button to send an invite email to the new user. Invitations will remain active for **7 days**, after which they will be deleted. Any invites currently open in the tenant will be listed at the top of the user side-panel in the builder:

<figure><img src="https://files.readme.io/3e32c86-pending_invites.png" alt="Pending invite for Ellen

 







In the time before a user accepts their invitation, you still have the option to update their access level to any application on your tenant." width="300"><figcaption><p>Pending invite for Samantha</p></figcaption></figure>

By default, the system will assign **Basic** access. Before a user accepts their invitation, you still have the option to update their access level to any application on your tenant.

> ### 🚧
>
> Valid SMTP configuration must be available to allow the sending of email invites.

#### Create a user with a temporary password

In the event that the tenant does not have valid SMTP email settings readily available, the fallback behaviour is to create a new user and output temporary credentials.

<figure><img src="https://files.readme.io/9a7767e-nosmtp.png" alt="Adding a user with a temporary password" width="300"><figcaption><p>Adding a user with a temporary password</p></figcaption></figure>

Keep a note of the credentials and relay them to your new user. They will be prompted to change them upon first login.
