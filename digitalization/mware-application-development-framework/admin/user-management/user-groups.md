# User groups

User groups allow an **Account Admin** to easily assign and manage users' access to apps.

### Creating a user group

A searchable table of user groups can be found under the _Manage_ section of the Portal.

<figure><img src="https://files.readme.io/f5c1b61dcd8bb90368bad90216a00c15d49df53dfabd8a4390cc74d213be248a-Screenshot_2024-09-09_at_11.16.32.png" alt=""><figcaption></figcaption></figure>

In this view you get a quick summary of the number of users and apps in each user group.

To add a user group, click on `Add group`.

This will pop-up a modal from which you can enter a team name, as well as select an icon and colour.

<figure><img src="https://files.readme.io/4b32798623711bcca22687b4a5a1ea4d87636152bbbffe171b6bb3bd94f921f5-Screenshot_2024-09-09_at_11.17.51.png" alt=""><figcaption></figcaption></figure>

Next you will be brought to screen where you can [Add user](https://docs.budibase.com/docs/user-groups#adding-a-user-to-a-group) and [Add app](https://docs.budibase.com/docs/user-groups#adding-apps-to-a-user-group) for your new group. When finished, the back button will take you back to the table of user groups.

<figure><img src="https://files.readme.io/4eeeb37f8b4335a4d56059acfc4a4e9dc62c12cd6672c43ea2d3da61dd0b59a9-Screenshot_2024-09-09_at_12.13.42.png" alt=""><figcaption></figcaption></figure>

If you want to change the team name, icon or colour, this can be done through the context menu in the top right corner. The user group can also be deleted if you decide you no longer need it.\


<figure><img src="https://files.readme.io/d23e0b58e2bd0b28e680b2f5e180f83e57f4b99ddccc02e6e5265148c968e66f-Screenshot_2024-09-09_at_12.14.47.png" alt=""><figcaption></figcaption></figure>

### Adding a user to a group

When you click `Add user` you will be presented with a searchable list of all the user email addresses. For your convenience, when you select a user it will float to the top of the dropdown, which itself will remain expanded so you can easily click through and add multiple users. When done simply click and empty part of the page.

<figure><img src="https://files.readme.io/c7138f63d9970564ef5880b7b4f678a6ad48ea3d24585b17dabc72223f864009-Screenshot_2024-09-09_at_12.15.31.png" alt=""><figcaption></figcaption></figure>

To remove users, click the entries with the blue tick to unselect them.

Alternatively, you can add a user to a group from the users' detail page, which you can access by clicking on a user within the users list of the group. Alternatively you can select `Users` under the _Manage_ section of the portal, and then click on a user from the users table.

Here you will be able to see the user groups the selected user belongs to:

<figure><img src="https://files.readme.io/97dfa13-Screenshot_2022-10-20_at_15.56.56.png" alt=""><figcaption></figcaption></figure>

To remove access to a group click the `X` beside the group.

To add this user to another group, click `Add to user group`. This will provide a convenient searchable dropdown for selecting and unselecting user groups.

\


<figure><img src="https://files.readme.io/d84f1c357af48988db013a716d16cf4be7ac8f5f295336b8219a3c0ea625856d-Screenshot_2024-09-09_at_12.59.23.png" alt=""><figcaption></figcaption></figure>

### Adding apps to a user group

From the builder, open your app and click on the Users tab.

![](https://files.readme.io/2ae4b5bb9a1a5d0e244ace11d00c93feabb63867ce141a8c8cbcd91128825652-image.png)

This will open up a side panel from which you can add groups or individual users by setting their permission-level for that app.

\


<figure><img src="https://files.readme.io/13d76c0f3c81d7132265756370a10ba08b187e11b0ad8b9df533e8ef7e0fbe39-Screenshot_2024-09-09_at_13.16.18.png" alt=""><figcaption></figcaption></figure>

### Individual roles and group roles

If a user has access to an app, but is also a member of a user group, then the highest permission available will be given.

For example, consider this app:

<figure><img src="https://files.readme.io/36b7420-Screenshot_2022-10-20_at_16.22.09.png" alt=""><figcaption></figcaption></figure>

Harold is a member of the Super group which has _Power_ access to this app. However he is also an _Admin_ user of this app. Therefore the highest role is chosen, and he will have _Admin_ access.

On the other hand, consider the case where Harold has _Basic_ permissions for the app.

<figure><img src="https://files.readme.io/9e33a8e-Screenshot_2022-10-20_at_16.25.42.png" alt=""><figcaption></figcaption></figure>

Because Harold is a member of the Super group, he will be given _Power_ permissions.

