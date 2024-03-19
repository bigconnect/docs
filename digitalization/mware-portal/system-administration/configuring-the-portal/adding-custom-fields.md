# Adding Custom Fields

Many of Liferay’s assets and resources can be extended with custom fields. Adding custom fields can give you greater control and flexibility over your Site. Here are all the different places custom fields can be used:

* Blogs Entry
* Calendar Event
* Document
* Documents Folder
* Knowledge Base Article
* Knowledge Base Folder
* Message Boards Category
* Message Boards Message
* Organization
* Page
* Role
* Site
* Site Navigation Menu Item
* User
* User Group
* Web Content Article
* Web Content Folder
* Wiki Page

There are many reasons for using custom fields. Here’s one example.

**Max Role Use Case**: Suppose you want to limit the number of users that can be assigned to a particular Role on your Site. To accomplish this, you could create a _max-users_ custom field for the Role. Then ask your developer to create logic to keep track and prevent more Users to be added once the set limit was reached.

### [Adding Custom Fields](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/adding-custom-fields#adding-custom-fields-1) <a href="#adding-custom-fields-1" id="adding-custom-fields-1"></a>

Follow these steps to see how a custom field can be created for the example use case above:

1.  Navigate to Control Panel → Configuration → Custom Fields.

    ![Navigate to Custom Fields in the Configuration section of Control Panel.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/adding-custom-fields/images/01.png)
2. Scroll down and click on the link for _Role_.
3. Click the _Add_ icon (![Click on the add icon to add a custom field.](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to add a custom field.
4.  There are several different field types you can choose.

    ![There are different field types you can choose from.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/adding-custom-fields/images/02.png)

    * **Text and Numbers**
      * **Text Area**: This field type can collect longer lines of text.
      * **Input Field**: This field type can collect a string of text, a decimal number, or an integer.
    * **Selection**
      * **Dropdown**: This field type can provide a drop-down list of items to select. The list can be text, decimal numbers, or integers.
      * **Checkbox**: This field type shows a checkbox.
      * **Radio**: This field type shows a selection of two or more radio buttons.
    * **Others**
      * **Geolocation**: This field type collects geolocation information.
      * **Date**: This field type collects date and time information.
      * **True/False**: This field type shows a true or false choice.
5.  For the above use case, select _Input Field_ as the field type. Input a _Field Name_ and select the Data Type as Integer.

    ![Give your custom field a name you can remember, because developers must access it using this name.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/adding-custom-fields/images/03.png)

    Click the _Save_ button to save your changes.

    NOTE

    The Field Name is the identifier stored in your database and the name that is used by the developer to access the custom field.

Once you have created a custom field, you can find it in the UI of the resource you created it for. Navigate to the resource, and the custom field is displayed in the UI.

For example, the _max-users_ custom field is now visible for all Roles:

1. Navigate to Control Panel → Users → Roles.
2.  Click the Options icon (![Click on the options icon to add a custom field.](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) for a Role and click Edit.

    ![The custom field is not visible in the resource UI.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/adding-custom-fields/images/04.png)

    Notice the _Max Users_ field at the bottom.

### [Editing Custom Fields](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/adding-custom-fields#editing-custom-fields) <a href="#editing-custom-fields" id="editing-custom-fields"></a>

Click the Options icon (![Click on the options icon for the custom field.](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) next to the name of the custom field to edit. These options appear:

![Clicking on the options icon gives you different choices.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/adding-custom-fields/images/05.png)

**Edit:** You can change different aspects of the custom field, but note that you cannot change the custom field name or custom field type.

![Click Edit to open a new window to make changes to the custom field.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/adding-custom-fields/images/06.png)

The configuration options you have will also depend on the field type you choose.

**Permissions:** You can modify the permissions to allow other users to edit or view a custom field.

**Delete:** You can delete the custom field.

\
