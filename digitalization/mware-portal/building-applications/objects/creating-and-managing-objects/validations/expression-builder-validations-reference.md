# Expression Builder Validations Reference

With custom Objects, you can create field validations using Expression Builder. This tool provides an integrated editor for quickly defining complex validation rules using predefined fields, operators, and functions. You can browse available elements in the conditions side panel.

IMPORTANT

Expression Builder validations can only be used with text, numeric, date, and Boolean field types.

### Text Fields <a href="#text-fields" id="text-fields"></a>

The portal provides the following operators and functions for Text and Long Text fields. You can use any of these fields as arguments. With some elements, you can also add string values to compare with the field’s value (e.g., `"foo"`, `"[A-Za-z]{1,50}"`).

#### Concat <a href="#concat" id="concat"></a>

Combines multiple strings or text fields and return a single string that can be used with other validation functions.

```log
concat([textField], "[string]")
```

#### Condition (with Text) <a href="#condition-with-text" id="condition-with-text"></a>

Checks if user input meets one or more conditions and returns a Boolean value. This function works like `if` statements. Each expression includes at least one `condition` (e.g., `textFieldName == "foo"`) and returns `true` or `false`. You can add multiple conditions to the same expression and determine a return value if none of the conditions are met (e.g., `textFieldName == "foo", textFieldName == "bar", true, false`).

```log
condition([condition], [return-value])
```

#### Contains (with Text) <a href="#contains-with-text" id="contains-with-text"></a>

Checks if a text field contains a specific string value and returns a Boolean. If the field _does not_ contain the value, it returns `false`.

```log
contains([textField], "[string]")
```

#### Does Not Contain (with Text) <a href="#does-not-contain-with-text" id="does-not-contain-with-text"></a>

Checks if a text field does not contain a specific string value and returns a Boolean. If the field _does_ contain the value, it returns `false`.

```log
NOT(contains([textField], "[string]"))
```

#### Is a URL <a href="#is-a-url" id="is-a-url"></a>

Checks if a text field is a URL and returns a Boolean. If the field does not match a URL pattern, it returns `false`. To return `true`, entries must use standard URL elements (e.g., `http`, `://`, `.com`)

```log
isURL([textField])
```

#### Is an Email <a href="#is-an-email" id="is-an-email"></a>

Checks if a text field is an email and returns a Boolean. If the field does not match a specific email or email pattern, it returns `false`. To return `true`, entries must use standard email elements (e.g., `@gmail`, `.com`)

```log
isEmailAddress([textField])
```

#### Is Empty <a href="#is-empty" id="is-empty"></a>

Checks if a text field is empty and returns a Boolean. If the field is not empty, it returns `false`.

```log
isEmpty([textField])
```

#### Is Equal To (with Text) <a href="#is-equal-to-with-text" id="is-equal-to-with-text"></a>

Checks if a text field is equal to a specific string value and returns a Boolean. If they _are not_ equal, it returns `false`.

```log
[textField] == "[string]"
```

#### Is Not Equal To (with Text) <a href="#is-not-equal-to-with-text" id="is-not-equal-to-with-text"></a>

Checks if a text field is different from a specific string value and returns a Boolean. If they _are_ equal, it returns `false`.

```log
[textField] != "[string]"
```

#### Matches <a href="#matches" id="matches"></a>

Checks if a text field matches a specific string value or RegEx expression and returns a Boolean. If the field does not match the value, it returns `false`.

```log
match([textField], "[string]")

match([textField], "[regex]")
```

#### Old Value (with Text) <a href="#old-value-with-text" id="old-value-with-text"></a>

Retrieves the prior value for the specified text field. You can use the value with other functions and operators.

```log
oldValue([textFieldName])
```

### Numeric Fields <a href="#numeric-fields" id="numeric-fields"></a>

The portal provides the following operators and functions for Integer, Long Integer, Decimal, and Precision Decimal fields. You can use any of these fields as arguments. With some elements, you can also add numeric values to compare with the field’s value (e.g., `123`, `3.1415`).

#### Condition (with Numeric) <a href="#condition-with-numeric" id="condition-with-numeric"></a>

Checks if user input meets one or more conditions and returns a Boolean value. This function works like `if` statements. Each expression includes at least one `condition` (e.g., `numericFieldName == 10`) and returns `true` or `false`. You can add multiple conditions to the same expression and determine a return value if none of the conditions are met (e.g., `numericFieldName == 10, numericFieldName != 100, true, false`).

```log
condition([condition], [return-value])
```

#### Contains (with Numeric) <a href="#contains-with-numeric" id="contains-with-numeric"></a>

Checks if a number field contains a specific numeric value and returns a Boolean. If the field _does not_ contain the value, it returns `false`.

```log
contains([numericField], [number])
```

#### Does not Contain (with Numeric) <a href="#does-not-contain-with-numeric" id="does-not-contain-with-numeric"></a>

Checks if a number field contains a specific numeric value and returns a Boolean. If the field _does_ contain the value, it returns `false`.

```log
NOT(contains([numericField], [number]))
```

#### Is Decimal <a href="#is-decimal" id="is-decimal"></a>

Checks if a number field is a decimal and returns a Boolean. If the field _is not_ a decimal, it returns `false`.

```log
isDecimal([numericField])
```

#### Is Integer <a href="#is-integer" id="is-integer"></a>

Checks if a number field is an integer and returns a Boolean. If the field _is not_ an integer, it returns `false`.

```log
isInteger([numericField])
```

#### Is Equal To (with Numeric) <a href="#is-equal-to-with-numeric" id="is-equal-to-with-numeric"></a>

Checks if a number field is equal to a specific numeric value and returns a Boolean. If they _are not_ equal, it returns `false`.

```log
[numericField] == [number]
```

#### Is Not Equal To (with Numeric) <a href="#is-not-equal-to-with-numeric" id="is-not-equal-to-with-numeric"></a>

Checks if a number field is different from a specific numeric value and returns a Boolean. If they _are_ equal, it returns `false`.

```log
[numericField] != [number]
```

#### Is Greater Than <a href="#is-greater-than" id="is-greater-than"></a>

Checks if a number field is greater than a specific numeric value and returns a Boolean. If the field _is not_ greater, it returns `false`.

```log
[numericField] > [number]
```

#### Is Greater Than or Equal To <a href="#is-greater-than-or-equal-to" id="is-greater-than-or-equal-to"></a>

Checks if a number field is greater than or equal to a specific numeric value and returns a Boolean. If the field _is not_ equal or greater, it returns `false`.

```log
[numericField] >= [number]
```

#### Is Less Than <a href="#is-less-than" id="is-less-than"></a>

Checks if a number field is less than a specific numeric value and returns a Boolean. If the field _is not_ less, it returns `false`.

```log
[numericField] < [number]
```

#### Is Less Than Or Equal To <a href="#is-less-than-or-equal-to" id="is-less-than-or-equal-to"></a>

Checks if a number field is less than or equal to a specific numeric value and returns a Boolean. If the field _is not_ equal or less, it returns `false`.

```log
[numericField] <= [number]
```

#### Old Value (with Numeric) <a href="#old-value-with-numeric" id="old-value-with-numeric"></a>

Retrieves the prior value for the specified numeric field. You can use the value with other functions and operators.

```log
oldValue([numericField])
```

#### Sum <a href="#sum" id="sum"></a>

Adds multiple numeric fields together and returns a single number that can be used with other validation functions.

```log
sum([numericField], [numericField])
```

### Date Fields <a href="#date-fields" id="date-fields"></a>

The portal provides the following operators and functions for Date fields. You can use any Date field as an argument or parameter. Additionally, you can enter fixed date values (`"yyyy-MM-dd"`) for comparing with your date field (e.g., `2020-03-19`).

#### Compare Dates <a href="#compare-dates" id="compare-dates"></a>

Checks if a date field’s value is the same as the parameter. If the field does not match the parameter, it returns `false`.

```log
compareDates([dateField], [parameter])
```

#### Condition (with Date) <a href="#condition-with-date" id="condition-with-date"></a>

Checks if user input meets one or more conditions and returns a Boolean value. This function works like `if` statements. Each expression includes at least one `condition` (e.g., `dateFieldName == 2020-01-01`) and returns `true` or `false`. You can add multiple conditions to the same expression and determine a return value if none of the conditions are met (e.g., `dateFieldName == 2020-01-01, dateFieldName != 2022-01-01, true, false`).

```log
condition([condition], [return-value])
```

#### Future Dates <a href="#future-dates" id="future-dates"></a>

Checks if a date field’s value is equal to or after a set parameter. If it _is not_, the function returns `false`.

```log
futureDates([dateField], [parameter])
```

For example,

```log
futureDates(endDate, startDate)
```

```log
futureDates(endDate, "2024-01-01")
```

The first example checks whether the `endDate` is equal to or after the `startDate` value. The second example checks whether the `endDate` value is equal to or after `2024-01-01`.

#### Old Value (with Date) <a href="#old-value-with-date" id="old-value-with-date"></a>

Retrieves the prior value for the specified date field. You can use the value with other functions and operators.

```log
oldValue([dateField])
```

#### Past Dates <a href="#past-dates" id="past-dates"></a>

Checks if a date field’s value is equal to or before a set parameter. If it _is not_, the function returns `false`.

```log
pastDates([dateField], [parameter])
```

For example,

```log
pastDates(startDate, endDate)
```

```log
pastDates(startDate, "2024-01-01")
```

The first example checks whether the `startDate` is equal to or before the `endDate` value. The second checks whether the `startDate` value is equal to or before `2024-01-01`.

#### Range <a href="#range" id="range"></a>

Checks whether date field values begin with a past date and end with a future date. If the dates _are not_ within the range, it returns `false`.

```log
futureDates([dateField], [parameter])
AND
pastDates([dateField], [parameter])
```

For example,

```log
futureDates(endDate, startDate)
AND
pastDates(startDate, endDate)
```

This checks whether the `endDate` value is equal to/after the `startDate` value and whether the `startDate` value is equal to/before the `endDate` value.

### Math Operators <a href="#math-operators" id="math-operators"></a>

You can use any of the following math operators with numeric fields.

#### Plus ( `+` ) <a href="#plus" id="plus"></a>

Adds numeric fields to create an expression.

```log
[numericField] + [numericField]
```

#### Minus ( `-` ) <a href="#minus" id="minus"></a>

Subtracts numeric fields from one another to create an expression.

```log
[numericField] - [numericField]
```

#### Divided By ( `/` ) <a href="#divided-by" id="divided-by"></a>

Divides one numeric field by another to create an expression.

```log
[numericField] / [numericField]
```

#### Multiply ( `*` ) <a href="#multiply" id="multiply"></a>

Multiplies numeric fields to create an expression.

```log
[numericField] * [numericField]
```

### Logical Operators <a href="#logical-operators" id="logical-operators"></a>

Logical operators are used with multiple elements to aggregate complex conditions from multiple elements. Currently, Expression Builder validations provide the `AND` and `OR` operators.

* `AND`: Indicates a dependent relationship.
* `OR`: Indicates an independent relationship.

In the following example, the `AND` clauses are dependent on each other and must be taken together. By contrast, the `OR` clauses are logically separate and not co-dependent on each other.

```log
match([phoneNumberField], "^([1-9]{2}) (?:[2-8]|9[1-9])[0-9]{3}-[0-9]{4}$")
AND match([nameField], "[A-Za-z]{1,50}")
AND match([addressField], "[A-Za-z]{1,50}")
OR [numericField] == 1
OR match([lastNameField], "[A-Za-z]{1,50}")
```

### Validation Examples <a href="#validation-examples" id="validation-examples"></a>

The following validations are common examples. To use them, replace `fieldName` with the name of the field to validate.

#### Name Validation (with Text) <a href="#name-validation-with-text" id="name-validation-with-text"></a>

Restricts values to alphabetic characters and limits the number of allowed characters.

```log
match(fieldName, "[A-Za-z]{1,50}")
```

Allows for numbers in last names.

```log
match(fieldName, "[A-Za-z][0-9]{1,50}")
```

#### Password (with Text) <a href="#password-with-text" id="password-with-text"></a>

Checks if entries meet the following password criteria:

* At least 8 characters
* At least 5 unique characters
* At least 1 symbol
* At least 1 number
* No spaces allowed

```log
match(fieldName, "^(?=.*[A-Za-z])(?=.*d)(?=.*[@$!%*#?&])[A-Za-zd@$!%*#?&]{8,}$")
```

#### Cell Phone Number (with Numeric) <a href="#cell-phone-number-with-numeric" id="cell-phone-number-with-numeric"></a>

Checks if entries match a phone number pattern. It restricts entries to numeric characters, limits the number of characters, and sets a standard phone number pattern.

```log
match(fieldName, "^([1-9]{2}) (?:[2-8]|9[1-9])[0-9]{3}-[0-9]{4}$")
```

#### Postal Code (with Numeric) <a href="#postal-code-with-numeric" id="postal-code-with-numeric"></a>

Checks if entries match a postal code pattern. It restricts entries to numeric characters, limits the number of characters, and sets a standard postal code pattern.

```log
match(fieldName, "^([1-9]{2}) (?:[2-8]|9[1-9])[0-9]{3}-[0-9]{4}$")
```

#### Specify Age Range (Date) <a href="#specify-age-range-date" id="specify-age-range-date"></a>

Checks if entries are between 18-65.

```log
pastDates(fieldName, startsFrom, responseDate, years, -120, endsOn, responseDate, years, -18)
AND
futureDates(fieldName, startsFrom, responseDate, years, 0, endsOn, responseDate, years, 65)
```
