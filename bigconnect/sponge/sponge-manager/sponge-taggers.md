# Taggers

A Tagger allows you to add to or modify existing document fields.&#x20;

They can be executed Pre or Post Stage:

* **Pre -** run before parsing the document content
* **Post** - run after parsing the document content

## **DOMTagger**

Extract the value of DOM elements into target fields using CSS selectors.

![BigConnect Sponge DOMTagger](<../../../.gitbook/assets/image (585).png>)

You can add one or more selectors to this tagger. Each selector has the following configuration properties:

* **CSS selector -** the CSS selector to identify a DOM element (e.g. `div.title h1 span` ) to extract a value from.
* **Target field -** the name of a document field where to place the extracted value. The field will be created if it doesn't exist.&#x20;
* **Default value -** if no value could be extracted with the provided CSS selector, this is the default value that will be written in the Target field.&#x20;
* **Extract -** what to extract from the DOM element identified by the CSS selector:
  * **text** - Default option when extract is blank. The text of the element, including combined children.
  * **html** - Extracts an element inner HTML (including children).
  * **outerHtml** - Extracts an element outer HTML (like "html", but includes the "current" tag).
  * **ownText** - Extracts the text owned by this element only; does not get the combined text of all children.
* **Overwrite** - whether to overwrite the Target field if it already exists.
* **Match blanks** - you can set this to true to match elements that are present but have blank values. Blank values are empty values or values containing white spaces only. Because white spaces are normalized by the DOM parser, such matches will always return an empty string (spaces will be trimmed). By default elements with blank values are not matched and are ignored.

## CharacterCaseTagger

Changes the character case of field value&#x73;**.**

![](<../../../.gitbook/assets/image (434).png>)

* **field -** the name of the document field to change
* **caseType** -  method used to change the character case:
  * Changes all characters to upper case.
  * Changes all characters values to lower case.
  * Converts the first letter of each words to upper case, and the rest to lower case.
  * Converts the first letter of a string if not numeric, and leaves the character case of other characters unchanged.
  * Converts all upper case characters to lower case, and all lower case to upper case
* **applyTo** - the change of character case can be applied to one of the following (defaults to "field values" when unspecified)
  * Applies to the field values.
  * Applies to the field name.
  * Applies to both the field name and its values.

## ConstantTagger

Define and add constant fields to a document.

![](<../../../.gitbook/assets/image (524).png>)

* **name -** the name of the field to add
* **value** - value of the field

## CopyTagger

Copies document fields.

![](<../../../.gitbook/assets/image (557).png>)

* **fromField** - source field
* **tofield** - target field
* **overwrite** - whether to overwrite the target field if it already exists.

## CountMatchesTagger

Counts the number of matches of a given string (or string pattern) and store the resulting value in a field in the specified **toField**.

If no **fromField** is specified, the document content will be used. If the **toField** already exists before counting begins, it will be overwritten with the result of the match count. If within this tagger the **toField** is repeated, the sum of all count will be added. If the **fromField** has multiple values, the total count of all matches will be stored as a single value.

Can be used as a **Pre**-Stage on text document only when matching strings on document content, or both as a **Pre**-Stage or **Post**-Stage when the **fromField** is used.

![](<../../../.gitbook/assets/image (545).png>)

* **fromField** - source field
* **toField** - target field
* **value** - text to match or regular expression
* **caseSensitive** - perform matching in a case-sensitive way
* **regex** - if the provided **value** is a [regex expression ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html)or not

## CurrentDateTagger

Adds the server UTC date to the specified field. If no field is provided, the date will be added to the field `document.importedDate`. The default date format is EPOCH (the difference, measured in milliseconds, between the current time and midnight, January 1, 1970 UTC).&#x20;

![](<../../../.gitbook/assets/image (474).png>)

* **field** - target field
* **format** - a custom date format, as per the formatting options found on [SimpleDateFormat](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/SimpleDateFormat.html).
* **locale** - the ISO two-letter language code, with an optional ISO country code, separated with an underscore (e.g., "fr" for French, "fr\_CA" for Canadian French). When no locale is specified, the default is "en\_US" (US English).
* **overwrite** - if the target field already has one or more values, the new date will be added to the list of existing values, unless "overwrite" is set to true.

## DateFormatTagger

Formats a date from any given format to a format of choice, as per the formatting options found on [SimpleDateFormat](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/SimpleDateFormat.html) with the exception of the string "EPOCH" which represents the difference, measured in milliseconds, between the date and midnight, January 1, 1970. The default format for **fromFormat** or **toFormat** when not specified is EPOCH.

![](<../../../.gitbook/assets/image (511).png>)



* **fromField** - source field
* **toField** - target field. When omitting this, the value will replace the one in the same field.&#x20;
* **fromLocale** - source locale. The locale is the ISO two-letter language code, with an optional ISO country code, separated with an underscore (e.g., "fr" for French, "fr\_CA" for Canadian French). When no locale is specified, the default is "en\_US" (US English).
* **toLocale** - target locale. The locale is the ISO two-letter language code, with an optional ISO country code, separated with an underscore (e.g., "fr" for French, "fr\_CA" for Canadian French). When no locale is specified, the default is "en\_US" (US English).
* **fromFormats** - source date format as per the formatting options found on [SimpleDateFormat](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/SimpleDateFormat.html) with the exception of the string "EPOCH" which represents the difference, measured in milliseconds, between the date and midnight, January 1, 1970. The default format when not specified is EPOCH. You can specify multiple formats separated by `!!` Each format will be tried in the order provided and the first format that succeed in parsing a date will be used. A date will be considered "bad" only if none of the formats could parse the date.
* **toFormat** - target date format as per the formatting options found on SimpleDateFormat with the exception of the string "EPOCH" which represents the difference, measured in milliseconds, between the date and midnight, January 1, 1970. The default format when not specified is EPOCH.
* **keepBadDates** - place the source date in the target field, even if it could not be parsed successfully.
* **overwrite** - If the **toField** already exists, the newly formatted date will be added to the list of existing values, unless this is set to `true`.

## DeleteTagger

Delete the document fields provided.

![](<../../../.gitbook/assets/image (418).png>)

* **fields** - coma-separated list of fields to delete

## KeepOnlyTagger

Keep only the document fields provided, delete all other ones.

![](<../../../.gitbook/assets/image (421).png>)

{% hint style="info" %}
Unless you have good reasons for doing otherwise, it is recommended to use this tagger as one of the last ones to be executed. This is a good practice to ensure all document fields are available to other taggers and filters that may require them even if they are not otherwise required.
{% endhint %}

* **fields** - coma-separated list of fields to keep

## MergeTagger

Merge multiple document fields into a single one.

![](<../../../.gitbook/assets/image (477).png>)

* **fromFields** - list all fields to merge, separated by commas
* **toField** - target field to store all values combined into one string, optionally separated by a **separator**
* **separator** - separator to use when joining source field values
* **deleteFromFields** -  delete source fields after they were merged

## RenameTagger

Rename document fields to different names.

![](<../../../.gitbook/assets/image (587).png>)

* **fromField** - source field name
* **toField** - target field name
* **overwrite** - If the target field already exists, the values of the original field name will be added, unless this is set to true.

## ReplaceTagger

Replaces an existing document field value with another one. The same field specified in **fromField** will be used for the replacement.

![](<../../../.gitbook/assets/image (463).png>)

* **fromField** - source field
* **fromValue** - source field value
* **toValue** - replacement value
* **caseSensitive** - whether to match in case-sensitive way
* **regex** - if **fromValue** is a [Regular Expression](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html)
* **wholeMatch** - whether matches should be made against the whole field value or not (default)
* **replaceAll** - only applicable when **wholeMatch** is false

## ScriptTagger

Tag incoming documents using a scripting language. The default script engine is JavaScript, version [ECMAScript Edition 5.1](http://www.ecma-international.org/ecma-262/5.1/).

The following are variables are made available to your script for each document:

* `reference`: Document unique reference as a string.
* `content`: Document content, as a string (of maxReadSize length).
* `metadata`: Document fields.
* `parsed`: Whether the document was already parsed, as a boolean.
* `sectionIndex`: Content section index if it had to be split, as an integer.

There is no expected return value from your script. Returning one has no effect.
