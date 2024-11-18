# User

The Single User and Multi User column types allow you to select a Budibase user from a dropdown list as determined by the Application access. This list will include all admins of the tenant, and any members of the app.

<figure><img src="https://files.readme.io/8db9d38-image.png" alt="Note the slight variation in icons between Single User (team_leader) and Mutli User (team_members) columns"><figcaption><p>Note the slight variation in icons between Single User (team_leader) and Multi User (team_members) columns</p></figcaption></figure>

With the user column added, you can now click the `+` within your data grid view to select a user as identified by their searchable email address. When using a Single User column, if you try to add another user, the current user will be removed.

> ### 👍SQL support
>
> The user column is available for both the Budibase DB and external SQL databases.
>
> In the case of SQL, when a user column is added within Budibase, it will create a varchar column in your table to hold the user id:
>
> ![](https://files.readme.io/2018311-sql.png)

\


***

\


### Usage

A common usage of user columns in your tables is to indicate _ownership_ or _assignment_.

For example, I can add an 'Assignee' user column to a 'Jobs' table to indicate who is responsible for completing that job.

![](https://files.readme.io/30a33d7-Screenshot\_2023-11-03\_at\_13.03.13.png)

When making use of a user column in your [Forms](https://docs.budibase.com/docs/forms), you need to use the [User Field](https://docs.budibase.com/docs/pickers#user-field) component.

\


#### Show current user records

For single user columns, you can filter on the currently logged in user by using the `{{ Current User._id }}` binding.

![](https://files.readme.io/96e4eaf-Screenshot\_2023-11-03\_at\_16.00.31.png)

If your user column has _Allow multiple users_ enabled, then the filter will be a contains like so:

![](https://files.readme.io/c4b0dec-Screenshot\_2023-11-03\_at\_16.05.54.png)\


***

\


### Migrate to user column

> ### 📘
>
> As of **v2.11.0** you will no longer be able to create a relationship between the Users table and a Budibase DB.
>
> To accommodate this change, apps that were created before **v2.11.0** can migrate existing user table relationship fields to the new user column, as outlined below.

1. Open your existing app and navigate to the Budibase table that contains a user relationship
2. Click the hamburger menu, and select the last option: `Migrate to user column`

![](https://files.readme.io/c19c5d3-Screenshot\_2023-11-03\_at\_16.28.38.png)

3. Click `Continue` to create the new user column. As warned, you may need to update some existing bindings

![](https://files.readme.io/10b98fa-Screenshot\_2023-11-03\_at\_16.30.18.png)

<figure><img src="https://files.readme.io/7346683-Screenshot_2023-11-03_at_16.31.17.png" alt="User column added"><figcaption><p>User column added</p></figcaption></figure>

This will remove the link from the **App users** table and can improve the performance of your apps.

It should also be noted that if you were displaying user data through a migrated relationship link, this would also need to be updated as shown in the 'Usage' section above.
