# Custom Fields

Many of MWARE Portal’s assets and resources let you add new fields to their edit forms. Here’s the complete list:

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
* User
* User Group
* Web Content Article
* Web Content Folder
* Wiki Page

**Developer Use Case:** Adding custom fields to MWARE Porta resources affords flexibility to developers. Suppose you must limit the number of users that can be assigned to a particular Role. First an administrator creates a custom field called _max-users_ for the Role. A developer then creates a module that inserts logic before a user is added to that Role. If the logic detects that the maximum number of Role users would be exceeded by completing the action, an exception is thrown and the action does not complete.

### Adding Custom Fields <a href="#adding-custom-fields" id="adding-custom-fields"></a>

To add custom fields, find the Custom Fields entry beneath the Control Panel’s Configuration heading.

To add a custom field to one of the listed entities,

1. Choose a resource by clicking on it.
2. Click the add (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030156611/icon-add.png)) button.
3. Choose a field type: Text Area, Input Field, Dropdown, Checkbox, Radio, Geolocation, Date, True/False.
4.  Add a name that’s used as a key for accessing the field programmatically.

    **Note:** The Key you enter here is the name of the new field. It’s stored in the database and used by developers to access the custom field with the `<liferay-ui:custom-attribute />` tag. It is also used to label the field in the UI.
5.  Choose the Data Type of field and set any advanced properties.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030187272/custom-fields-user-head-circumference.png" alt="Figure 1: At The Lunar Resort, a Head Circumference field is necessary for all users." data-size="original">

    Figure 1: At The Lunar Resort, a Head Circumference field is necessary for all users.
6. Click Save.

That’s it.

Once you have a custom field for a resource, go find it in the UI of the resource. First find the UI location for the resource, and all its custom fields are displayed in a Custom Fields panel. For example, consider the Users UI:

Navigate to _Control Panel → Users and Organizations_. Click on a User to open the Edit User form and scroll down to find your custom field.

![Figure 2: The Custom Fields panel is found at the bottom of the Edit User form.](https://liferay-support.zendesk.com/hc/article\_attachments/360030156591/custom-fields-panel.png)

Figure 2: The Custom Fields panel is found at the bottom of the Edit User form.

You can also leverage your custom field in Content Pages and Display Page Templates. See Editable Elements for more information.

### Editing a Custom Field <a href="#editing-a-custom-field" id="editing-a-custom-field"></a>

You can’t change the key or field type of a custom field, but you can delete it and create a new one if necessary.

![Figure 3: The exact Custom Fields configuration options you use depend on the field type you choose.](https://liferay-support.zendesk.com/hc/article\_attachments/360030187292/custom-fields-configuration.png)

Edit an individual custom field’s permissions by clicking the field’s kebab menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030156571/icon-actions.png)), then _Permissions_. Permission can be granted or removed for these actions:

* Delete
* Permissions
* Update
* View

![Figure 4: You can delete a custom field, edit it, or configure its permissions.](https://liferay-support.zendesk.com/hc/article\_attachments/360030187252/custom-fields-edit.png)

Custom fields make many of MWARE Portal’s entities extensible directly from the administrative user interface. Use them as is or combine them with some back-end code, and you have yet another powerful, flexible feature at your disposal. As they’re fond of saying at The Lunar Resort, “The sky is certainly not the limit.”
