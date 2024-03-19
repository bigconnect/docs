# Adding Field Validations

Validations set rules for determining valid field entries. Each validation has its own trigger, conditions, and error message, which you can set via the Objects UI. You can define validations using [Groovy scripts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-groovy-validations) or [Liferay expressions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-expression-builder-validations).

A third type of validation is available as of Liferay DXP 2024.Q1+/GA112+: [Composite Key validation](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-composite-key-validations). Use composite key validations to require that a combination of two or more fields be unique.

![Create validations for custom and system object fields.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/01.png)

NOTE

If out-of-the-box validations don’t meet your needs, you can create custom validations using the `objectValidationRule` client extension. See [Microservice Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/microservice-client-extensions) for more information.

To add a validation,

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _Objects_.
2. Begin editing an object definition.
3. Go to the _Validations_ tab and click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ).
4.  Enter a _label_ and select a validation type: _Groovy_, _Expression Builder_, or _Composite Key_.

    If you’ve deployed object validation rule client extensions to your instance, they also appear as options.

    ![Enter a label and select a validation type.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/02.png)
5. Click _Save_.
6. Begin editing the validation.
7.  In the Basic Info tab, activate the validation.

    ![Activate the validation.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/03.png)
8.  Select the _trigger event_ to determine when the validation runs.

    Each validation can only have one trigger event.
9.  Add validation rules.

    For Groovy and Expression Builder validations, go to the _Conditions_ tab and add conditions to the validation.

    Conditions can include multiple fields and functions for performing complex logic.

    ![Add conditions to the validation.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/04.png)

    When using Groovy, you can browse and add available data fields to your conditions via the side panel. See [Using Groovy Validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-groovy-validations) for more information.

    When using Expression Builder, you can browse and add fields, operators, and functions to your conditions via the side panel. See [Using Expression Builder Validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-expression-builder-validations) for more information.

    IMPORTANT

    Groovy script validations are only available for Liferay PaaS and Liferay DXP Self-Hosted.

    To set up composite key validations, go to the _Unique Composite Key_ tab and select the fields to use as a composite key. See [Using Composite Key Validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-composite-key-validations) for more information.

    ![Choose the fields to use as a composite key.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/09.png)
10. Enter a localizable _error message_. This message appears whenever the validation is triggered, and field entries do not meet one or more of the defined conditions.
11. Select an output validation type to determine where the error message appears.

    **Full Validation (Form Summary)**: Display the error message at the top of the form.

    **Partial Validation (Inline Field)**: Display the error message next to the specified field.

    ![Enter an error message and select an output validation type.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/05.png)
12. Click _Save_.

While activated, the validation runs for all new object entries and is displayed in [layouts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts) and [form containers](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-fragments-to-build-forms)

![Validations are displayed in form containers.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/06.png)

### [Using Groovy Validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-groovy-validations) <a href="#using-groovy-validations" id="using-groovy-validations"></a>

Liferay PaaS and Liferay DXP Self-Hosted

Groovy validations support all standard [Groovy Script](https://groovy-lang.org/) capabilities. When defining conditions, you must use the `invalidFields` variable. Liferay only displays the validation error message when `invalidFields` returns `true`.

![Use the side panel to add field elements to your Groovy validations.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/07.png)

Liferay uses the [GroovyShell class](https://docs.groovy-lang.org/latest/html/api/groovy/lang/GroovyShell.html) to check your Groovy scripts for valid syntax when you click _Save_. If the script is invalid, Liferay shows an error message.

### [Using Expression Builder Validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-expression-builder-validations) <a href="#using-expression-builder-validations" id="using-expression-builder-validations"></a>

Expression Builder provides predefined fields, operators, and functions that you can access in the Elements side panel. Clicking an element adds it to the conditions editor. These functions return a Boolean value. See [Expression Builder Validations Reference](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/expression-builder-validations-reference) for a complete list of provided operators and functions.

IMPORTANT

You can only use Expression Builder validations with text, numeric, date, and Boolean field types.

![Use the side panel to add field, operator, and function elements to your validation.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/08.png)

Liferay checks your expression for valid syntax when you click Save. If the expression is invalid, Liferay shows an error message.

#### [Expression Builder Operators](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#expression-builder-operators) <a href="#expression-builder-operators" id="expression-builder-operators"></a>

This table lists available operators for Expression Builder validations:

| Operator           | Description                                                           |
| ------------------ | --------------------------------------------------------------------- |
| And ( `AND` )      | Coordinating conjunction used to indicate a dependent relationship    |
| Divided By ( `/` ) | Mathematical operator for division                                    |
| Minus ( `-` )      | Mathematical operator for subtraction                                 |
| Or ( `OR` )        | Coordinating conjunction used to indicate an independent relationship |
| Plus ( `+` )       | Mathematical operator for addition                                    |
| Multiply ( `*` )   | Mathematical operator for multiplication                              |

#### [Expression Builder Functions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#expression-builder-functions) <a href="#expression-builder-functions" id="expression-builder-functions"></a>

This table lists available Expression Builder functions with their compatible field types:

| Operator                    | Text Fields | Numeric Fields | Date Fields | Description                                                                                                            |
| --------------------------- | ----------- | -------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------- |
| Compare Dates               |             |                | ✔           | Checks if a date field’s value is the same as a set value.                                                             |
| Concat                      | ✔           |                |             | Combines multiple strings or text fields and returns a single string that you can use with other validation functions. |
| Condition                   | ✔           | ✔              | ✔           | Checks if user input meets one or more conditions and returns a Boolean value.                                         |
| Contains                    | ✔           | ✔              |             | Checks if a field contains a specified value and returns a Boolean.                                                    |
| Does Not Contain            | ✔           | ✔              |             | Checks if a field does not contain a specified value and returns a Boolean.                                            |
| Future Dates                |             |                | ✔           | Checks if a date field’s value is in the future and returns a Boolean.                                                 |
| Is a URL                    | ✔           |                |             | Checks if a text field is a URL and returns a Boolean.                                                                 |
| Is an Email                 | ✔           |                |             | Checks if a text field is an email and returns a Boolean.                                                              |
| Is Decimal                  |             | ✔              |             | Checks if a number field is a decimal and returns a Boolean.                                                           |
| Is Empty                    | ✔           |                |             | Checks if a text field is empty and returns a Boolean.                                                                 |
| Is Equal To                 | ✔           | ✔              |             | Checks if a field value is equal to a specified value and returns a Boolean.                                           |
| Is Greater Than             |             | ✔              |             | Checks if a number field is greater than a specific numeric value and returns a Boolean.                               |
| Is Greater Than or Equal To |             | ✔              |             | Checks if a number field is greater than or equal to a specific numeric value and returns a Boolean.                   |
| Is Integer                  |             | ✔              |             | Checks if a number field is an integer and returns a Boolean.                                                          |
| Is Less Than                |             | ✔              |             | Checks if a number field is less than a specific numeric value and returns a Boolean.                                  |
| Is Less Than or Equal To    |             | ✔              |             | Checks if a number field is less than or equal to a specific numeric value and returns a Boolean.                      |
| Is Not Equal To             | ✔           | ✔              |             | Checks if a field value is different from a specified value and returns a Boolean.                                     |
| Match                       | ✔           |                |             | Checks if a text field matches a specific string value or RegEx expression and returns a Boolean.                      |
| Old Value                   | ✔           | ✔              | ✔           | Retrieves the prior value for the specified field.                                                                     |
| Past Dates                  |             |                | ✔           | Checks if a date field’s value is in the past and returns a Boolean.                                                   |
| Range                       |             |                | ✔           | Checks if a date range begins with a past date and ends with a future date and returns a Boolean.                      |
| Sum                         |             | ✔              |             | Adds multiple numeric fields together and returns a single number that you can use with other validation functions.    |

See [Expression Builder Validations Reference](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/expression-builder-validations-reference) for more information and examples.

### [Available Fields Reference](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#available-fields-reference) <a href="#available-fields-reference" id="available-fields-reference"></a>

When constructing conditions, you can use any of the object’s custom or system fields. You can also select from relationship fields on the child side of a one-to-many relationship.

Below are all default fields available for custom objects:

| Field                   | Description                                    |
| ----------------------- | ---------------------------------------------- |
| `companyId`             | Portal instance where the entry was created    |
| `createDate`            | When the entry was created                     |
| `currentDate`           | Date when the entry is submitted               |
| `currentUserId`         | ID of the user submitting the entry            |
| `externalReferenceCode` | External reference code for the entry          |
| `groupId`               | Site ID in where the entry was created         |
| `lastPublishDate`       | Date when the entry was last published         |
| `modifiedDate`          | Date when the entry was last modified          |
| `mvccVersion`           | MVCC version of the entry                      |
| `objectDefinitionId`    | ID of the entry’s object                       |
| `objectEntryId`         | ID for the entry                               |
| `status`                | Workflow status for the entry                  |
| `statusByUserId`        | ID of the assigned user in Workflow            |
| `statusByUserName`      | Name of the assigned user in Workflow          |
| `statusDate`            | Date when the Workflow status was last updated |
| `userId`                | ID of the entry’s author                       |
| `userName`              | User name of the entry’s author                |
| `uuid`                  | Unique universal ID for the entry              |

System objects have their own default fields, though there is some overlap with the above chart.

### [Using Composite Key Validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-composite-key-validations) <a href="#using-composite-key-validations" id="using-composite-key-validations"></a>

Liferay DXP 2024.Q1+/GA112+

To add composite key validation to your object, first enable its release feature flag. Go to _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → _Control Panel_ → _Instance Settings_ → _Feature Flags_. Open the _Release_ section and enable _Improve Field Validations (LPS-187854)_.

Some data models and applications require unique composite keys. For example, an orders object can require that each combination of customer ID and order date are unique in the system. Once enforced, this composite key can be used to look up and work with unique order entries. The Unique Composite Key validation ensures that a combination of fields is unique in the scope of the object. Site-scoped objects can have unique composite keys in the site, while instance-scoped objects can have unique composite keys throughout the instance.

You can use text, integer, and picklist fields in composite keys. You can also use relationship fields on the child side of a one-to-many relationship.

![You can use relationship fields in composite keys.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/10.png)

You cannot add a field to a composite key if it already has data.

![Fields with published data cannot be used in composite keys.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations/images/11.png)

Certain actions are only supported in draft objects:

| Action                                           | Object Status                 | Supported         |
| ------------------------------------------------ | ----------------------------- | ----------------- |
| Delete the validation                            | <p>Draft<br><br>Published</p> | <p>✔<br><br>✔</p> |
| Remove a field from the validation               | <p>Draft<br><br>Published</p> | <p>✔<br><br>✘</p> |
| Delete a field that’s being used in a validation | <p>Draft<br><br>Published</p> | <p>✘<br><br>✘</p> |
