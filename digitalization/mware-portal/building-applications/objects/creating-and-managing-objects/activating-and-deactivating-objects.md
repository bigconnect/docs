# Activating and Deactivating Objects

Custom objects are _activated_ when [published](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#publishing-object-drafts). While active, you can use the object to create entries via the Liferay UI or dedicated REST APIs. If needed, you can deactivate custom objects at any time.

This prevents users from creating or accessing its entries. While inactive, its data is unavailable, and you can only edit the object’s definition. If the object has relationships, it no longer appears in its related objects’ layouts. Additionally, the object does not appear in any [integrated frameworks](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/understanding-object-integrations) (e.g., permissions, workflow). This means that site elements referencing the object’s entries become blank.

Deactivating an object does not affect its data; it only prevents users from accessing it until the object is reactivated.

To deactivate or reactivate a custom object,

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2. Click the desired object. You cannot deactivate system objects.
3.  Under Object Data in the Details tab, toggle the switch to _Active_ or _Inactive_.

    ![Switch the object to Active or Inactive.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/activating-and-deactivating-objects/images/01.png)
4. Click _Save_.

\
