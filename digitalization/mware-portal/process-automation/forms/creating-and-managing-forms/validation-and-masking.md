# Validation and Masking

Certain Forms fields can be validated (Text, Date, and Numeric fields) and masked (Numeric fields) to ensure User input is valid.

NOTE

Date Fields can be validated beginning in Liferay 7.4.

Enable input validation to ensure that only certain values are accepted in a Text or Numeric field.

### [Enabling Field Validation](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#enabling-field-validation) <a href="#enabling-field-validation" id="enabling-field-validation"></a>

To enable validation:

1. While viewing a form, click on a Text, Numeric, or Date field to open the field configuration menu.
2. Click the _Advanced_ tab.
3.  Switch the _Validation_ toggle to the right to enable validation.

    ![Validate data to ensure you're collecting only useful information.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/01.png)

Once validation has been enabled, configure the settings. For example, the Text field can specify the _Accept If Input_ condition and the _Error Message_ to indicate how validation failed.

The Field validation rule is triggered as soon as a User navigates away from a validated form field (by clicking into another field, for example).

![Validation is triggered when the User leaves a validated field.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/06.png)

### [Validating Text Fields](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#validating-text-fields) <a href="#validating-text-fields" id="validating-text-fields"></a>

See the [Validation Conditions Reference](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-conditions-reference) for more information about the other validation conditions.

#### [Validating Email Addresses](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#validating-email-addresses) <a href="#validating-email-addresses" id="validating-email-addresses"></a>

Forms can be configured to validate email addresses. To ensure that the user has entered a valid email address:

1. Select the _Is an email_ condition from the _Accept If Input_ dropdown.
2. Enter the error message in the _Error Message_ field: Please enter a valid email address.
3.  Click _Save_ when finished.

    ![Use text field validation to make sure users enter a valid email address or URL.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/04.png)

#### [Validating URLs](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#validating-urls) <a href="#validating-urls" id="validating-urls"></a>

Similar to emails, Forms can be configured to validate URLs.

To validate URLs:

1. Select the _Is a URL_ condition from the _Accept If Input_ dropdown.
2. Enter an error message.
3. Click _Save_ when finished.

#### [Using Regular Expressions](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#using-regular-expressions) <a href="#using-regular-expressions" id="using-regular-expressions"></a>

Forms can be configured to use [regular expressions](https://en.wikipedia.org/wiki/Regular\_expression) to create custom validation criteria. For example, use this regular expression to ensure that ten consecutive numeric digits are entered in a phone number field:

```log
^[0-9]{10}$
```

To validate a text field using a regular expression:

1. Select the _Matches_ condition from the _Accept If Input_ dropdown menu.
2. Enter the regular expression.
3.  Enter an error message in the _Error Message_ field.

    ![Regular expression text validation opens up countless possibilities.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/05.png)
4. Click _Save Form_ when finished.

### [Validating Numeric Fields](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#validating-numeric-fields) <a href="#validating-numeric-fields" id="validating-numeric-fields"></a>

Numeric field validation is similar to text field validation, but the conditions compare the value of the number entered to some other value.

To validate numeric fields:

1. Select the desired condition from the _Accept If Input_ dropdown menu (for example, _Is greater than_).
2. Enter numeric value (_10_) that serves as the limit.
3.  Enter an error message in the _Show Error Message_ field.

    ![Numeric conditions constrain user-entered numeric data.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/02.png)
4. Click _Save Form_ when finished.

#### [Requiring Only Integers](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#requiring-only-integers) <a href="#requiring-only-integers" id="requiring-only-integers"></a>

Forms can be configured to require numeric values to be integers. By default, the _My numeric type is_ value is set to Decimal.

To configure the Numeric field to accept only integers:

1. While viewing a form, click on a Numeric field to open the field configuration menu.
2.  On the _Basic_ tab, click on the _Integer_ radio button.

    ![Specify whether numbers can be decimals or integers only.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/03.png)
3. Click _Save Form_ to apply the changes.

### [Validating Date Fields](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#validating-date-fields) <a href="#validating-date-fields" id="validating-date-fields"></a>

As of Liferay 7.4, the Advanced configuration of the Date Field includes Validation.

![Enter validation rules for date fields.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/07.png)

Validation options include

**Accepted Date:** Require that the selected date be a future date, a past date, or a specified range.

**Starts From/Ends On:** In the case of future dates, define the date picker Start From date. For past dates, define the Ends On date. Configure whether the date picker should use the Response Date (the date that the user is filling out the form) or a custom date for this value. Both Starts From and Ends On must be configured for a date range validation.

**Error Message:** The error message can be customized to better suit the specific validation rule.

### [Masking Numeric Fields](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking#masking-numeric-fields) <a href="#masking-numeric-fields" id="masking-numeric-fields"></a>

The Input Mask configuration lets you you display the correct number format as the user enters numeric data. The configuration behavior differs between integer fields and decimal fields:

*   Integer field masks: configure the Format. A second entry, Character Options, is a read-only field setting that provides additional guidance on the character behavior used to define the format.

    ![Create an input mask for integer fields.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/08.png)
*   Decimal field masks: Configure the Thousands Separator, the Decimal Separator, the Decimal Places, and the Prefix or Suffix settings. The thousands separator style can be `None`, `1,000`, `1.000`, or `1 000`. The decimal separator style can be `0.00` or `0,00`. Prefix or Suffix. Use the Decimal Places setting to specify a limit to the number of decimal digits. The prefix or suffix is usually used to define the units of the decimal number. For example, use `$` as a prefix for a field that expects the user to enter a dollar amount.

    ![Create an input mask for decimal fields.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/validating-text-and-numeric-field-entries/images/09.png)

\
