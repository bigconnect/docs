# Navigation

The navigation section of the design tab allows you position and style the app navigation bar as well as the available navigation links.

### Settings

Click on the navigation icon in the Component Tree to access the navigation settings.

<figure><img src="https://files.readme.io/e28f987-nav.png" alt=""><figcaption></figcaption></figure>

The list of properties that can be changed for the navigation bar are displayed in the table below.

| Name             | Description                                                                                                                             | Visual                                                                        |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Position         | Will anchor the nav bar to the top or left hand side of the page.                                                                       | ![](https://files.readme.io/24f5251-Screenshot\_2022-07-08\_at\_14.36.28.png) |
| Sticky header    | <p><em>Top anchored nav bar only.</em><br><br>When scrolling down the page, the nav bar will remain visible at the top of the page.</p> | ![](https://files.readme.io/83fcab0-Screenshot\_2022-07-08\_at\_14.36.42.png) |
| Width            | <p><em>Top anchored nav bar only.</em><br><br>The width of the nav bar content.</p>                                                     | ![](https://files.readme.io/29ba9a2-Screenshot\_2022-07-08\_at\_14.38.55.png) |
| Title            | Show/Hide the specified nav bar title.                                                                                                  | ![](https://files.readme.io/0baf7c1-Screenshot\_2022-07-08\_at\_14.37.45.png) |
| Background color | The background color of the nav bar.                                                                                                    | ![](https://files.readme.io/8f10888-Screenshot\_2022-07-08\_at\_14.38.13.png) |
| Text color       | The color of the nav bar title and links.                                                                                               | ![](https://files.readme.io/4a55201-Screenshot\_2022-07-08\_at\_14.38.37.png) |

<figure><img src="https://files.readme.io/3a5625a60af07e88f437cffff7d08c616b94deb8adf0f83a4fd0a11aadb93bb1-Screen_Recording_2024-11-05_at_13.36.54.gif" alt=""><figcaption></figcaption></figure>

By default the navigation bar is visible for all Screens, however it can be turned on a per screen basis.

<figure><img src="https://files.readme.io/8f7fb9284da29fdcce5b54993c20e70fea1f5d839a57c0ba4a6cef44d8c6cc2e-Screenshot_2024-08-28_at_08.46.37.png" alt=""><figcaption></figcaption></figure>

### Configure links

The links shown in your navigation bar can be configured in the navigation panel by clicking on the 'Navigation' icon in the Component Tree and then clicking the add new nav item button in the Settings Panel.

<figure><img src="https://files.readme.io/646cde9-Screenshot_2024-04-16_at_12.11.01.png" alt=""><figcaption></figcaption></figure>

This will add a new navigation item under any existing ones. This will default to the name `Nav Item`. With basic role permissions attached to the navigation item.

You can customise various elements such as the text, link (External links or linking to internal screens), role permissions, or deciding whether a nav item is a single link or opens up a modal which contains sub-links. These can all be modified by clicking on the specific nav item you wish to amend.

![](https://files.readme.io/81a899b-Screenshot\_2024-04-16\_at\_12.24.23.png)

#### Sub links

If you select `Open sub links` as the option for nav item. The link field will be changed to a button. When clicked this will open a drawer that allows you to add, update and delete sub navigation links related to the parent nav item.

\


<figure><img src="https://files.readme.io/af2ddf2-Screenshot_2024-04-16_at_12.48.11.png" alt=""><figcaption></figcaption></figure>

You can begin adding sub-links by clicking the `+ Add link` button. Feel free to add as many sub-links as you need, each with its own name. Just like the parent navigation items, these sub-links can direct users externally (e.g., to Google.com) or internally within your app. Additionally, the role permissions for these sub-links automatically inherit those of the parent navigation item.

<figure><img src="https://files.readme.io/30d8c14-Screenshot_2024-04-16_at_12.54.08.png" alt=""><figcaption></figcaption></figure>

### Logo

By default the navigation bar will show the default logo, but there are a number of settings available to customise this under the _Logo_ section of the navigation settings.

<figure><img src="https://files.readme.io/8ecebe2-default_logo_settings.png" alt="Default logo settings"><figcaption><p>Default logo settings</p></figcaption></figure>

| Name           | Description                                                                                                              |
| -------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Show logo      | Toggle off to hide the logo from the nav-bar.                                                                            |
| Logo image URL | Allows you to replace the logo with a custom image.                                                                      |
| Logo link URL  | <p>If provided, turns your logo into a hyperlink.<br>This can be a screen (such as Home), or to an external webpage.</p> |
| New tab        | Logo hyperlinks will be opened in a new tab.                                                                             |
