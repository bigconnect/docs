# Exporting and Importing Object Definitions

You can export and import object definitions as `.json` files. These files include the definition’s complete schema (e.g., configurations, fields, actions, and relationships).

When exporting definitions with relationships, the JSON file includes all relationships defined in the current object, as well as many-to-many relationships defined in other objects. However, the file does not include one-to-many relationships defined in other objects.

If an imported definition references picklists or objects that do not exist, Liferay automatically generates them using the external reference codes provided in the JSON file.

### [Exporting Definitions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/exporting-and-importing-object-definitions#exporting-definitions) <a href="#exporting-definitions" id="exporting-definitions"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2.  Click the _Actions_ button (![Action Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired object and select _Export as JSON_.

    ![Click the Actions button for the desired object and select Export as JSON.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/exporting-and-importing-object-definitions/images/01.png)
3. Click _Save_ in the dialog window to download the object definition as a JSON file.

Once exported, you can import the object definition into compatible Liferay environments.

### [Importing Definitions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/exporting-and-importing-object-definitions#importing-definitions) <a href="#importing-definitions" id="importing-definitions"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar and select _Import Object_.

    ![Click the Actions button in the Application Bar and select Import Object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/exporting-and-importing-object-definitions/images/02.png)
3.  Enter a name for the object. You can edit this value after importing.

    IMPORTANT

    Object names must be unique and use PascalCase.
4.  Select a JSON file to import.

    Once selected, the modal window displays the definition’s external reference code (ERC). You can edit the ERC after importing.

    ![Enter a name and select the desired JSON file.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/exporting-and-importing-object-definitions/images/03.png)
5. Click _Import_.

The object definition begins importing in the background and can take a few minutes.

\
