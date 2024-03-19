# Creating Picklists

Picklists are lists of name/key pairs stored as string values. Once defined, you can use the lists with object definitions to populate single or multiple selection fields. See [Picklists](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists) for more picklist use cases.

To create a picklist,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Picklists_.
2.  Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).

    ![Click Add to create a new picklist.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/picklists/creating-picklists/images/01.png)
3. Name the picklist to identify the list in the UI. You can localize the name after creation.
4. Click _Save_ to create an empty picklist definition and generate an external reference code (ERC). You can edit the ERC after creation.
5. Click the new picklist to begin editing it.
6.  Under Items, click _Add_ (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).

    ![Click Add to add a new item to the picklist.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/picklists/creating-picklists/images/02.png)
7.  In the modal window, enter a name and key for the item.

    **Name**: Determines the item’s display name and can be localized after creation.

    **Key**: Determines the standard value understood by applications in the back-end and uses camel case.

    ![Enter a name and key, and then click Save.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/picklists/creating-picklists/images/03.png)
8.  Click _Save_. The picklist updates immediately with the new item and generates an external reference code (ERC) for it.

    You can click the list item to localize its name or edit its ERC, but you cannot change its key. You can remove items at any time.
9.  Repeat the above steps to add additional items to the list.

    ![Add multiple items to a picklist.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/picklists/creating-picklists/images/04.png)

Once created, you can select a picklist when creating object fields. Select the _Picklist_ (single select) or _Multiselect Picklist_ field type and choose the list. See [Adding Fields to Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-fields-to-objects) and [Picklist Fields](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/picklist-fields) for more information.

![Users can select the picklist when creating new object fields.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/picklists/creating-picklists/images/05.png)
