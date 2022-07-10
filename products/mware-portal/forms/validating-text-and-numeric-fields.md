# Validating Text and Numeric Fields

Validation ensures that only certain values are entered in a field. Validation functionality is available for text and numeric fields.

To enable validation,

1. Add a Text or Numeric field to a form in the Builder view.
2. Open the field’s _Properties_ tab.
3. Turn on the _Validation_ toggle to enable validation and open its configuration options.

![Figure 1: Validate data to ensure youre collecting only useful information.](https://liferay-support.zendesk.com/hc/article\_attachments/360030163271/forms-text-validation.png)

### Validating Text Fields <a href="#validating-text-fields" id="validating-text-fields"></a>

Validation for text fields contains several options. You must first choose a list of available conditions to check:

* If Input _Contains_
* If Input _Does Not Contain_
* If Input _Is not URL_
* If Input _Is not Email_
* If Input _Does not Match_

If the condition isn’t met, an error message is displayed to the user. Where you go from there depends on which condition you used.

#### IF INPUT CONTAINS/DOES NOT CONTAIN <a href="#if-input-containsdoes-not-contain" id="if-input-containsdoes-not-contain"></a>

When you validate text data to check if it contains a certain value, there are two additional steps to take after selecting the condition:

1. Enter the text to check for.
2. Enter an error message so users understand why their submission failed.

![Figure 2: If Liferay isnt part of the fields value, an error message is displayed.](https://liferay-support.zendesk.com/hc/article\_attachments/360030194752/forms-text-val-contains.png)

#### IF INPUT IS NOT URL/EMAIL <a href="#if-input-is-not-urlemail" id="if-input-is-not-urlemail"></a>

Checking for properly formatted URLs and emails is easy. Just choose the condition from the _If Input_ drop-down and enter the error message.

Valid URLs begin with `http://` or `https://`. Valid emails must contain `@`.

![Figure 3: Use text field validation to make sure users enter a valid email address or URL.](https://liferay-support.zendesk.com/hc/article\_attachments/360030163251/forms-text-val-email.png)

#### IF INPUT DOES NOT MATCH <a href="#if-input-does-not-match" id="if-input-does-not-match"></a>

The _Does Not Match_ condition is used for entering [regular expressions](https://en.wikipedia.org/wiki/Regular\_expression) to create custom validation criteria. For example, use this regular expression to ensure that ten consecutive numeric digits are entered in a phone number field:

```
^[0-9]{10}$
```

If you use regular expression validation, provide some explanatory text (e.g., help text, placeholder text, and a clear error message) to guide form users in entering the proper data.

![Figure 4: Regular expression text validation opens up countless possibilities.](https://liferay-support.zendesk.com/hc/article\_attachments/360030163291/forms-text-val-regex.png)

### Validating Numeric Fields <a href="#validating-numeric-fields" id="validating-numeric-fields"></a>

Numeric field validation is similar to text field validation, but the conditions compare the value of the number entered to some other value.

![Figure 5: Numeric conditions constrain user-entered numeric data.](https://liferay-support.zendesk.com/hc/article\_attachments/360030194772/forms-numeric-val2.png)

Available conditions to check include

* Is greater than or equal to
* Is greater than
* Is not equal to
* Is less than or equal to
* Is less than

For example, to make sure users don’t enter a number over 10, enable validation and use _Is greater than_ with a value of _10_. Use the message _Please enter 10 or less_.

![Figure 6: Make sure user-entered numeric data is within reasonable bounds. Nobody needs 11 sandwiches for lunch.](https://liferay-support.zendesk.com/hc/article\_attachments/360030194792/forms-numeric-val1.png)

Note that numeric fields are text fields validated to allow only numeric data entry. That’s why they’re in the Customized Elements section of the form fields list. In addition, the property _My numeric type is_ (can be Integer or Decimal) on the Basic tab of a numeric form is another form of validation.
