# Filters

A Filter allows you to filter out certain incoming documents based on field values.

## DateMetadataFilter

Accepts or rejects a document based on the date value(s) of a document field, stored in a specified format. If multiple values are found for a field, only one of them needs to match for this filter to take effect. If the value cannot be parsed to a valid date, it is considered not to be matching.

![](<../../../.gitbook/assets/image (503).png>)

When adding a condition, you can specify a static date (i.e. a constant date value), or you can tell this filter you want to use a date relative to the current type. There is a distinction to be made between TODAY and NOW. TODAY is the current day without the hours, minutes, and seconds, where as NOW is the current day with the hours, minutes, and seconds. You can also decide whether you want the current date to be fixed (does not change after being set for the first time), or whether it should be refreshed on every call to reflect system date time changes.

Use one or two (for ranges) conditions where:        &#x20;

```
Possible operators are: 
    gt -> greater than
    ge -> greater equal
    lt -> lower than
    le -> lowe equal
    eq -> equals

Condition date value format are either one of:
    yyyy-MM-dd                  -> date (e.g. 2015-05-31) 
    
    yyyy-MM-ddThh:mm:ss[.SSS]   -> date and time with optional 
                                   milliseconds (e.g. 2015-05-31T22:44:15)
                                   
    TODAY[-+]9[YMDhms][*] -> the string "TODAY" (at 0:00:00) minus
                             or plus a number of years, months, days, 
                             hours, minutes, or seconds
                             (e.g. 1 week ago: TODAY-7d).
                             * means TODAY can change from one invocation
                               to another to adjust to a change of current day
                               
    NOW[-+]9[YMDhms][*]  -> the string "NOW" (at current time) minus
                            or plus a number of years, months, days,
                            hours, minutes, or seconds
                            (e.g. 1 week ago: NOW-7d).
                            * means NOW changes from one invocation
                              to another to adjust to the current time.
```

* **field** - sourcde field
* **format** - to successfully parse a date, an optional date format can be specified, as per the formatting options found on [SimpleDateFormat](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/SimpleDateFormat.html). The default format when not specified is EPOCH (the difference, measured in milliseconds, between the date and midnight, January 1, 1970).
* **onMatch** - whether to include or exclude the document when this filter matches all conditions
* **condition1** - the first condition
* **condition2** - (optional) second condition for a date range

For example, let's say you want to keep only documents from the last seven days, not including today. The following would achieve that:

* _**condition1**: TODAY-7_
* _**condition2**: TODAY_

## EmptyMetadataFilter

Accepts or rejects a document based on whether any of the specified document fields are empty or not. Any control characters (char <= 32) are removed before evaluating if a property is empty or not.

![](<../../../.gitbook/assets/image (413).png>)

* **onMatch** - whether to include or exclude the document when this filter matches all conditions
* **fields** - coma separated list of fields to match

## NumericMetadataFilter

Accepts or rejects a document based on the numeric value(s) of a field, supporting decimals. If multiple values are found for a field, only one of them needs to match for this filter to take effect. If the value is not a valid number, it is considered not to be matching. The decimal character is expected to be a dot. To reject decimals or to deal with non-numeric fields in your own way, you can use **RegexMetadataFilter**.

![](<../../../.gitbook/assets/image (622).png>)

Use one or two (for ranges) conditions, where possible operators are:&#x20;

```
gt -> greater than
ge -> greater equal
lt -> lower than
le -> lowe equal
eq -> equals
```

* **field** - source field
* **onMatch** - whether to include or exclude the document when this filter matches all conditions
* **condition1** - the first condition
* **condition2** - (optional) second condition for a numeric range

For example, let's say you are importing customer profile documents and you have a field called "age" and you need to only consider documents for customers in their twenties (greater or equal to 20, but lower than 30). The following would achieve that:

* _**condition1**: ge 20_
* _**condition2**: lt 30_

## RegexMetadataFilter

Accepts or rejects a document based on its field values using [regular expression](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html).&#x20;

{% hint style="info" %}
You can test your Regex patterns using this [tool](https://www.freeformatter.com/java-regex-tester.html).
{% endhint %}

![](<../../../.gitbook/assets/image (580).png>)

* **onMatch** - whether to include or exclude the document when this filter matches all conditions
* **field** - source field
* **caseSensitive** - perform the matching case-sensitive
* **regex** -  [regular expression](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html)&#x20;

## RegexReferenceFilter

Accepts or rejects a document based on its reference (e.g. URL).

{% hint style="info" %}
You can test your Regex patterns using this [tool](https://www.freeformatter.com/java-regex-tester.html).
{% endhint %}

![](<../../../.gitbook/assets/image (586).png>)

* **onMatch** - whether to include or exclude the document when this filter matches all conditions
* **caseSensitive** - perform the matching case-sensitive
* **regex** - [regular expression](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html)

## ScriptFilter

Filter incoming documents using a scripting language. The default script engine is JavaScript. The default script engine is JavaScript, version [ECMAScript Edition 5.1](http://www.ecma-international.org/ecma-262/5.1/).

The following are variables made available to your script for each document:

* `reference`**:** Document unique reference as a string.
* `content`**:** Document content, as a string (of `maxReadSize` length).
* `metadata`**:** Document fields
* `parsed`**:** Whether the document was already parsed, as a boolean.
* `sectionIndex`**:** Content section index if it had to be split, as an integer.

The expected **return value** from your script is a boolean indicating whether the document was matched or not.
