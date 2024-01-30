# Managing Data Definitions

There are several ways to manage your data definitions. Of course, you can edit a data definition, but you can also configure its permissions, manage its templates, copy it, or delete it.

Follow these steps to access your data definitions:

1. Open the Menu (![Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360030163731/icon-menu.png)) and expand your site’s menu (the Site Administration menu). Then select _Content & Data_ → _Dynamic Data Lists_. This opens the Dynamic Data Lists screen. A table lists any existing dynamic data lists.
2. Click the _Options_ button at the top-right (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030163711/icon-options.png)) and select _Manage Data Definitions_. The Data Definitions screen appears. A table lists the data definitions.

![Figure 1: You can copy an existing data definition, manage its templates, and more.](https://liferay-support.zendesk.com/hc/article\_attachments/360030163691/ddl-definitions-actions.png)

You can manage your data definitions via the _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030195352/icon-actions.png)) for each definition:

**Edit:** Edit the data definition. The edit screen uses the same form for creating data definitions. Note that if you edit a data definition referenced elsewhere (e.g., by a dynamic data list or display template), then you must update that reference.

**Manage Templates:** The _Manage Templates_ screen creates and manages templates for the data definition. For details, see [Using Templates to Display Forms and Lists](https://help.liferay.com/hc/zh-cn/articles/360028821792-Using-Templates-to-Display-Forms-and-Lists).

**Permissions:** Configure the data definition’s permissions. Note that these permissions are for an individual definition accessed through the Dynamic Data Lists application in _Site Administration_ → _Content_ → _Dynamic Data Lists_. For example, if Site members have View permission for a data definition, any Site member who also has a Role that can access the Dynamic Data Lists app and its data definitions can see this definition listed in the Manage Data Definitions screen. If you don’t want this, remove the View permission for Site Member, and Site members won’t see your data definition listed with the others.

**Copy:** The _Copy Data Definition_ form copies the definition and its templates. In the form, give the copied definition a new name and description and select whether to also copy the original definition’s templates. Click _Copy_ when you’re done. The copied definition then appears in the Data Definitions table with existing definitions. You can create new definitions based on existing ones, and then modify the copied one to suit your needs. You can, of course, edit any definition in the portal, but if you copy a definition instead, you can still access the original.

**Delete:** Delete the definition.
