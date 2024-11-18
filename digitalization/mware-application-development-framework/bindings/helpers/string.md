# String

### Append

The "append" function appends a specified suffix to a given string. It takes two arguments: the string to which the suffix will be appended, and the suffix itself. The function returns the original string with the suffix appended to it.

Example:

handleBars

```Text
{{ append 'index' '.html' }}
```

This example appends the suffix `.html` to the string `index` and returns `index.html`.

### Camelcase

The "camelcase" function converts a given string to camel case by removing spaces and capitalising the first letter of each word after the first word. It takes one argument, which is the string to be converted. The function returns the camel-cased version of the string.

Example:

handleBars

```Text
{{ camelcase 'foo bar baz' }}
```

This example converts the string `foo bar baz` to camel case and returns `fooBarBaz`.

### Capitalize

The "capitalize" function capitalises the first letter of a given string. It takes one argument, which is the string to be capitalised. The function returns the string with the first letter capitalised.

Example:

handleBars

```Text
{{ capitalize 'foo bar baz' }}
```

This example capitalises the first letter of the string `foo bar baz` and returns `Foo bar baz`.

### Capitalize all

The "capitalizeAll" function capitalises all words in a given string. It takes one argument, which is the string to be capitalised. The function returns the string with the first letter of each word capitalised.

Example:

handleBars

```Text
{{ capitalizeAll 'foo bar baz' }}
```

This example capitalises all words in the string `foo bar baz` and returns `Foo Bar Baz`.

### Center

The "center" function is used to center a string by adding non-breaking spaces around it. It takes two arguments: the string to be centered and the number of spaces to add on each side.

Example:

handleBars

```Text
{{ center 'test' 1 }}
```

This example centers the string "test" by adding one non-breaking space on each side, resulting in the string " test ".

Note: Non-breaking spaces are used to ensure that the string stays centered even if it is rendered on multiple lines.

### Chop

The "chop" function is used to remove extraneous whitespace and non-word characters from both the beginning and end of a string. It takes one argument, which is the string to be processed.

Example:

handleBars

```Text
{{ chop ' ABC '}}
```

This example removes both extraneous whitespace and non-word characters from the string `ABC`, resulting in the string `ABC`.

### Dashcase

The "dashcase" function is used to convert a string to dash-case format. It replaces non-word characters and periods with hyphens. This format is commonly used for URLs, CSS class names, and file names.

Example:

handleBars

```Text
{{ dashcase 'a-b-c d_e' }}
```

This example converts the string `a-b-c d_e` to dash-case format, resulting in the string `a-b-c-d-e`.

Note: In dash-case, all letters are lowercase and words are separated by hyphens. Non-word characters and periods are replaced with hyphens.

### Dot case

The "dotcase" function is used to convert a string to dot.case format. It separates words in the string with periods (dots).

Example:

handleBars

```Text
{{ dotcase 'a-b-c d_e' }}
```

This example converts the string `a-b-c d_e` to dot.case format, resulting in the string `a.b.c.d.e`.

In dot.case format, all letters are lowercase, and words are separated by periods. This format is often used for file extensions, object keys, and URLs.

### Down case

The "downcase" function is used to convert all characters in a string to lowercase.

Example:

handleBars

```Text
{{ downcase 'aBcDeF' }}
```

This example converts the string `aBcDeF` to lowercase, resulting in the string `abcdef`.

The "downcase" function is an alias for the "lowercase" function, and both can be used interchangeably to achieve the same result of converting a string to lowercase.

### Ellipsis

The "ellipsis" function is used to truncate a string to a specified length and append it with an ellipsis ("...").

Example:

handleBars

```Text
{{ ellipsis 'foo bar baz' 7 }}
```

This example truncates the string `foo bar baz` to a length of `7` characters and appends it with an ellipsis, resulting in the string `foo bar...`.

The "ellipsis" function is useful when you want to limit the length of a string and indicate that it has been truncated by adding an ellipsis at the end.

### Hyphenate

The "hyphenate" function is used to replace spaces in a string with hyphens `("-")`.

Example:

handleBars

```Text
{{ hyphenate 'foo bar baz qux' }}
```

This example replaces the spaces in the string `foo bar baz qux` with hyphens, resulting in the string `foo-bar-baz-qux`.

The "hyphenate" function is useful when you want to convert a string with spaces into a hyphen-separated string, which is often used in URLs or CSS class names.

### Is String

The "isString" function is used to determine whether a value is a string or not. It returns true if the value is a string and false otherwise.

Example:

handleBars

```Text
{{ isString 'foo' }}
```

This example checks whether the value `foo` is a string. Since `foo` is indeed a string, the function will return true.

The "isString" function is useful when you need to validate the type of a value in your template logic. It allows you to conditionally handle different types of values based on whether they are strings or not.

### lowercase

The lowercase function converts all characters in the given string to lowercase. This is useful for standardizing text for comparisons or display purposes.

handleBars

```Text
{{ lowercase str }}
```

#### Parameters

* str (String): The string you want to convert to lowercase.

**Example**

handleBars

```Text
{{ lowercase 'Foo BAR baZ' }}
```

**Output**

```
foo bar baz
```

The function transforms the input string to all lowercase characters in this example.

### Occurrences

The "occurrences" function is used to count the number of occurrences of a substring within a given string.

Example:

handleBars

```Text
{{ occurrences 'foo bar foo bar baz' 'foo' }}
```

In this example, the function is applied to the string `foo bar foo bar baz` and the substring `foo`. The function will count the number of occurrences of the substring within the string, resulting in the value `2`.

The "occurrences" function is useful when you need to determine the frequency of a particular substring within a larger string. It can be used for various purposes such as data analysis, text processing, or string manipulation.

### Pascal case

The "pascalcase" function is used to convert a string into PascalCase format, where each word within the string starts with an uppercase letter and the remaining letters are lowercase.

Example:

handleBars

```Text
{{ pascalcase 'foo bar baz' }}
```

In this example, the function is applied to the string `foo bar baz`. The function will convert the string to PascalCase format, resulting in the value `FooBarBaz`.

### Pathcase

The "pathcase" function is used to convert a string into path/case format, where each word within the string is separated by a forward slash `("/")`.

Example:

handleBars

```Text
{{ pathcase 'a-b-c d_e' }}
```

In this example, the function is applied to the string `a-b-c d_e'` The function will convert the string to path/case format, resulting in the value `a/b/c/d/e`.

The "pathcase" function is commonly used to format strings that represent paths or directory structures. It can be useful when working with file systems or URLs in various programming contexts.

### Plusify

The "plusify" function is used to replace spaces in a string with pluses `("+")`. It is commonly used for URL encoding or creating query strings.

Example:

handleBars

```Text
{{ plusify 'foo bar baz' }}
```

In this example, the function is applied to the string `foo bar baz`. The function will replace the spaces with pluses, resulting in the value `foo+bar+baz`.

The "plusify" function is helpful when you need to transform a string to a format where spaces are represented by pluses, such as when constructing URL parameters or query strings.

### Prepend

The "prepend" function is used to add a prefix to the beginning of a string.

Example:

```
{{ prepend 'bar' 'foo-' }}
```

In this example, the function is applied to the string 'bar' with the prefix `foo-`. The function will prepend the prefix to the string, resulting in the value `foo-bar`.

### Raw

The "raw" function is used to render a block of content without processing any Mustache templates inside the block.

Example:

handleBars

```Text
{{#raw}} {{foo}} {{/raw}}
```

In this example, the content within the "raw" block will be rendered as-is, without any Mustache template processing. Any occurrences of `{{foo}}` within the block will be treated as literal text and will not be evaluated as a Mustache variable.

The "raw" function is useful when you need to include Mustache template syntax or variables as plain text within your template, without them being interpreted as template directives.

### Remove

The "remove" function is used to remove all occurrences of a substring from a given string.

Example:

handleBars

```Text
{{ remove 'a b a b a b' 'a ' }}
```

In this example, the substring `a` will be removed from the string `a b a b a b`. The resulting string will be `b b b`.

The "remove" function is useful when you need to eliminate specific substrings from a string. It will remove all occurrences of the specified substring throughout the string.

### Remove First

The "removeFirst" function is used to remove the first occurrence of a substring from a given string.

Example:

handleBars

```Text
{{ removeFirst 'a b a b a b' 'a' }}
```

In this example, the first occurrence of the substring `a` will be removed from the string `a b a b a b`. The resulting string will be `b a b a b`.

The "removeFirst" function is useful when you want to remove only the first occurrence of a specific substring from a string while leaving the subsequent occurrences untouched.

### Replace

The "replace" function is used to replace all occurrences of a substring with another substring in a given string.

Example:

```
{{ replace 'a b a b a b' 'a' 'z' }}
```

In this example, all occurrences of the substring "a" will be replaced with the substring `z` in the string `a b a b a b`. The resulting string will be `z b z b z b`.

### Replace first

In this example, all occurrences of the substring `a` will be replaced with the substring `z` in the string `a b a b a b`. The resulting string will be `z b z b z b`.

"replaceFirst" function is used to replace the first occurrence of a substring with another substring in a given string.

Example:

handleBars

```Text
{{ replaceFirst 'a b a b a b' 'a' 'z' }}
```

In this example, the first occurrence of the substring `a` will be replaced with the substring `z` in the string `a b a b a b`. The resulting string will be `z b a b a b`.

### Sentence

The "sentence" function is used to convert the given string into sentence case, where the first letter of each sentence is capitalised and the rest of the letters are in lowercase.

Example:

```
{{ sentence 'hello world. goodbye world.' }}
```

In this example, the string "hello world. goodbye world." will be converted to sentence case. The resulting string will be `Hello world. Goodbye world.`

The "sentence" function is useful when you want to format a string to have proper capitalisation for each sentence.

### Snake case

The "snakecase" function is used to convert the given string into snake\_case, where spaces and non-word characters are replaced with underscores.

Example:

handleBars

```Text
{{ snakecase 'a-b-c d_e' }}
```

In this example, the string `a-b-c d_e` will be converted to snake\_case. The resulting string will be `a_b_c_d_e`.

The "snakecase" function is commonly used when you want to format a string to have consistent lowercase letters separated by underscores, which is a common naming convention in programming.

### Split

The "split" function is used to split a string into an array of substrings based on a specified character.

Example:

handleBars

```Text
{{ split 'a,b,c' }}
```

In this example, the string `a,b,c` will be split using the comma `(",")` as the separator. The resulting array will contain the substrings `['a', 'b', 'c']`.

The "split" function is useful when you need to break a string into smaller parts based on a specific delimiter, such as separating comma-separated values into individual elements of an array.

### Starts with

The "startsWith" function is used to test whether a string begins with a specified prefix.

Example:

handleBars

```Text
{{#startsWith 'Goodbye' 'Hello, world!'}} Yep {{else}} Nope {{/startsWith}}
```

In this example, the string `Hello, world!` is tested to see if it starts with the prefix `Goodbye`. Since it does not, the "else" block is executed, and the output will be `Nope`.

### Titleize

The "titleize" function is used to convert a string into title case, where the first letter of each word is capitalised.

Example:

handleBars

```Text
{{#titleize 'this is title case'}}
```

In this example, the string `this is title case` will be converted to title case, resulting in the output `This Is Title Case`.

The "titleize" function is useful when you want to format a string with title case, such as for titles, headings, or proper noun capitalisation.

### Trim

The "trim" function is used to remove extraneous whitespace from the beginning and end of a string.

Example:

handleBars

```Text
{{ trim '  ABC  ' }}
```

In this example, the string `ABC` will have the leading and trailing whitespace removed, resulting in the output `ABC`.

### Trim left

The "trimLeft" function is used to remove extraneous whitespace from the beginning of a string.

Example:

handleBars

```Text
{{ trimLeft '  ABC ' }}
```

In this example, the string `ABC` will have the leading whitespace removed, resulting in the output `ABC` .

### Trim right

The "trimRight" function is used to remove extraneous whitespace from the end of a string.

Using the previous example the only different this will do is trim everything on the right hand side of the string.

### Truncate

The "truncate" function is used to truncate a string to a specified length.

Example:

hanbdleBars

```Text
{{ truncate 'foo bar baz' 7 }}
```

In this example, the string `foo bar baz` will be truncated to a length of 7 characters, resulting in the output `foo bar`.

You can also provide an optional suffix parameter to append to the truncated string if it exceeds the specified length. For example:

handleBars

```Text
{{ truncate 'foo bar baz' 7 '...' }}
```

In this case, if the string is longer than `7` characters, it will be truncated to `foo bar...` with the suffix `...` added.

The "truncate" function is useful when you want to limit the length of a string, such as when displaying a preview or summary of a longer text.

### Truncate words

This is similar as the previous helper the only difference being instead of the optional number applying to the characters. It applies the truncation to the words instead.

The "truncateWords" function is useful when you want to limit the number of words in a string, such as when displaying a preview or summary of a longer text. It is similar to the "truncate" function, but instead of specifying a character length, you specify the desired number of words to keep.

### Upcase

The "upcase" function is used to convert all characters in a string to uppercase.

Example:

handleBars

```Text
{{ upcase 'aBcDef' }}
```

In this example, the string `aBcDef` will be converted to uppercase, resulting in the output `ABCDEF`.

The "upcase" function is useful when you need to transform the case of a string to uppercase for consistent formatting or comparison purposes. It is an alias for the "uppercase" function, which serves the same purpose.

### Uppercase

The "uppercase" function is used to convert all characters in a string to uppercase.

Example:

handleBars

```Text
{{ uppercase 'aBcDef' }} -> ABCDEF
```

In this example, the string "aBcDef" will be converted to uppercase, resulting in the output "ABCDEF".

The "uppercase" function can also be used as a block helper to uppercase the entire block of text. Here's an example of using it as a block helper:

handleBars

```Text
{{#uppercase}}
  This text will be converted to uppercase.
{{/uppercase}}
```

In this case, the entire block of text within the "uppercase" helper will be converted to uppercase.

Please note that the "uppercase" function does not support inverse blocks.

The "uppercase" function is useful when you need to transform the case of a string to uppercase for consistent formatting or comparison purposes.

### Lorem

The lorem function generates a string of Lorem Ipsum text based on the specified number of characters. This is useful for filling content areas with placeholder text during design and development.

handleBars

```Text
{{ lorem num }}
```

#### Parameters

* num (Number): The number of characters of Lorem Ipsum text you want to generate.

**Example**

handleBars

```Text
{{ lorem 250 }}
```

**Output**

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus imperdiet, nulla et dictum interdum, nisi lorem egestas odio, vitae scelerisque enim ligula venenatis dolor. Maecenas nisl est, ultrices nec congue eget, auctor vitae massa. Fusce luc
```

In this example, the function returns 250 characters of Lorem Ipsum text.
