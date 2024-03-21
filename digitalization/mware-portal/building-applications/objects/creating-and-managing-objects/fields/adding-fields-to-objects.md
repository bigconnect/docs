# Adding Fields to Objects

Fields are data definitions that represent database columns and store different [types of values](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-fields-to-objects#field-types-ui-reference). By default, all objects include these system fields: Author, Create Date, External Reference Code, ID, Modified Date, and Status. But you can add new fields at any time.

Follow these steps:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2.  Begin editing the desired object definition.

    NOTE

    For 7.4 +, you can add fields to system objects and access them via Headless APIs. See Extending System Objects for more information.
3.  Go to the _Fields_ tab and click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).

    ![Go to the Fields tab, click the Add button, and enter the required details.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/adding-fields-to-objects/images/01.png)
4.  Enter a _Label_ and _Field Name_.

    **Label**: This value identifies the field in the Objects UI and can be localized after the field is created.

    **Field Name**: This value determines the field’s name in the back-end and uses camel case. Once a field is published, this value cannot be changed.

    NOTE

    Some field names are reserved by the portal and cannot be used. See Reserved Field Names for more information.
5.  Select a field _Type_. Some field types include additional configuration options (e.g., picklist, attachment). See Field Type Reference below for an overview of each type.

    Some options appear during field creation, and others only appear after field creation.

    TIP

    You can require some types to accept only unique values. For example, you can prevent users from entering an existing email address. For text or integer field types, enable _Accept Unique Values Only_.
6. Determine whether the field is mandatory.
7. Click _Save_.

After saving fields to an object draft, you can access additional configuration options. For instance, you can determine whether a field is searchable, limit the number of allowed characters for text fields, and more. Available configuration options depend on the field’s type. If you publish the draft or add fields to an already published object, configuration options are restricted. You can remove custom fields from objects at any time.

IMPORTANT

For version 7.4 and earlier versions, users cannot remove published fields. After publishing, you can only remove fields added to the object’s side table (i.e., `[Initial_Table_Name]_x`). See Publishing Object Drafts for more information.

### Field Types UI Reference <a href="#field-types-ui-reference" id="field-types-ui-reference"></a>

| Field Types (UI)                            | Data Type (HEADLESS) | Description                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Aggregation                                 | String               | Stores read-only values calculated using values from a relationship table. Available functions include `count`, `sum`, `average`, `min`, and `max`.                                                                                                                                                                                                                           |
| Attachment                                  | Relationship > Long  | Stores files; by default, uploaded files can be no larger than 100 MB. This field supports all file types accepted by Documents and Media.                                                                                                                                                                                                                                    |
| Auto-Increment                              | String               | Stores read-only values calculated by incrementing the last assigned value by `1`. You can configure the starting value, prefix, and suffix (e.g., `foo200bar`, `foo201bar`, `foo202bar`, …).                                                                                                                                                                                 |
| Boolean                                     | Boolean              | Stores true or false values. Mandatory boolean fields require a true value from end users.                                                                                                                                                                                                                                                                                    |
| Date                                        | Date                 | Stores date values.                                                                                                                                                                                                                                                                                                                                                           |
| Date and Time                               | DateTime             | <p>Stores date and time values. You can store time values as entered or convert them to UTC.<br><br>Convert to UTC: Stores the time in UTC and displays it in the user’s timezone.<br><br>Use Input as Entered: Stores and displays the time as entered, regardless of timezone.</p>                                                                                          |
| Decimal (_previously Double_)               | Double               | <p>Stores a decimal number value with a limit of 16 digits.<br><br>Example: <code>999.9999999999999</code> (16 digits total)</p>                                                                                                                                                                                                                                              |
| Encrypted                                   | String               | Stores encrypted string values.                                                                                                                                                                                                                                                                                                                                               |
| Formula                                     | Integer or Double    | Stores read-only values calculated using numeric fields in the object definition. During field creation, determine whether the total is stored as an integer or decimal. After creation, edit the field to set its formula. Available functions include add (`+`), subtract (`-`), multiply (`*`), and divide (`/`).                                                          |
| Integer                                     | Integer              | <p>Stores an integer up to nine digits in length. You can choose to allow only unique values.<br><br>Maximum value: <code>2147483647</code><br><br>Minimum value: <code>-2147483648</code></p>                                                                                                                                                                                |
| Long Integer (_previously Long_)            | Long                 | <p>Stores a large integer up to 16 digits in length.<br><br>Maximum value: <code>9007199254740991</code><br><br>Minimum value: <code>-9007199254740991</code></p>                                                                                                                                                                                                             |
| Long Text (_previously Clob_)               | Clob                 | Stores a text box value that supports up to 65,000 characters. After creating a long text field, you can limit the number of characters allowed.                                                                                                                                                                                                                              |
| Multiselect Picklist                        | String               | Stores one or more string values from a [picklist](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists).                                                                                                                                                                                                                                        |
| Picklist                                    | String               | Stores one string value from a [picklist](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists). After adding the field, go to the Advanced tab to set a default value. You can set this value manually by selecting a specific picklist option or dynamically using the expression builder.                                                     |
| Precision Decimal (_previously BigDecimal_) | BigDecimal           | <p>Stores a high-precision decimal number without rounding, up to 16 decimal digits.<br><br>Maximum Value: <code>99999999999999.9999999999999999</code><br><br>Minimum value: <code>-99999999999999.9999999999999999</code></p>                                                                                                                                               |
| Relationship                                | Long                 | Stores the numeric ID for all related object entries. Objects automatically adds relationship fields to objects on the child side of a one-to-many relationship. See [Defining Object Relationships](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships) for more information. |
| Rich Text                                   | Clob                 | Stores text with advanced formatting tools and media elements (e.g., images, videos, audio).                                                                                                                                                                                                                                                                                  |
| Text (_previously String_)                  | String               | Stores simple text values up to 280 characters. You can choose to allow only unique values. After creating the field, you can limit the number of characters allowed.                                                                                                                                                                                                         |

### Reserved Field Names <a href="#reserved-field-names" id="reserved-field-names"></a>

The following field names are reserved by the portal and cannot be used. If users attempt to create a field using one of these field names, the portal displays an error message.

| Field                   | Description                                     |
| ----------------------- | ----------------------------------------------- |
| `companyId`             | Portal instance where the entry was created.    |
| `createDate`            | When the entry was created.                     |
| `externalReferenceCode` | External reference code for the entry.          |
| `groupId`               | Site ID where the entry was created.            |
| `lastPublishDate`       | Date when the entry was last published.         |
| `modifiedDate`          | Date when the entry was last modified.          |
| `mvccVersion`           | MVCC version of the entry.                      |
| `objectDefinitionId`    | ID of the entry’s object.                       |
| `objectEntryId`         | ID for the entry.                               |
| `status`                | Workflow status for the entry.                  |
| `statusByUserId`        | ID of the assigned user in Workflow.            |
| `statusByUserName`      | Name of the assigned user in Workflow.          |
| `statusDate`            | Date when the Workflow status was last updated. |
| `userEmailAddress`      | Entry author’s email address.                   |
| `userFirstName`         | Entry author’s first name.                      |
| `userId`                | Entry author’s user ID.                         |
| `userLastName`          | Entry author’s Last name.                       |
| `userName`              | Entry author’s user name.                       |
| `uuid`                  | Entry’s unique universal ID.                    |
