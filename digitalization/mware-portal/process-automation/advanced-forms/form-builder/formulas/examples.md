# Examples

### Testing on a Yes No Answer field <a href="#testing-on-a-yes-no-answer-field" id="testing-on-a-yes-no-answer-field"></a>

Say you have a _Yes/No Answer_ field (before 2016.1 called _Boolean Input_), named `yes-no`, and if the answer is _Yes_, you would like to show another field. In the _Visibility_ expression for this other field, you should write:

Copy

```
$yes-no/string() = 'true'
```

* Why not just write `$yes-no`? If you do, the expression will always evaluate to true, because Advanced Forms evaluates `boolean($yes-no)`: since `$yes-no` points to a node, the `boolean` function [always returns true](https://www.w3.org/TR/xpath-functions-3/#func-boolean).
* Why not write `$yes-no = true()`? In this case, since `$yes-no` compared to boolean value, it is [atomized](https://www.w3.org/TR/xpath-30/#id-atomization). The node pointed to by `$yes-no` is of type boolean, so the atomization process takes the node typed value. When the node contains the string `true` or `false`, this expression works as expected. However, if it contains an empty string, then the expression fails. For the purpose of determining whether a field is valid, read-only, or visible, when the expression fails the result will be considered to be `false`, so it will work if the expression is used on its own. However, if the expression is used in an `or` clause (`$yes-no = true() or something`), if the `$yes-no = true()` part is evaluated first, the expression as a whole will fail and return false, which is incorrect in case the other part of the `or` clause is true. Also, in the case of a calculated value, the result will be the empty string, instead of `false`.

### Sum of values in a repeat <a href="#sum-of-values-in-a-repeat" id="sum-of-values-in-a-repeat"></a>

Scenario: compute the sum of values in multiple repeat repetitions. Say you have:

* a repeated grid
* a decimal field called `price` on each row
* an integer field called `quantity` on each row
* a decimal text output field called `row-total` on each row
* a decimal text output field called `total` below the grid

You want to compute the row totals and athe general total.

Calculated value expression for `row-total`:

```
$price * $quantity
```

Calculated value expression for `total`:

```
sum($row-total[string() castable as xs:decimal], 0.0)
```

Explanation:

* when accessing control values with variables, the "closest" variables are found
* this means that the `row-total` control calculation applies to the closest `price` and `quantity` controls, that is, those on the same row, and each row gets its own total
* the `total` calculation is outside the repeat, and when it refers to `$row-total`, all `row-total` values are returned
* `sum()` is a standard XPath function to compute the sum of a sequence of items
* the predicate `[string() castable as xs:decimal]` excludes values that are blank or not a decimal number
* `sum()` supports a second argument which is the value to return in case no value satisfies the predicate (this makes sure that we return a decimal value, as we are using a literal decimal 0.0)

### Constrain a number between two values <a href="#constrain-a-number-between-two-values" id="constrain-a-number-between-two-values"></a>

Scenario: Make the current integer number field valid only if its value is between two values, say 12 and 17 included.

Expression:

```
. >= 12 and . <= 17
```

Explanation:

* `.` refers to the current value of the control
* `>=` or `ge` means "greater than or equals to"
* `<=` or `le` means "less than or equals to"
* `and` is the logical "and" operator

If you want to refer to a specific control by name, you can use:

```
$my-control >= 12 and $my-control <= 17
```

### Constrain the length of a string between two values <a href="#constrain-the-length-of-a-string-between-two-values" id="constrain-the-length-of-a-string-between-two-values"></a>

Scenario: Make the current field valid only if its length is between two values, say 2 and 140.

Expression:

```
string-length(.) >= 2 and string-length(.) <= 140
```

Explanation:

* `.` refers to the current value of the control
* The standard `string-length()` function returns the length of its argument
* `>=` or `ge` means "greater than or equals to"
* `<=` or `le` means "less than or equals to"
* `and` is the logical "and" operator

If you want to refer to a specific control by name, you can use:

```
string-length($message) >= 2 and string-length($message) <= 140
```

The same can be expressed, for the current control, as:

```
xxf:min-length(2) and xxf:max-length(140)
```

### Validating with a regular expression <a href="#validating-with-a-regular-expression" id="validating-with-a-regular-expression"></a>

Scenario: check that a given value matches a regular expression, for example "only ASCII letters and digits, the dash, and underscore character".

```
matches(., '^[A-Za-z0-9\-_]+$')
```

Explanation:

* the standard `matches()` function applies the regular expression passed as second argument to the first argument, and returns true if it does match

### Validating with a regular expression for an optional value <a href="#validating-with-a-regular-expression-for-an-optional-value" id="validating-with-a-regular-expression-for-an-optional-value"></a>

Scenario: check that a given number value is either blank or has exactly 5 digits.

```
matches(string(.), '\d{5}') or xxf:is-blank(string(.))
```

Explanation:

* the standard `matches()` function applies the regular expression passed as second argument to the first argument, and returns true if it does match
* the built-in `xxf:is-blank()` function returns `true()` if the value passed is blank
* since the data is a number, we use the `string(.)` function to convert the value to a string before passing it to functions

### Make a control read-only based on the value of another control <a href="#make-a-control-read-only-based-on-the-value-of-another-control" id="make-a-control-read-only-based-on-the-value-of-another-control"></a>

Scenario: Make a control read-only if the value of the `first-name` control is blank:

Expression:

```
xxf:is-blank($first-name)
```

Explanation:

* `$first-name` returns the value of the control with name "first-name"
* the built-in `xxf:is-blank()` function returns `true()` if the value passed is blank

### Setting a dynamic initial value <a href="#setting-a-dynamic-initial-value" id="setting-a-dynamic-initial-value"></a>

Scenario: As a form author, you can set a _static initial value_ for a control simply by setting that value at design time. For example:

* Enter a value in an input field
* Select an item in a dropdown list

But not all initial values can be static. For example, you might want a date selection control to contain the current date until the user changes it. In this case, you can use an "Initial Value" expression.

Initial Value expression:

```
current-date()
```

Explanation:

* `current-date()` is a standard XPath function returning the current date.

### Simple calculated values <a href="#simple-calculated-values" id="simple-calculated-values"></a>

Scenario: compute the sum of two numbers entered by the user in two fields, "quantity1" and "quantity2".

Calculated Value expression:

```
if ($quantity1 castable as xs:integer and $quantity2 castable as xs:integer)
then $quantity1 + $quantity2
else ''
```

Explanation:

* `if (...) then ... else ...` evaluates a condition and then returns one of two alternatives
* the condition `quantity1 castable as xs:integer` checks that the value from the field "quantity1" is an integer
* `quantity1 + $quantity2` simply adds the two values
* the value `''` represents an empty string This can be specified for example on a Text Output control.

_NOTE: If the value of a control is calculated, by default it is also marked as read-only. If you want a calculated control to be still editable by the user, set its Read-Only property explicitly to `false()`._

### Access a control in a particular repeat iteration <a href="#access-a-control-in-a-particular-repeat-iteration" id="access-a-control-in-a-particular-repeat-iteration"></a>

Given a form and a control called `name` within a repeat:

* `$name[2]`: return the value of the control in the second iteration
* `string-join($name, ', ')`: join all values with commas
* `count($name)`: return the number of values

_NOTE: This works when the expression is outside repeat repetitions. For expressions within the same repeat, `$name` returns the closest control._

### Check the Form Runner mode <a href="#check-the-form-runner-mode" id="check-the-form-runner-mode"></a>

#### Modes <a href="#modes" id="modes"></a>

The Form Runner detail page can have the following modes:

* `new`
* `edit`
* `view`
* `pdf`
* `email`
* `test` (when you are testing a form within Form Builder)



You can use the `fr:mode()` XPath function exposed by Form Runner to all XPath expressions.

```
fr:mode() = 'edit'
```

### Access HTTP request parameters and HTTP headers <a href="#access-http-request-parameters-and-http-headers" id="access-http-request-parameters-and-http-headers"></a>

It can be useful to access HTTP headers to set default values upon form initialization, for example when single sign-on systems use HTTP headers as a way of communicating information to an application.

XPath expressions have access to a special function, `xxf:request-header()`, which allows retrieving a header by name. Example of setting the default value of a field using an initial value:

```
xxf:get-request-header('full-name')
```

### Check the type of an attachment <a href="#check-the-type-of-an-attachment" id="check-the-type-of-an-attachment"></a>

Scenario: field `my-attachment` must be a PDF file.

Constraint expression:

```
ends-with(lower-case($my-attachment/@filename), '.pdf')
```

Explanation:

* Form Runner stores information about a file into XML attributes:
  * `@filename` accesses the file name as sent by the user's browser
  * `@mediatype` accesses the file type as sent by the user's browser
  * `@size` accesses the file size
* `$my-attachment/@filename` returns the file name associated with attachment "my-attachment"
* The `lower-case()` function converts that name to a lower case value
* The `ends-with()` function checks whether its first argument ends with the second argument

Similarly, you can test the file type:

```
$my-attachment/@mediatype = 'application/pdf'
```

### Set the value of a field from a URL parameter <a href="#set-the-value-of-a-field-from-a-url-parameter" id="set-the-value-of-a-field-from-a-url-parameter"></a>

Scenario: We want to set the value of a field from a URL parameter, but only if that parameter exists. If it doesn't, we want to leave the value of the field as it is.

```
(xxf:get-request-parameter('my-parameter'), .)[1]
```

Explanation:

* If the parameter exists
  * `xxf:get-request-parameter()` returns a single string
  * so you get a sequence containing that string following by the current value of the field
  * we take the first value of the sequence, so the value of the parameter is used
* if the parameter doesn't exist
  * `xxf:get-request-parameter()` returns an empty XPath sequence
  * so you get a sequence containing only the current value of the field
  * we take the first value of that sequence, so we get the current value of the control

### Testing the values of a checkbox <a href="#testing-the-values-of-a-checkbox" id="testing-the-values-of-a-checkbox"></a>

Say:

* a Checkboxes control is needed named `my-checkboxes`
* the Checkboxes control contains several choices, including two choices with values `foo` and `bar`
* you'd like to show another control if `my-checkboxes` has either `foo` or `bar` selected

```
xxf:split($my-checkboxes) = ('foo', 'bar')
```

Explanation:

* `$my-checkboxes` refers to the XML element containing the checkbox values as a space-separated list of tokens, such as:
  * ""
  * "foo"
  * "bar"
  * "foo bar"
  * "baz foo bar"
  * etc. if you have more values
* the `xxf:split()` function splits that value on spaces and returns an XPath sequence of string tokens
* the `=` comparison will return `true()` if at least one value matches another value on the other side

So in the end that formula returns `true()` if the field `my-checkboxes` has the `foo` or `bar` value selected.

### Constraint the number of selected checkboxes <a href="#constraint-the-number-of-selected-checkboxes" id="constraint-the-number-of-selected-checkboxes"></a>

Scenario: For a given set of checkboxes, make sure the number of selected checkboxes is at most 3.

```
count(xxf:split(.)) le 3
```

Explanation:

* `xxf:split(.)` tokenizes the space-separated values selected by the checkbox
* the number of tokens obtained with `count()` corresponds to the number of selected checkboxes
* then this makes sure the number of tokens is lower than or equal to 3

### Validating a Legal Entity Identifier (LEI) <a href="#validating-a-legal-entity-identifier-lei" id="validating-a-legal-entity-identifier-lei"></a>

You can use the following formula, in Control Settings, Validation and Alerts, Formula, to validate the relevant [LEI](https://en.wikipedia.org/wiki/Legal\_Entity\_Identifier) field. Should you need to assess the validity of a LEI field that is somewhere else in the form, just replace `.` on the first line with the appropriate expression. An examples of valid LEI are:

* `F50EOCWSQFAUVO9Q8Z97` (examples in ISO/DIS 17442)
* `549300Y7W34DK0WBPY51` (Google)
* `98450054NFCE7A67C172` (ManagedLEI)

Copy

```
let $lei := .
return
  string-length($lei) = 20 and
  xs:decimal(
    string-join(
      for $code in string-to-codepoints($lei)
      return
        if ($code >= string-to-codepoints('A') and string-to-codepoints('Z') >= $code) then
          (: Letters A-Z :)
          xs:string($code - string-to-codepoints('A') + 10)
        else if ($code >= string-to-codepoints('0') and string-to-codepoints('9') >= $code) then
          (: Digits 0-9 :)
          xs:string($code - string-to-codepoints('0'))
          (: Unrecognized characters :)
        else
          '',
      ''
    )
  ) mod 97 = 1
```

### Validating an International Securities Identification Number (ISIN) <a href="#validating-an-international-securities-identification-number-isin" id="validating-an-international-securities-identification-number-isin"></a>

You can use the following formula, in Control Settings, Validation and Alerts, Formula, to validate the relevant [ISIN](https://en.wikipedia.org/wiki/International\_Securities\_Identification\_Number) field. Should you need to assess the validity of a ISIN field that is somewhere else in the form, just replace `.` on the first line with the appropriate expression. Examples of valid ISIN are:

* `US0378331005` (Apple, Inc.)
* `AU0000XVGZA3` (Treasury Corporation of Victoria)
* `GB0002634946` (BAE Systems)

Copy

```
let $isin := .
return
  (
    let
      $without-checksum := substring($isin, 1, 11),
      $letter-as-numbers-string :=
        string-join(
          for $code in string-to-codepoints($without-checksum)
          return
            if ($code >= string-to-codepoints('A') and string-to-codepoints('Z') >= $code) then
              (: Letters A-Z :)
              xs:string($code - string-to-codepoints('A') + 10)
            else if ($code >= string-to-codepoints('0') and string-to-codepoints('9') >= $code) then
              (: Digits 0-9 :)
              xs:string($code - string-to-codepoints('0'))
              (: Unrecognized characters :)
            else
              '',
          ''
        ),
      $letter-as-numbers-digits :=
        for $code in string-to-codepoints($letter-as-numbers-string)
        return xs:decimal(codepoints-to-string($code)),
      $reversed-digits     := reverse($letter-as-numbers-digits),
      $group-1-digits      := $reversed-digits[position() mod 2 = 1],
      $group-2-digits      := $reversed-digits[position() mod 2 = 0],
      $group-1-x2-decimals := for $d in $group-1-digits return $d * 2,
      $group-1-x2-string   := string-join(for $d in $group-1-x2-decimals return string($d), ''),
      $group-1-x2-digits   := for $code in string-to-codepoints($group-1-x2-string)
                              return xs:decimal(codepoints-to-string($code)),
      $sum-digits          := sum(($group-1-x2-digits, $group-2-digits)),
      $checksum-computed   := string((10 - ($sum-digits mod 10)) mod 10),
      $checksum-provided   := substring($isin, 12, 1),
      $proper-checksum     := $checksum-computed = $checksum-provided,
      $proper-length       := string-length($isin) = 12
    return
      $proper-checksum and $proper-length
  )
```

### Number of weekdays between 2 dates <a href="#number-of-weekdays-between-2-dates" id="number-of-weekdays-between-2-dates"></a>

* _Input_ – In the following XPath expression, the start and end dates are inline, so you'll most likely want to modify it to refer to, say, dates entered by users in a form fields.
* _Weekends_ – The expression takes Saturday and Sunday to be part of the weekend.
* _First day of the week_ – The expression assumes Monday is the first day of the week. For instance check that on your system `format-date(xs:date('2018-12-10'), '[F1]')` returns 1. If not, you'll need to change the expressions for `$startW` and `$endW`.

```
for
    $start          in xs:date('2018-12-15'),
    $end            in xs:date('2018-12-26'),
    $startW         in xs:integer(format-date($start, '[F1]')),
    $endW           in xs:integer(format-date($end, '[F1]')),
    $days           in days-from-duration($end - $start),
    $daysNoWeekends in $days - (floor($days div 7) * 2)
return
    if ($days mod 7 != 0 and ($startW = 7 or $endW = 7)) then $daysNoWeekends - 1
    else if ($endW < $startW) then $daysNoWeekends - 2
    else $daysNoWeekends
```

### Converting a date/time to a named timezone <a href="#converting-a-date-time-to-a-named-timezone" id="converting-a-date-time-to-a-named-timezone"></a>

The following formula formats the current date/time to an ISO date/time in a specific named timezone:

```
format-dateTime(
  current-dateTime(),
  '[Y0001]-[M01]-[D01]T[H01]:[m01]:[s01]',
  'en',
  (),
  'Europe/Paris'
) 
```
