# Icon

The Icon component uses the fantastic Font Awesome icon font, a project by Dave Gandy. Altogether Font Awesome provides more than 1500 symbols and glyphs for web-related actions. These icons are great, because they enable you to easily change color, size, and more via the Design panel. They are scalable vector graphics, which means that they look great on high-resolution displays.

![](https://files.readme.io/6a2b8c2-CleanShot\_2022-03-09\_at\_17.36.27.png)

To add an Icon component, click `Add component` and search for "Icon", or locate it under the _Basic_ section.

<figure><img src="https://files.readme.io/d980cdb-Screenshot_2023-02-07_at_12.32.03.png" alt="Adding an icon component"><figcaption><p>Adding an icon component</p></figcaption></figure>

Within the Settings Panel under the _Icon_ setting, a button will show the currently selected icon, or `Pick icon` if no icon has been selected. Click the button to change the icon.

<figure><img src="https://files.readme.io/6c6f031b435eb4329ada28e9b2394535deb51ec63be868e5e1ce25fe6384514c-Screenshot_2024-09-27_at_08.29.24.png" alt=""><figcaption></figcaption></figure>

This will allow you to filter the Font Awesome library alphabetically or search for keywords.

\


### Using an icon as a button

Icons support the **On Click** event. You can define all of the Actions that a button can.

<figure><img src="https://files.readme.io/c99440d-Screenshot_2023-02-07_at_13.32.58.png" alt="Define click actions in the settings panel"><figcaption><p>Define click actions in the settings panel</p></figcaption></figure>

### Tutorial: Displaying options as icons

In this tutorial, we will create an app with the sample data, and display an icon based on the _Item Tags_ value of the _Inventory_ table. To do this, we will make use of Conditional UI.

1. Add a Data provider for the _Inventory_ table.
2. Add a Repeater nested inside the data provider.
3.  Nest an Icon component within the repeater block. This will display an icon for each tag.

    ![](https://files.readme.io/87670f0ecfcaaf874b73437a4faec7b5426e9843c7c7071c774bdc235125c6d5-Screenshot\_2024-09-27\_at\_13.20.58.png)
4. Select the icon component on the far right of the screen. You should see its settings.
5.  Click the conditions tab

    ![](https://files.readme.io/4ac1a767df466d10155ac91d4de18a9f40a0551d913a6d55bd7dd4840c767346-Screenshot\_2024-09-27\_at\_13.22.56.png)
6. Click the `no conditions set` button, this will open a drawer were you can configure your conditions
7.  Click the `Add condition` button to create your conditions. In our case, we are going to use the update setting option. This will allow us to dynamically change the icon based on the inventory table's tag values. (Use the duplicate button to make your life easier!)



    <figure><img src="https://files.readme.io/3b6a5f9edb4fe862cdad66385041dc4f65b5f9cb67993c051f530d3a27ca1ed3-Screenshot_2024-09-27_at_13.28.30.png" alt=""><figcaption></figcaption></figure>

#### Result

<figure><img src="https://files.readme.io/90f41df23d1055f9501288817de77c654640926d57b48950200f1fc71c0773f7-Screenshot_2024-09-27_at_13.30.28.png" alt=""><figcaption></figcaption></figure>
