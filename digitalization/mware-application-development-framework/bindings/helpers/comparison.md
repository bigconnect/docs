# Comparison

### And

The "and" helper is used to check if both of the given values are truthy. It can be used as a block helper, inline helper, or subexpression.

As a block helper, it renders the block if both values are truthy. If an inverse block is specified, it will be rendered when the values are falsey.

Example usage as a block helper:

handleBars

```Text
{{#and great magnificent}}
  both
{{else}}
  no
{{/and}}
```

In this example, if the values of `great` and "magnificent" are both truthy, the block `both` will be rendered. Otherwise, the inverse block `no` will be rendered.

The "and" helper can also be used as an inline helper or subexpression. Here's an example of using it as an inline helper:

handleBars

```Text
{{and great magnificent}}
```

In this case, the result of the "and" helper will be output directly.

The "and" helper is useful when you need to check if multiple values are all truthy before rendering a block of content or making a decision based on their combined truthiness.

### Compare

The "compare" helper is used to perform a comparison between two values and render a block if the comparison returns true. It takes four arguments: `a`, operator, `b`, and options.

The `a` and `b` arguments are the values to be compared. The operator argument specifies the arithmetic operator to use for the comparison. The available operators are:

* `==` or `===` : Equal to (strict equality)
* `!=` or `!==` : Not equal to (strict inequality)
* `<` : Less than
* `>` : Greater than
* `<=` : Less than or equal to
* `=` : Greater than or equal to

Example usage:

handleBars

```Text
{{#compare 10 '<' 5}}
  This block will be rendered because 10 is less than 5.
{{else}}
  This block will not be rendered.
{{/compare}}
```

In this example, the comparison `10 < 5` evaluates to false, so the inverse block is not rendered. If the comparison were true, the main block would be rendered.

You can also use the "compare" helper as an inline helper or subexpression to directly output the result of the comparison.

### Contains

The "contains" helper is a block helper that checks if a given value exists in a collection. It takes four arguments: collection, value, startIndex (optional), and options.

The collection argument represents the collection in which the search is performed. It can be an array or a string.

The value argument is the value to search for within the collection. The helper uses strict equality `(===)` for comparison.

The startIndex argument is an optional parameter that specifies the index at which to start the search. If a negative value is provided, it is treated as an offset from the end of the collection. By default, the search starts from index `0`.

Example usage:

handleBars

```Text
{{#contains ['a', 'b', 'c'] 'd'}}
	This will not be rendered. 
{{else}} 
 This will be rendered. 
{{/contains}}
```

In this example, the collection `['a', 'b', 'c']` contains the value `d`, so the else block is rendered. If the value were found in the collection, the main block would be rendered.

You can also use the "contains" helper with strings as the collection:

handleBars

```Text
{{#contains 'Hello, world!' 'world'}}
  This block will be rendered because 'world' is found in the string.
{{else}}
  This block will not be rendered.
{{/contains}}
```

In this case, the string `Hello, world!` contains the substring `world`, so the main block is rendered.

### Default

The "default" helper is used to return the first value that is not undefined. It takes two arguments: value and defaultValue.

The `value` argument represents the value that will be checked. If this value is not undefined, it will be returned as the result.

The `defaultValue` argument is the fallback value that will be returned if the value is undefined.

Example usage:

handleBars

```Text
{{default variable 'N/A'}}
```

In this example, the "default" helper checks the value of the `variable`. If `variable` is defined (not undefined), its value will be returned. If variable is `undefined`, the fallback value `N/A` will be returned.

Here's another example:

handleBars

```Text
{{default null 'default'}}
```

In this case, the `value` is explicitly set to `null`. Since `null` is defined (not undefined), the "default" helper will return `null`, and the fallback value `default` will not be used.

### Eq

The "eq" helper is a block helper that compares two values, `a` and `b`, and renders a block of content if they are equal. It takes three arguments: `a, b, and options`.

If a is equal to `b`, the block of content within the "eq" helper will be rendered. If `a` is not equal to `b`, and an inverse block is specified, the inverse block will be rendered instead.

Here's an example usage of the "eq" helper:

handleBars

```Text
{{#eq 3 3}} 
  equal
{{else}} 
  not equal
{{/eq}}
```

In this example, `a` is set to `3` and `b` is also set to `3`. Since `3` is equal to `3`, the block within the "eq" helper will be rendered, and the output will be "equal".

You can also use the compare hash argument to specify the second value:

handleBars

```Text
{{#eq value compare=3}}
	equal
{{else}} 
 	not equal
{{/eq}}
```

In this case, the value of value will be compared to `3`. If they are equal, the block will be rendered; otherwise, the inverse block will be rendered.

Note that the "eq" helper performs strict equality comparison `(===)`.

### Gt

The "gt" helper is a block helper that compares two values, a and b, and renders a block of content if a is greater than b. It takes three arguments: `a, b, and options`.

If a is greater than `b,` the block of content within the "gt" helper will be rendered. If a is not greater than `b`, and an inverse block is specified, the inverse block will be rendered instead.

Here's an example usage of the "gt" helper:

handleBars

```Text
{{#gt 4 3}} 
	greater than
{{else}} 
 	not greater than
{{/gt}}
```

In this example, `a` is set to `4` and `b` is set to `3`. Since `4` is greater than `3`, the block within the "gt" helper will be rendered, and the output will be "greater than".

You can also use the `compare` hash argument to specify the second value:

handleBars

```Text
{{#gt value compare=3}} 
	greater than
{{else}} 
 	not greater than
{{/gt}}
```

In this case, the value of value will be compared to `3`. If value is greater than `3`, the block will be rendered; otherwise, the inverse block will be rendered.

Note that the "gt" helper performs numerical comparison, so it expects numeric values for `a and b`.

### Gte

The "gte" helper is a block helper that compares two values, a and b, and renders a block of content if `a` is greater than or equal to `b`. It takes three arguments: `a, b, and options`.

If a is greater than or equal to `b`, the block of content within the "gte" helper will be rendered. If a is not greater than or equal to `b`, and an inverse block is specified, the inverse block will be rendered instead.

Here's an example usage of the "gte" helper:

handleBars

```Text
{{#gte 4 3}} 
	greater than or equal
{{else}} 
	not greater than
{{/gte}}
```

In this example, `a` is set to `4` and `b` is set to `3`. Since `4` is greater than or equal to `3`, the block within the "gte" helper will be rendered, and the output will be "greater than or equal".

You can also use the `compare` hash argument to specify the second value:

handleBars

```Text
{{#gte value compare=3}}
	greater than or equal
{{else}} 
	not greater than
{{/gte}}
```

In this case, the value of value will be compared to `3`. If value is greater than or equal to `3`, the block will be rendered; otherwise, the inverse block will be rendered.

Note that the "gte" helper performs numerical comparison, so it expects numeric values for `a and b`.

### Has

The "has" helper is a block helper that checks if a given value, val, contains a specific pattern, pattern. It takes three arguments: `val, pattern, and options`.

If the val contains the pattern, the block of content within the "has" helper will be rendered. If the val does not contain the pattern, and an inverse block is specified, the inverse block will be rendered instead.

Here's an example usage of the "has" helper:

handleBars

```Text
{{#has 'foobar' 'foo'}} 
	has it
{{else}} 
 	doesn't
{{/has}}
```

In this example, the val is set to 'foobar' and the pattern is set to 'foo'. Since the val contains the pattern, the block within the "has" helper will be rendered, and the output will be "has it".

You can use the "has" helper to perform pattern matching within a string and conditionally render content based on whether the pattern exists in the value.

### Isfalsey

The "isFalsey" helper is a helper that checks if a given value, val, is falsey. It takes two arguments: `val and options`.

If the val is considered falsey according to the falsey library, the helper will return `true.` Otherwise, it will return `false`.

Here's an example usage of the "isFalsey" helper:

handleBars

```Text
{{isFalsey '' }}
```

In this example, the `val` is an empty string. Since an empty string is considered falsey, the helper will return true.

### Istruthy

The "isTruthy" helper is similar to the "isFalsey" helper, but it checks if a given value, val, is truthy. It also takes two arguments: `val and options`.

If the val is considered truthy according to the falsey library, the helper will return`true`. Otherwise, it will return `false`.

Here's an example usage of the "isTruthy" helper:

handleBars

```Text
{{isTruthy '12' }}
```

In this example, the val is the string '12'. Since a non-empty string is considered truthy, the helper will return true.

Both the "isFalsey" and "isTruthy" helpers rely on the falsey library for determining truthiness and falseyness. You can refer to that library for more information on how values are evaluated.

### Ifeven

The "ifEven" helper is a helper that checks if a given value, number, is an even number. It takes two arguments: number and options.

If the number is an even number, the helper will render the block specified by the "if" section `{{#ifEven}}`. If the number is not an even number, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "ifEven" helper:

handleBars

```Text
{{#ifEven 2}}
  even
{{else}}
  odd
{{/ifEven}}
```

In this example, the number is `2`. Since `2` is an even number, the helper will render the block inside the "if" section, which outputs "even".

If you provide an odd number as the number argument, the block inside the "else" section will be rendered instead, outputting "odd".

The "ifEven" helper simplifies the process of checking if a number is even within your Handlebars templates.

### Ifnth

The "ifNth" helper is a conditional helper that checks if the remainder is zero when the a operand is divided by the b operand. It takes three arguments: `a, b, and options`.

If the remainder is zero when a is divided by `b`, the helper will render the block specified by the "if" section `{{#ifNth}}`. If the remainder is not zero, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "ifNth" helper:

handleBars

```Text
{{#ifNth 10 2}}
  remainder
{{else}}
  no remainder
{{/ifNth}}
```

In this example, `a` is `10` and `b` is `2`. When `10` is divided by `2`, the remainder is `zero`. Therefore, the helper will render the block inside the "if" section, which outputs "remainder".

If you provide operands where the remainder is not zero, the block inside the "else" section will be rendered instead, outputting "no remainder".

The "ifNth" helper allows you to conditionally render blocks based on the remainder of a division operation within your Handlebars templates.

### Ifodd

The "ifOdd" helper is a block helper that checks if the given value is an odd number. It takes two arguments: value and options.

If the value is an odd number, the helper will render the block specified by the "if" section `{{#ifOdd}}`. If the value is not an odd number, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "ifOdd" helper:

handleBars

```Text
{{#ifOdd 3}}
  odd
{{else}}
  even
{{/ifOdd}}
```

In this example, the value is `3`, which is an odd number. Therefore, the helper will render the block inside the "if" section, which outputs "odd".

If you provide an even number as the value, the block inside the "else" section will be rendered instead, outputting `even`.

The "ifOdd" helper allows you to conditionally render blocks based on whether a value is an odd number or not in your Handlebars templates.

### Is

The "is" helper is a block helper that checks if the value of a is equal to the value of `b`. It takes three arguments: `a, b, and options`.

If the values of `a` and `b` are equal, the helper will render the block specified by the "if" section `{{#is}}`. If the values are not equal, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "is" helper:

handleBars

```Text
{{#is 3 3}}
  is
{{else}}
  is not
{{/is}}
```

In this example, the values of `a` and `b` are both 3, which are equal. Therefore, the helper will render the block inside the "if" section, which outputs "is".

If you provide different values for a and `b`, the block inside the "else" section will be rendered instead, outputting "is not".

The "is" helper allows you to compare two values for equality in your Handlebars templates. Unlike the "eq" helper, the "is" helper does not perform strict equality comparison.

### Isnt

The "isnt" helper is a block helper that checks if the value of a is not equal to the value of b. It takes three arguments: `a, b, and options`.

If the values of a and b are not equal, the helper will render the block specified by the "if" section `{{#isnt}}`. If the values are equal, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "isnt" helper:

handleBars

```Text
{{#isnt 3 3}}
  isnt
{{else}}
  is
{{/isnt}}
```

In this example, the values of a and b are both 3, which are equal. Therefore, the helper will render the block inside the "else" section, which outputs "is".

If you provide different values for a and b, the block inside the "if" section will be rendered instead, outputting "isnt".

The "isnt" helper allows you to compare two values for inequality in your Handlebars templates. Unlike the "unlesseq" helper, the "isnt" helper does not use strict equality for comparisons.

### Lt

The "lt" helper is a block helper that checks if the value of a is less than the value of `b`. It takes two arguments: `a and b`, along with the options object.

If a is indeed less than `b`, the helper will render the block specified by the "if" section `{{#lt}}`. If a is not less than `b`, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "lt" helper:

handleBars

```Text
{{#lt 2 3}}
  less than
{{else}}
  more than or equal
{{/lt}}
```

In this example,`a` is `2` and `b` is `3`. Since 2 is less than 3, the helper will render the block inside the "if" section, which outputs "less than".

If you provide different values for a and b where a is not less than b, the block inside the "else" section will be rendered instead, outputting "more than or equal".

The "lt" helper allows you to compare two values to check if one is less than the other in your Handlebars templates.

### Ite

The "lte" helper is a block helper that checks if the value of `a` is less than or equal to the value of `b`. It takes two arguments: `a and b`, along with the options object.

If a is less than or equal to `b`, the helper will render the block specified by the "if" section `{{#lte}}`. If a is greater than `b`, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "lte" helper:

handleBars

```Text
{{#lte 2 3}}
  less than or equal
{{else}}
  more than
{{/lte}}
```

In this example, `a` is 2 and `b` is 3. Since 2 is less than or equal to 3, the helper will render the block inside the "if" section, which outputs "less than or equal".

If you provide different values for `a` and `b` where a is greater than `b`, the block inside the "else" section will be rendered instead, outputting "more than".

The "lte" helper allows you to compare two values to check if one is less than or equal to the other in your Handlebars templates.

### Neither

The "neither" helper is a block helper that checks if neither of the given values are truthy. It takes two arguments: a and b, along with the options object.

If both a and b are falsy values (i.e., not truthy), the helper will render the block specified by the "if" section `{{#neither}}`. If either a or b (or both) are truthy, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "neither" helper:

handleBars

```Text
{{#neither null null}}
  both falsey
{{else}}
  both not falsey
{{/neither}}
```

In this example, both a and b are null, which are falsey values. Therefore, the helper will render the block inside the "if" section, which outputs "both falsey".

If you provide different values for a and b where at least one of them is truthy, the block inside the "else" section will be rendered instead, outputting "both not falsey".

The "neither" helper allows you to check if neither of the given values are truthy in your Handlebars templates.

### Not

The "not" helper is used to check if a value is falsey. It takes one argument, val, and an optional options object.

If the val is falsey (i.e., undefined, null, false, 0, "", or NaN), the helper will render the block specified by the "if" section `{{#not}}`. If the val is truthy, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "not" helper:

handleBars

```Text
{{#not undefined}}
  falsey
{{else}}
  not falsey
{{/not}}
```

In this example, the val is undefined, which is a falsey value. Therefore, the helper will render the block inside the "if" section, which outputs "falsey".

If you provide a truthy value for val, the block inside the "else" section will be rendered instead, outputting "not falsey".

The "not" helper allows you to check if a value is falsey in your Handlebars templates.

### Or

The "or" helper is used to check if any of the given values are truthy. It takes multiple arguments separated by spaces and an optional options object.

If at least one of the given values is truthy, the helper will render the block specified by the "if" section `{{#or}}`. If all of the given values are falsey, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "or" helper:

handleBars

```Text
{{#or 1 2 undefined}}
  at least one truthy
{{else}}
  all falsey
{{/or}}
```

In this example, the first two values `(1 and 2)` are truthy, while the third value (undefined) is falsey. Therefore, the helper will render the block inside the "if" section, which outputs "at least one truthy".

If all of the provided values are falsey, the block inside the "else" section will be rendered instead, outputting "all falsey".

The "or" helper allows you to check if any of the given values are truthy in your Handlebars templates.

### Unlesseq

The "unlessEq" helper is a block helper that renders the inverse block unless the value of a is equal to the value of `b`. It takes two values, `a and b`, as arguments and an optional options object.

If a is not equal to `b`, the helper will render the block specified by the "if" section `{{#unlessEq}}`. However, if a is equal to `b`, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "unlessEq" helper:

handleBars

```Text
{{#unlessEq 2 1}}
  not equal
{{else}}
  equal
{{/unlessEq}}
```

In this example, `a` is 2 and `b` is 1. Since a is not equal to `b`, the helper will render the block inside the "if" section, which outputs "not equal".

If a were equal to b, the block inside the "else" section would be rendered instead, outputting "equal".

The "unlessEq" helper provides a convenient way to conditionally render blocks based on the inequality of two values in your Handlebars templates.

### Unlessgt

The "unlessGt" helper is a block helper that renders the inverse block unless the value of a is greater than the value of `b`. It takes two values, `a and b`, as arguments and an optional options object.

If `a` is not greater than `b`, the helper will render the block specified by the "if" section `{{#unlessGt}}`. However, if a is greater than `b`, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "unlessGt" helper

handleBars

```Text
{{#unlessGt 20 1}}
  not greater than
{{else}}
  greater than
{{/unlessGt}}
```

In this example, `a` is 20 and `b` is 1. Since a is greater than `b`, the helper will render the block inside the "else" section, which outputs "greater than".

If a were not greater than `b`, the block inside the "if" section would be rendered instead, outputting "not greater than".

The "unlessGt" helper provides a convenient way to conditionally render blocks based on the comparison of two values in your Handlebars templates.

### Unlesslt

The "unlessLt" helper is a block helper that renders the inverse block unless the value of a is less than the value of `b`. It takes two values, `a and b`, as arguments and an optional options object.

If a is not less than `b`, the helper will render the block specified by the "if" section `{{#unlessLt}}`. However, if a is less than `b`, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "unlessLt" helper:

handleBars

```Text
{{#unlessLt 20 1}}
  greater than or equal
{{else}}
  less than
{{/unlessLt}}
```

In this example, `a` is 20 and `b` is 1. Since a is greater than `b`, the helper will render the block inside the "if" section, which outputs "greater than or equal".

If a were less than `b`, the block inside the "else" section would be rendered instead, outputting "less than".

The "unlessLt" helper provides a convenient way to conditionally render blocks based on the comparison of two values in your Handlebars templates.

### Unlessgteq

The "unlessGteq" helper is a block helper that renders the inverse block unless the value of `a` is greater than or equal to the value of `b`. It takes two values, `a and b`, as arguments and an optional options object.

If `a` is not greater than or equal to `b`, the helper will render the block specified by the "if" section `{{#unlessGteq}}`. However, if `a` is greater than or equal to `b`, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "unlessGteq" helper:

handleBars

```Text
{{#unlessGteq 20 1}}
  less than
{{else}}
  greater than or equal to
{{/unlessGteq}}
```

In this example, `a` is 20 and b is 1. Since `a` is greater than or equal to `b`, the helper will render the block inside the "else" section, which outputs "greater than or equal to".

If a were less than `b`, the block inside the "if" section would be rendered instead, outputting "less than".

The "unlessGteq" helper provides a convenient way to conditionally render blocks based on the comparison of two values in your Handlebars templates.

### Unlesslteq

The "unlessLteq" helper is a block helper that renders the inverse block unless the value of `a` is less than or equal to the value of `b`. It takes two values, `a and b`, as arguments and an optional options object.

If `a` is not less than or equal to `b`, the helper will render the block specified by the "if" section `{{#unlessLteq}}`. However, if a is less than or equal to `b`, the helper will render the block specified by the "else" section `{{else}}`.

Here's an example usage of the "unlessLteq" helper:

handleBars

```Text
{{#unlessLteq 20 1}}
  greater than
{{else}}
  less than or equal to
{{/unlessLteq}}
```

In this example, `a` is 20 and `b` is 1. Since a is not less than or equal to `b`, the helper will render the block inside the "if" section, which outputs "greater than".

If `a` were less than or equal to `b`, the block inside the "else" section would be rendered instead, outputting "less than or equal to".

The "unlessLteq" helper allows you to conditionally render blocks based on the comparison of two values in your Handlebars templates.
