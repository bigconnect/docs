# String Functions

Use string functions to transform string data.

You can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following string functions:

### str:concat(\<string1>, \<string2>)

Concatenates two strings together.Uses the following arguments:

* string1 - The first string to concatenate.
* string2 - The second string to concatenate.

Use the str:concat function to concatenate strings within an expression. For example, to concatenate the FirstName field with the LastName field and then check if that result matches a pattern, use the following expression:${str:matches(str:concat(record:value("/FirstName") , record:value("/LastName")) , "\[A-Z]\[a-z]+\[A-Z]\[a-z]+")}str:contains(\<string>, \<subset>)

Returns true or false based on whether the string contains the configured subset of characters.

Uses the following arguments:

* string - The string to evaluate.
* subset - The subset of characters to look for.

For example, ${str:contains("Jane", "boo")} returns: false.

### str:endsWith(\<string>, \<subset>)

Returns true or false based on whether the string ends with the configured subset of characters.Uses the following arguments:

* string - The string to evaluate.
* subset - The subset of characters to look for.

For example, ${str:endsWith("32403-1001", "1001")} returns: true.str:escapeXML10(\<string>)

Returns a string that you can embed in an XML 1.0 or 1.1 document.

Uses the following argument:

* string - The string to escape.

Return type: String.

For example, you can use the following expression to generate XML-safe string data based on the XML in the XMLdata field: `${str:escapeXML10(record:value('/XMLdata'))}`

### str:escapeXML11(\<string>)

Returns a string that you can embed in an XML 1.1 document.Uses the following argument:

* string - The string to escape.

Return type: String.

For example, you can use the following expression to generate XML-safe string data based on the XML in the XMLdata field: `${str:escapeXML11(record:value('/XMLdata'))}`

### str:indexOf(\<string>, \<subset>)

Returns the index within a string of the first occurrence of the specified subset of characters.Return type: Integer.

Uses the following arguments:

* string - The string to return the index of.
* subset - The subset of characters to look for.

For example, `${str:indexOf("pepper", "pe")}` returns: `0`.

### str:isNullOrEmpty(\<string>)

Returns true or false based on whether a string is null or is the empty string.

Uses the following argument:

* string - The string to evaluate.

For example, ${str:isNullOrEmpty(record:value("/work\_phone"))} returns true if the work\_phone field is the empty string.

### str:lastIndexOf(\<string>, \<subset>)

Returns the index within a string of the last occurrence of the specified subset of characters.

Return type: Integer.

Uses the following arguments:

* string - The string to return the index of.
* subset - The subset of characters to look for.

For example, `${str:lastIndexOf("pepper", "pe")}` returns: `3`.

### str:length(\<string>)

Returns the length of a string.Return type: Integer.Uses the following argument:

* string - The string to return the length for.

For example, ${str:length("tomorrow")} returns: 8.

### str:matches(\<string>, \<regEx>)

Returns true or false based on whether a string matches a Java regex pattern.Uses the following arguments:

* string - The string to evaluate.
* regEx - Regular expression that describes the pattern of the string.

For example, ${str:matches(record:value("/HostName"), ".\*\\.com$")} checks if the string in the HostName field ends with the characters ".com".

### str:regExCapture(\<string>, \<regEx>, \<group>)

Parses a complex string into groups based on a Java regex pattern and returns the specified group.Uses the following arguments:

* string - The string that contains a pattern of characters.
* regEx - Regular expression that describes the pattern of the string, separating it into groups. Use the backslash as an escape character for special characters in the expression. For example, to represent a digit in the expression with the characters `\d`, use `\\d`.
* group - The number of the group to return, where 1 represents the first group, 2 represents the second group, etc. 0 returns the entire string.

For example, to extract the month and day from the Date field that uses the format 'mm-dd-yyyy', use the following function: ${str:regExCapture(record:value('/Date'), '(\\\w{2}\\\\-\\\w{2})-\\\w\*', 1)}

### str:replace(\<string>, \<oldChar>, \<newChar>)

Replaces all instances of a specified character in a string with a new character.

Uses the following arguments:

* string - The string that contains the character to replace.
* oldChar - Character to replace.
* newChar - Character to use for replacement.

For example, ${str:replace("lecucereche", "e", "a")} returns: lacucaracha.

### str:replaceAll(\<string>, \<regEx>, \<newString>)

Replaces a set of characters in a string with a new set of characters.Uses the following arguments:

* string - The string that contains the group of characters to replace.
* regEx - A regular expression that describes the string to replace.
* newString - The set of characters to use for replacement.

For example, ${str:replaceAll("shoes\_sandals","^shoes","footwear")} returns: footwear\_sandals.

### str:split(\<string>, \<separator>)

Splits a string into a list of strings based on the specified separator.Uses the following arguments:

* string - An input string.
* separator - The set of characters that designate a string split.

For example, suppose a record contains a `dimensions` string field that records a height, width, and length: "height=32 width=24 length=36". The function `${str:split(record:value("/dimensions"), " ")}` returns the following list of strings:

```
"height-32", "width=24", "length=36"
```

### str:splitKV(\<string>, \<pairSeparator>, \<keyValueSeparator>)

Splits key-value pairs in a string into a map of string values.Uses the following arguments:

* string - The string containing the key-value pairs.
* pairSeparator - The set of characters that separate the key-value pairs.
* keyValueSeparator - The set of characters that separate each key and value.

For example, suppose a record contains a `dimensions` string field that records a height, width, and length: "height=32 width=24 length=36". The function `${str:splitKV(record:value("/dimensions"), " ", "=")}` returns the following map of string values:

```
"dimensions": {
      "height": "32",
      "width": "24",
      "length": "36"
    }
```

### str:startsWith(\<string>, \<subset>)

Returns true or false based on whether the string starts with the configured subset of characters.Uses the following arguments:

* string - The string to evaluate.
* subset - The subset of characters to look for.

For example, ${str:startsWith("Data Collector", "Data")} returns: true.

### str:substring(\<string>, \<beginIndex>, \<endIndex>)

Returns a subset of the string value that starts with the beginIndex character and ends one character before the endIndex.

Uses the following arguments:

* string - The string that contains the return substring that you want.
* beginIndex - An integer that represents the beginning position of the returned substring. Start the count from the left with 0.
* endIndex - An integer that represents one position past the end of the substring.

For example, ${str:substring("Chewing Gum", 0, 5)} returns: Chew.

### str:toLower(\<string>)

Converts string data to all lowercase letters.Uses the following argument:

* string - The string to lower case.

For example, ${str:toLower("FALSE")} returns: false.

### str:toUpper(\<string>)

Converts string data to all capital letters.Uses the following argument:

* string - The string to capitalize.

For example, ${str:toUpper("true")} returns: TRUE.

### str:trim(\<string>)

Trims leading and trailing white space characters from a string, including spaces and return characters.Uses the following argument:

* string - The string to return without additional white space characters.

For example, ${str:trim(record:value("/Address"))} trims leading and trailing white space characters from the Address field.

### str:truncate(\<string>, \<length>)

Returns a string truncated to the specified length. Use an integer to specify the length.Uses the following arguments:

* string - The string to truncate.
* length - An integer that represents the number of characters to keep.

For example, when a phone number field includes string numbers such as 415-555-5555, ${str:truncate(record:value('/phone'),3)} returns the area code of the phone number.

### str:unescapeJava(\<string>)

Returns an unescaped string from a string with special Java characters. Use to include binary or non-printable characters in any location where you can enter an expression.

Uses the following argument:

* string - The string to process.

### str:unescapeXML(\<string>)

Returns an unescaped string from a string that had XML data escaped.Uses the following argument:

* string - The string that includes escaped XML data.

For example, say you have escaped XML data embedded within an XML document in an additionalXML field. The following expression unescapes the XML: `${str:unescapeXML(record:value('/additionalXML'))}`

### str:urlDecode(\<URL>, \<charset>)

Converts characters from a URL to the specified character set, such as UTF-8.Uses the following arguments:

* URL - The URL or field of URLs to decode.
* charset - The character set to apply to the URL. For information about valid encodings, see [https://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html](https://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html).

Return value: String.

For example, to convert a URL to UTF-8, use the following expression:

```
${str:urlDecode(record:value('/URL'), 'UTF-8')}
```

### str:urlEncode(\<infoforURL>, \<charset>)

Converts invalid characters to help create a valid URL based on the specified character set, such as UTF-8. You might use this function when using record data to add additional information, like a fragment, to a URL.

Uses the following arguments:

* infoforURL - The information to encode for a URL.
* charset - The character set to convert to, such as UTF-8. For information about valid encodings, see [https://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html](https://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html).

Return value: String.

For example, to encode a search result for use in a URL, converting spaces to valid characters, you might use the following expression:

```
${str:urlEncode(record:value('/result'), 'UTF-8')}
```
