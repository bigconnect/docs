# Number

### bytes

The "bytes" function is used to format a number or string representing a size in bytes into a human-readable format. It takes a single argument, which can be a number or a string, and returns the formatted size.

Example:

handleBars

```Text
{{ bytes 1386 }}
```

In the above example, the "bytes" function is called with the number `1386`. It formats the number into its equivalent in bytes and returns a human-readable format. The output would be `1.4 KB`.

The "bytes" function is useful when you need to display the size of a file, storage, or any other data in a user-friendly format. It converts large numbers representing bytes into a more readable format, such as kilobytes (KB), megabytes (MB), gigabytes (GB), etc.

Here are some examples of how the "bytes" function formats different values:

* 'foo' => 3 B
* 13661855 => 13.66 MB
* 825399 => 825.39 kB
* 1396 => 1.4 kB

### addCommas

The "addCommas" function is used to add commas to a given number. It takes a single argument, which should be a number, and returns the number formatted with commas.

Example:

handleBars

```Text
{{ addCommas 1000000 }}
```

In the above example, the "addCommas" function is called with the number `1000000`. It adds commas to the number and returns it as a formatted string. The output would be `1,000,000`.

The "addCommas" function is useful when you want to improve the readability of large numbers by adding commas as thousands separators. It is commonly used when displaying numbers in a user interface, such as financial data, population figures, or any other numeric values that can benefit from improved readability.

### phoneNumber

The "phoneNumber" function is used to convert a string or number to a formatted phone number. It takes a single argument, which should be a string or number representing a phone number, and returns the phone number formatted in a standard format.

Example:

handleBars

```Text
{{ phoneNumber 8005551212 }}
```

In the above example, the "phoneNumber" function is called with the number `8005551212`. It formats the number as a phone number in the standard format and returns it as a string. The output would be `(800) 555-1212`.

The "phoneNumber" function is useful when you want to display phone numbers in a user-friendly format. It adds formatting elements such as parentheses for the area code and hyphens for separating the different parts of the phone number.

### toAbbr

The "toAbbr" function is used to abbreviate numbers to a given precision. It takes two arguments: the "number" to be abbreviated and the "precision" which determines the number of decimal places to be displayed in the abbreviated form. The "number" argument can be a number or a string representation of a number.

Example:

handleBars

```Text
{{ toAbbr 10123 2 }}
```

In the above example, the "toAbbr" function is called with the number `10123` and a precision of `2`. It abbreviates the number to a shorter form with the specified precision and returns it as a string. The output would be `10.12k`, indicating `10.12` thousand.

The "toAbbr" function is useful when you want to display large numbers in a more concise and readable format. It abbreviates the number by using common metric prefixes such as "k" for thousand, "M" for million, "B" for billion, and so on.

### toExponential

The "toExponential" function is used to convert a number to a string representation in exponential notation. It takes two arguments: the "number" to be converted and the "fractionDigits" which determines the number of digits after the decimal point in the exponential representation. The "number" argument can be a number or a string representation of a number.

Example:

handleBars

```Text
{{ toExponential 10123 2 }}
```

In the above example, the "toExponential" function is called with the number `10123` and a fractionDigits value of `2`. It converts the number to a string representation in exponential notation with two decimal places. The output would be `101e+4`, indicating `101` multiplied by `10` to the power of `4`.

### toFixed

The "toFixed" function is used to format a number using fixed-point notation. It takes two arguments: the "number" to be formatted and the "digits" which specifies the number of digits to appear after the decimal point.

Example:

handleBars

```Text
{{ toFixed 1.1234 2 }}
```

In the above example, the "toFixed" function is called with the number `1.1234` and a digits value of `2`. It formats the number using fixed-point notation with two decimal places. The output would be `1.12`.

### toFloat

The "toFloat" function is used to convert the input to a floating-point number. It takes one argument, which can be a number or a string representing a number.

Example:

handleBars

```Text
{{ toFloat "3.14" }}
```

In the above example, the "toFloat" function is called with the string `3.14`. It converts the string to a floating-point number and returns the numeric value. The output would be the floating-point number `3.14`.

### toInt

The "toInt" function is used to convert the input to an integer. It takes one argument, which can be a number or a string representing a number.

Example:

handleBars

```Text
{{ toInt "42" }}
```

In the above example, the "toInt" function is called with the string `42`. It converts the string to an integer and returns the numeric value. The output would be the integer `42`.

### toPrecision

The "toPrecision" function is used to format a number to the specified precision. It takes two arguments: the number to be formatted and the desired precision.

Example:

handleBars

```Text
{{ toPrecision 1.1234 2 }}
```

In the above example, the "toPrecision" function is called with the number `1.1234` and a precision of `2`. It formats the number to have two significant digits and returns a string representing the formatted number. The output would be `1.1`.

The "toPrecision" function is useful when you want to control the precision of a number and represent it as a string. It allows you to specify the number of significant digits that should be included in the output.
