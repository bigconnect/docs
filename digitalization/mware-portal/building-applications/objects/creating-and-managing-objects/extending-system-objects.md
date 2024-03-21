# Extending System Objects

With Objects, you can extend supported system services. Currently, only these services are integrated with the Objects framework:

* Account
* Commerce Order
* Commerce Product
* Commerce Product Group
* Organization
* Postal Address
* User

NOTE

The Postal Address object stores account addresses. For this reason, you can only relate it to custom objects that are related to the Accounts object. See Using System Accounts with Custom Objects for more information.

![Extend system services integrated with the Objects Framework.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/extending-system-objects/images/01.png)

When extending system objects, you can add fields, relationships, actions, and validations to them.

| Feature       | System Objects | Custom Objects |
| ------------- | -------------- | -------------- |
| Fields        | ✔              | ✔              |
| Relationships | ✔              | ✔              |
| Layouts       |                | ✔              |
| Actions       | ✔              | ✔              |
| Views         |                | ✔              |
| Validations   | ✔              | ✔              |

### Adding Fields <a href="#adding-fields" id="adding-fields"></a>

You can add fields to system objects. The additional database columns are added to a separate database table that uses the service’s original table name with `_x` appended to it (e.g., `AccountEntry_x`). System objects support all available field types. Since system objects do not support custom layouts or views, you can only access these custom fields via Headless APIs. See Adding Fields to Objects for more information.

NOTE

Custom Fields application is for adding fields to most portal system applications using the Expando framework. Objects does not support the Expando-based custom fields, so you cannot use them with Objects features (e.g., relationships, actions, and validations).

### Adding Relationships <a href="#adding-relationships" id="adding-relationships"></a>

You can add relationships between system and custom objects to link their entities. System objects support both one-to-many and many-to-many relationships. See Defining Object Relationships for more information.

IMPORTANT

Since system objects don’t support custom layouts and views, you cannot add relationships to the UI for system objects. You can only interact with them via headless APIs.

#### Relationship APIs <a href="#relationship-apis" id="relationship-apis"></a>

If the system object is in a many-to-many relationship or on the parent side of a one-to-many relationship, the portal generates REST endpoints for querying and managing the relationship. This includes a GET endpoint for returning related object entries, PUT endpoint for relating entries, and DELETE endpoint for disassociating related entries.

In this example, the User system object is in a one-to-many relationship (`userRequests`) with a custom object (`timeOffRequest`). These endpoints appear in the [`headless-admin-user`](http://localhost:8080/o/api?endpoint=http://localhost:8080/o/headless-admin-user/v1.0/openapi.json) service.

![The portal generates REST endpoints for querying and managing the relationship.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/extending-system-objects/images/02.png)

See Using Relationships REST APIs for an introductory tutorial.

### Adding Actions <a href="#adding-actions" id="adding-actions"></a>

You can add actions to system objects, triggered on events like adding, updating, or removing entries. The table below shows actions supported by system objects:

| Action Type         | Supported in System Objects? |
| ------------------- | ---------------------------- |
| Webhook             | ✔                            |
| Groovy Script       | ✔                            |
| Notification        | ✔                            |
| Add Object Entry    | ✔                            |
| Update Object Entry | ✔                            |

See Defining Object Actions for more information.

### Adding Validations <a href="#adding-validations" id="adding-validations"></a>

You can add Groovy and Expression Builder validations to system objects. When triggered, these validations check for valid field entries and show custom error messages for invalid entries. Currently, Objects only supports validations for a system object’s default fields and does not support validations for its custom fields. See Adding Field Validations for more information.

### Additional Configuration Options <a href="#additional-configuration-options" id="additional-configuration-options"></a>

When editing a system object, you can also configure the following settings in the _Details_ tab.

| Field                            | Description                                                              | Supported in System Objects? |
| -------------------------------- | ------------------------------------------------------------------------ | ---------------------------- |
| Title Field                      | Determine the field used as the title for object entries.                | ✔                            |
| Panel Link                       | Determine where the application appears in the UI.                       | ✖                            |
| Widget in Page Builder           | Determine whether the object’s Page widget is available in sites.        | ✖                            |
| Categorization of Object Entries | Determine whether categorization tools are available for object entries. | ✖                            |
| Comments in Page Builder         | Determine whether users can add comments on object entries.              | ✖                            |
| Entry History in Audit Framework | Determine whether object events appear in the audit history.             | ✖                            |
