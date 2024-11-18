# User roles

At a high level, a user can have access to the Portal as an **Admin**, or simply be a **Member**, i.e. an End User of authorised apps.

Additional fine-tuning of permissions can be achieved through the Creator role which allows a user to be a Builder of some apps, and be restricted to an End User for others.

### Access permission

There are three levels of user role permissions :

* Account Admin - Has full access to all apps and settings in your account
* Creator - Can create and edit apps they have access to
* App User - Can only use published apps they have access to

<figure><img src="https://files.readme.io/e792c609a041c9fbe4e003b0fb7f607855db02ddfc32b0ce4199691e92984919-Screenshot_2024-08-29_at_09.36.17.png" alt="Admin users can add new users"><figcaption><p>Admin users can add new users</p></figcaption></figure>

> ### 📘Publish your application to give users access.
>
> App Users are only be able to view published applications.
>
> The Account Admin role allows the user to manage users and global settings.

### Changing permissions

An **Account Admin** user can edit user permissions within the Portal, by clicking on the `Users` tab.

<figure><img src="https://files.readme.io/7637d3f41e9e48bfbe12d4129106ca4be1416f1ccad72d1a4a25bbe8bbfbc3db-Screenshot_2024-08-29_at_10.00.20.png" alt=""><figcaption></figcaption></figure>

Click on the user you want to change, and then set the _Role_.

<figure><img src="https://files.readme.io/a849bc4e47bc9b02da921260af696a55939e1533bc28f99158b2b05038ef65f2-Screenshot_2024-08-29_at_10.01.03.png" alt=""><figcaption></figcaption></figure>

Once you're happy with your access change, you can close the screen. The permissions save automatically.

### App specific roles

All users in your app without `Account Admin` permission will only see applications you give them access to. But not only can you configure which applications each user can see, you can also configure their role in each application.

There are several roles to choose from, which impact what the user will be able to do and see in your application based on the roles you've defined for each section inside your application.\
Access levels can be set for your each of your screens.

Higher tier roles will be able to see everything lower tiers can see, plus their own role-specific elements. This means, an `Admin` role can see all the pages built for Power, Basic and Public roles.

| Order | Role   | Description                                            |
| ----- | ------ | ------------------------------------------------------ |
| 1     | Public | Can see un-authenticated. Cannot be given to a user    |
| 2     | Basic  | Lowest authenticated role                              |
| 3     | Power  | Has all permissions of Basic and Public, but not Admin |
| 4     | Admin  | Highest authenticated role                             |

> ### 📘Order matters!
>
> The order of user role matters. Higher roles can see everything from lower roles as well as their own.

#### Adding custom roles

The core app roles cannot be deleted, however you can add and remove any number of custom roles if you need more granular access control.

Within your app, navigate to the Users table and click on _Edit roles_.

<figure><img src="https://files.readme.io/5591bd9-Screenshot_2022-10-13_at_08.43.00.png" alt=""><figcaption></figcaption></figure>

A modal will pop up to enter data into. Select _Create new role_.

<figure><img src="https://files.readme.io/034350a-Screenshot_2022-10-13_at_11.22.30.png" alt=""><figcaption></figcaption></figure>

**Inherits Role** determines the rank of your new role. In this case, a _Super_ user will be able to access all screens of access level Super, Power, Basic and Public, but will not be able to access Admin app screens.

**Base Permissions** determines the read/write permissions of the role. Options include:

| Order | Role       | Description                                                                                     |
| ----- | ---------- | ----------------------------------------------------------------------------------------------- |
| 1     | Public     | <ul><li>No permissions</li></ul>                                                                |
| 2     | Read Only  | <ul><li>Read data from queries, tables and views</li></ul>                                      |
| 3     | Read/Write | <ul><li>Read</li><li>Write to tables</li><li>POST queries</li><li>Trigger automations</li></ul> |
| 4     | Power      | <ul><li>Read/Write</li><li>Read from the Users table</li></ul>                                  |
| 5     | Admin      | <ul><li>Read/Write</li><li>Perform admin functions</li></ul>                                    |
