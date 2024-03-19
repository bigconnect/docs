# Validation

### Introduction <a href="#introduction" id="introduction"></a>

An important part of designing a form is to prevent incorrect data from being captured. For example:

* an applicant's first and last names is required
* an applicant's age must be a positive number and has to be greater than a minimum
* an id number has to follow a specific syntax

If such conditions are not met, the user must see an _error_ and cannot submit the form until they are corrected.

In addition, some values might be correct, but the user should be encouraged to pay special attention to them. In such cases, the user should see a _warning_ or _informational message_ before submitting the form.

Form Builder supports this kind of validations via the "Validations and Alerts" tab of the "Control Settings" dialog. You open the dialog with the "Control Settings" icon to the right of each control.

_NOTE: Prior to Orbeon Forms 4.3, the dialog was called "Validation Properties". Orbeon Forms 4.3 combines dialogs and adds functionality._

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LY8kXxiKo1JfSTk7new%252F-LEkC5V\_Hi-90aIve8oZ%252Fvalidation.png%3Fgeneration=1549573222079977%26alt=media\&width=768\&dpr=4\&quality=100\&sign=a0019555d6ce20e136757ae5fb278cc2dbed18e24d296afa370688134e5c6210)

Control validation settings

### Validation types <a href="#validation-types" id="validation-types"></a>

#### Introduction <a href="#introduction-1" id="introduction-1"></a>

The value associated with a control can be validated with 3 different validation types:

1. _Required_. This indicates whether the value can be empty or not.
2. _Data Type_. For example `string`, `decimal`, `date`, or `time`.
3. _Formula_. A custom formula, expressed in XPath, which determines whether the value is valid or not for a certain validation level.
4. _Common Constraints_. This includes constraints such as "Maximum Length", "Minimum Length", and more. \[SINCE Orbeon Forms 4.10]

#### Required validation <a href="#required-validation" id="required-validation"></a>

This simple validation has 3 possibilities:

* _Yes_: a value is required and cannot be empty. (_NOTE: Blank spaces count as "not empty"._)
* _No_: a value is not required and can be empty.
* _Formula_: an XPath expression to dynamically control whether the control is valid or not. \[SINCE Orbeon Forms 4.7]

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LY8kXxiKo1JfSTk7new%252F-LEkC5VwndfWtxEALqSA%252Frequired-formula.png%3Fgeneration=1549573233672493%26alt=media\&width=768\&dpr=4\&quality=100\&sign=0a46124c5ca027f2191390f6ed70b0c7c9b0266d31e50464abfc3a82511c29d9)

Options for required validation

When the value is required, an asterisk appears next to the control to signify to the user that the value is required.

At runtime, if the value is required but not empty, the value is marked as invalid.

\[SINCE Orbeon Forms 4.9]

The Required validation can have a custom alert message.

#### Trimming leading and trailing spaces <a href="#trimming-leading-and-trailing-spaces" id="trimming-leading-and-trailing-spaces"></a>

\[SINCE Orbeon Forms 2016.1]

When the "Trim leading and trailing spaces" option is enabled, leading and trailing spaces are removed from the value of the control before validation. This option is enabled by default for text and email fields.

When used in conjunction with the Required validation, this allows making sure that a required field doesn't contain just spaces and in fact contains some value.

This option is also useful for non-required fields which must not contain leading or trailing spaces. For example if the user enters " Sam", the value is automatically converted into "Sam" as the user focuses out of the field.

See also [Required fields: more subtle than you might think](https://blog.orbeon.com/2016/02/required-fields-more-subtle-than-you.html).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LY8kXxiKo1JfSTk7new%252F-LEkC5W-Ff\_PhkPn1g2X%252Fwhitespace-trimming.png%3Fgeneration=1549573222005087%26alt=media\&width=768\&dpr=4\&quality=100\&sign=47791cf811fec593c3e6dbdfbc8a67f804034eecc291280331f45f5f4a8d3ae2)

Whitespace trimming

#### Data type validation <a href="#data-type-validation" id="data-type-validation"></a>

The list of data types includes:

* Built-in types
* XML Schema types (only if an XML Schema with simple types was attache to the form).

Built-in types include:

* Common Types
  * String: any string of characters
  * Boolean: `true` or `false`
  * Email address: an email address
* Number Types
  * Decimal: positive or negative decimal number, which can be fractional with an optional decimal point `.`
  * Integer: positive or negative integer decimal number
  * Double-precision floating-point
    * \[UNTIL Orbeon Forms 4.10]
    * \[SINCE Orbeon Forms 2021.1] (again, see below)
* Date and Time Types
  * Date: date stored in ISO format
  * Time: time stored in ISO format
  * Date and Time: date and time stored in ISO format

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-5c67bae7a63635a5469148c05d51db8ec73c8608%252Fbuilt-in-types.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=bb10f722071be905b688a8d019a0808fb906f44e65f4f128cff5d473c24e1aca)

Built-in Types

_NOTE: Prior to Orbeon Forms 4.3, the list of available types was longer, as explained in_ [_this blog post_](https://blog.orbeon.com/2013/07/simplifying-list-of-data-types-in-form.html)_._

_NOTE: Until Orbeon Forms 4.10, the "Double-precision floating point" data type was available. It was removed with Orbeon Forms 2016.1, as explained in this_ [_blog post_](https://blog.orbeon.com/2016/01/removing-double-datatype.html)_, and re-added with Orbeon Forms 2021.1._

_The reason for the removal was that in the vast majority of cases, this is not the appropriate type ("Integer" or "Decimal" are), and users would often select the incorrect type and then get rounding errors in fields such as the Currency field._

_The reason for re-adding the double-precisiong floating-point data type is that it is appropriate for certain scientific calculations. This said, we recommend its use only in very specific circumstances._

At runtime, if the value is required and does not match the specified datatype, the value is marked as invalid.

_NOTE: When selecting certain controls from the toolbox, such as "Email", "Date", "Time", and "Date and Time", the appropriate data type is already selected by Form Builder. Changing the type to a different type might change the appearance of the control to match the type selected._

If an XML Schema containing simple types has been attached to the form \[Orbeon Forms PE only], the simples types are listed in the Schema Type menu.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC5WECdrg8lFENX1x%252Fschema-types.png%3Fgeneration=1528743370885318%26alt=media\&width=768\&dpr=4\&quality=100\&sign=e00042d710193452c254b283edb9a3d2c8d6df529e9c71fec5fdc155276c42b6)

Built-in Types

Either a built-in data type or an XML Schema data type can be selected. If you select an XML Schema data type, the built-in data type is automatically reset. Similarly, if you select a built-in data type, the XML Schema data type is reset.

At runtime, if the value is required and does not match the specified datatype, the value is marked as invalid. For example, if the value must be an `integer`, the value "John" is invalid.

_NOTE: If the control is of type `string`, doesn't have a constraint and is not required, then any value entered is valid. This is the default for input fields and text areas._

When an XML Schema data type is selected:

* If _Required_ is set to _Yes_, the control is still made required, and an asterisk appears.
* If _Required_ is set to _No_, the value must still match the definition of the XML Schema type to be valid. If the XML Schema type requires a non-empty value, setting _Required_ to _No_ does not make the value optional.

\[SINCE Orbeon Forms 4.9]

The Required validation can have a custom alert message.

#### Formula validation <a href="#formula-validation" id="formula-validation"></a>

A formula validation is a boolean XPath expression running with the XML element containing the value as context item. The validation _fails_ if the expression doesn't return `true()`. This also means that it fails if there is an error while running the validation.

See also [Form Builder Formulas](https://doc.orbeon.com/form-builder/formulas).

For example the following expression, which would make sense for a birthday date field, checks that the user is 18 year old or older:

Copy

```
. <= (current-date() - xs:yearMonthDuration("P18Y"))
```

\[SINCE Orbeon Forms 4.3]

There can be more than one formulas applied to a given control. You add formulas with the `+` icon and remove them with the `-` icon.

\[SINCE Orbeon Forms 4.3]

Each formula can have a _level_ associated with it and a custom alert message.

#### Dates to Exclude constraint <a href="#dates-to-exclude-constraint" id="dates-to-exclude-constraint"></a>

\[SINCE Orbeon Forms 2018.2]

The Dates to Exclude constraint takes a list (sequence) of dates to exclude, provided by a formula. The constraint fails if users type a date that is in the supplied list of dates to exclude. Also, dates in this list are disabled in the non-native date picker, so users cannot select them, as in the case of the 18th and 20th in the screenshot below. When the date picker is native, the dates are not disabled because the native date picker doesn't support disabling specific dates, but the validation still applies.

![Dates disabled in date picker](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LU9GQQUFuTgG-po\_RuD%252F-LU9G\_eqXh56gujs50oC%252Fvalidation-excluded-dates.png%3Fgeneration=1545286870583215%26alt=media\&width=768\&dpr=4\&quality=100\&sign=91b96189a9c1812bcd9ba4f1a62f7051b64736b528430f37dc87222b118c955c)

You might have a service storing a the list of dates to exclude in a [dataset](https://doc.orbeon.com/form-runner/features/datasets), for example. From that dataset, you can extract and convert the dates to XPath dates.

For example, assuming the following `my-excluded-dates` dataset, with dates in the ISO format:

Copy

```
<result>
  <date>2018-12-31</date>
  <date>2019-01-15</date>
  <date>2019-01-31</date>
</result>
```

You can pass the following expression as parameter to the "Dates to Exclude" common constraint:

Copy

```
for $d in fr:dataset('my-excluded-dates')/date return xs:date($d)
```

#### Other common constraints <a href="#other-common-constraints" id="other-common-constraints"></a>

* "Maximum Length"
  * SINCE Orbeon Forms 4.10
  * applies to "String" and "Email" types only
  * the constraint fails if the length of the value converted to a string is larger than the specified integer value
* "Minimum Length"
  * SINCE Orbeon Forms 4.10
  * applies to "String" and "Email" types only
  * the constraint fails if the length of the value converted to a string is smaller than the specified integer value
* "Positive"
  * SINCE Orbeon Forms 2016.1
  * applies to "Decimal" and "Integer" types only
  * the constraint fails if the number is not positive
* "Positive or Zero"
  * SINCE Orbeon Forms 2016.1
  * applies to "Decimal" and "Integer" types only
  * the constraint fails if the number is not positive or zero
* "Negative or Zero"
  * SINCE Orbeon Forms 2016.1
  * applies to "Decimal" and "Integer" types only
  * the constraint fails if the number is not negative or zero
* "Negative"
  * SINCE Orbeon Forms 2016.1
  * applies to "Decimal" and "Integer" types only
  * the constraint fails if the number is not negative
* "Maximum Fractional Digits"
  * SINCE Orbeon Forms 2016.1
  * applies to "Decimal" type only
  * the constraint fails if the decimal number has more than the specified number of significant digits (trailing zeros are ignored)
* "Maximum Attachment Size" ("Maximum Attachment Size per File" in Multiple File Attachments controls)
  * SINCE Orbeon Forms 2017.1
  * applies to Single and Multiple File Attachments controls only
  * the constraint fails if the attachment size is larger than the specified size
* "Maximum Aggregate Attachment Size"
  * SINCE Orbeon Forms 2024.1
  * applies to Multiple File Attachments controls only
  * the constraint fails if the attachment size is larger than the remaining allowed size for the control
* "Supported File Types"
  * SINCE Orbeon Forms 2017.1
  * applies to Single and Multiple File Attachments controls only
  * the constraint fails if the attachment mediatype doest not match one of the included mediatypes or mediatype wildcards

In the future, it is expected that more common constraints will be added (see [#2281](https://github.com/orbeon/orbeon-forms/issues/2281)).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LY8kXxiKo1JfSTk7new%252F-LEkC5WNsb5f8E5orVr-%252Fmin-max-constraints.png%3Fgeneration=1549573225882970%26alt=media\&width=768\&dpr=4\&quality=100\&sign=b61c26a1afa158d0cbed9dc5ed9dbe6fac1f7eac59506853e4a752d0928c9b66)

Min and max length constraints

### Control validity <a href="#control-validity" id="control-validity"></a>

A control value (entered by the user, constant, or calculated) is either _valid_ or _invalid_. It is invalid if any of the following conditions is met:

* It is required but remains empty.
* It does not match the selected data type.
* There is at least one failed error formula or common constraint validation.

### Validation levels <a href="#validation-levels" id="validation-levels"></a>

\[SINCE Orbeon Forms 4.3]

If a control is valid, it can have a _warning_ level. This is the case if there is at least one failed warning validation.

If a control doesn't have a warning level, it can have an _info_ level. This is the case if there is at least one failed info validation.

A warning or info level does not make the control value invalid and it is still possible to submit form data.

_NOTE: It is not possible to associate a validation level to the required or data type validations: they always use the error level._

### Localization of messages <a href="#localization-of-messages" id="localization-of-messages"></a>

\[Orbeon Forms PE only]

All alert messages can be localized.

When opening the dialog, the current language of the form determines the language used. To switch languages:

* close the dialog
* select another form language
* reopen the dialog

### Alert messages <a href="#alert-messages" id="alert-messages"></a>

When the user enters data, if the value is invalid or if the control has a warning or info level, the control is highlighted and one or more alert messages are shown. The message is selected as follows, generally following the philosophy of "more specific messages win over less specific messages":

_NOTE: Since 4.6.2, required validations take precedence over other validations, see_ [_#1830_](https://github.com/orbeon/orbeon-forms/issues/1830)_._

* If a required validation has failed:
  * The default alert message for the control is used if available, or a global default Form Runner message is used otherwise.
  * Other messages are not used, even if there are data type or error formula or common constraint validations.
* If data type validation has failed:
  * The default alert message for the control is used if available, or a global default Form Runner message is used otherwise.
  * Other messages are not used, even if there are error formula or common constraint validations.
* If at least one error validation has failed:
  * If no specific alert message is specified for the validation, the default alert message for the control is used if available, or a global default Form Runner message is used otherwise.
  * If a specific alert message is specified, then it is used.
  * More than one message can show is several error validations have failed.
* Only if the control is valid, if at least one warning formula or common constraint has failed:
  * The specific alert message is used.
  * More than one message can show is several warning validations have failed.
* Only if the control is valid and doesn't have any failed warning s, if at least one info formula or common constraint has failed:
  * The specific alert message is used.
  * More than one message can show is several info validations have failed.

_NOTE: It is not possible to associate specific alert message to the required or data type validations: they always use the default or global alert message._

Alert messages appear:

* under the control value
* in the Error Summary section of the form
* as badge counts in the navigation bar
* as general count in the browser's title bar

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LYDetF2V5j1m6fTZmmq%252F-LEkC5Wegthix-1JvV-f%252Fvalidation.png%3Fgeneration=1549655633402419%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1012da898f160b1eabf6b4be9992618a35fac50580e0ee74770aff261c60e2b1)

Alert Messages

### Validation errors and review messages dialog <a href="#validation-errors-and-review-messages-dialog" id="validation-errors-and-review-messages-dialog"></a>

By default, when saving or sending form data, the following happens:

* If any control value is invalid, a dialog shows and the operation is stopped.
* If all controls are valid and there are no warning or info messages, the operation continues.
* If all controls are valid and there is at least one warning or info message, the "Review Messages" dialog shows. \[SINCE Orbeon Forms 4.3]

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC5KBEjDkuvu6WRcF%252Freview-messages.png%3Fgeneration=1528743371046481%26alt=media\&width=768\&dpr=4\&quality=100\&sign=70f86f77fa14aa06c7bd0a9a2c2fa969fab1a219acf018fa0d2bb3cc5ca36b0b)

Review Messages

The user has the following choices:

* Stop the operation, close the dialog and review the warnings and/or informational messages.
* Continue the operation, ignoring the warnings and/or informational messages.

These processes are entirely configurable. See [Buttons and Processes](https://doc.orbeon.com/form-runner/advanced/buttons-and-processes) for more information.

Optionally, it is possible to annotate the XML data submitted with error, warning or informational messages. See [Buttons and Processes](https://doc.orbeon.com/form-runner/advanced/buttons-and-processes) for more information.

### Using an external validation service <a href="#using-an-external-validation-service" id="using-an-external-validation-service"></a>

You can validate a field using an external validation service as follows:

1. In your form, you'll want to have:
   * (a) the field you want to validate,
   * (b) a [hidden field](https://doc.orbeon.com/form-runner/component/hidden) used to store the result from the validation.
2. You create (c) an HTTP Service for your validation service
3. You create an action, which, when the value of the field you want to validate (a) changes, calls the service (c), passing the value of the field (a), and stores the result from the validation in the hidden field (b). That result is typically be a boolean, `true` if valid, and `false` if invalid.
4. In the Control Settings for the field you want to validate (a), in the Validations and Alerts tab, you use a formula to declare that the field is valid only of the value of the hidden field (b) is `true`.
