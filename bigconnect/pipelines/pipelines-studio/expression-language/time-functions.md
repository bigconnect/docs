# Time Functions

Use time functions to return the current time or to transform datetime data.

You can replace any datetime argument with an expression that evaluates to a datetime value. You cannot replace a datetime argument with a datetime literal.

You can replace any long or string argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following time functions:

### time:createDateFromStringTZ(\<string>, \<time zone>, \<date format>)

Creates a Date object based on a datetime in a String field and using the specified time zone. The datetime string should not include the time zone.

Uses the following arguments:

* string - String with datetime values, not including the time zone.
* time zone - Time zone associated with the datetime values. The time zone is used when creating the Date object.You can use the following time zone formats:
  * \<area>/\<location> - For a list of valid time zones in this format, see [https://www.vmware.com/support/developer/vc-sdk/visdk400pubs/ReferenceGuide/timezone.html](https://www.vmware.com/support/developer/vc-sdk/visdk400pubs/ReferenceGuide/timezone.html).
  * Numeric time zones with the GMT prefix, such as GMT-0500 or GMT-8:00. Note that numeric-only time zones such as -500 are not supported.
  * Short time zone IDs such as EST and CST - These time zones should generally be avoided because they can stand for multiple time zones, e.g. CST stands for both Central Standard Time and China Standard Time.
* date format - The date format used by the string data. For information about date formats, see [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html).

For example, the following expression converts string values in the Datetime field to a Date object using the time zone specified in the TZ field. It assumes the date is in the format, "03/30/2016 12:15:33 AM":

```
${time:createDateFromStringTZ(record:value('/Datetime'), 'record:value('/TZ')', 'MM/dd/yyyy hh:mm:ss a')}
```

### time:dateTimeToMilliseconds(\<Date object>)

Converts a Date object to an epoch or UNIX time in milliseconds.

For example, the following expression converts the current time to epoch or UNIX time in seconds, and then multiplies the value by 1000 to convert the value to milliseconds:

```
${time:dateTimeToMilliseconds(time:now())}
```

Return type: Long.

### time:dateTimeZoneOffset(\<Date object>, \<time zone>)

Returns the time zone offset in milliseconds for the specified date and time zone. The time zone offset is the difference in hours and minutes from Coordinated Universal Time (UTC).

Uses the following arguments:

* Date object - Date object to use.
* time zone - Time zone associated with the Date object.You can use the following time zone formats:
  * \<area>/\<location> - For a list of valid time zones in this format, see [https://www.vmware.com/support/developer/vc-sdk/visdk400pubs/ReferenceGuide/timezone.html](https://www.vmware.com/support/developer/vc-sdk/visdk400pubs/ReferenceGuide/timezone.html).
  * Numeric time zones with the GMT prefix, such as GMT-0500 or GMT-8:00. Note that numeric-only time zones such as -500 are not supported.
  * Short time zone IDs such as EST and CST - These time zones should generally be avoided because they can stand for multiple time zones, e.g. CST stands for both Central Standard Time and China Standard Time.

For example, the following expression returns the time zone offset of the Date object stored in the Date field using the time zone specified in the TZ field:

```
${time:dateTimeZoneOffset(record:value('/Date'), record:value('/TZ'))}
```

Return type: Long.

### time:extractDateFromString(\<string>, \<format string>)

Extracts a Date object from a String, based on the specified date format.

Uses the following arguments:

* string - String to extract the Date object from.
* format string - String that specifies the date format of the data in the \<string> argument. For information about creating a date format, see [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html).

For example, the following expression converts the string '2017-05-01 20:15:30.915' to a Date object:

```
${time:extractDateFromString('2017-05-01 20:15:30.915','yyyy-MM-dd HH:mm:ss.SSS')}
```

Return type: Date object.

### time:extractLongFromDate(\<Date object>, \<format string>)

Extracts a long value from a Date object, based on the specified date format.Uses the following arguments:

* Date object - Date object to extract the long value from.
* format string - String that specifies the format of the Date data to be extracted. For information about creating a date format, see [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html).

For example, the following expression returns the month as a long value from the Date object stored in the Timestamp field:

```
${time:extractLongFromDate(record:value('/Timestamp'), "MM")}
```

Return type: Long.

Note: Because the function returns a long value, you cannot specify non-numeric data in the data format string. For example, the date format "MMM" returns the three character abbreviation for the month (such as "Jul"), which causes the function to return incorrect results.

### time:extractStringFromDate(\<Date object>, \<format string>)

Extracts a string value from a Date object based on the specified date format.Uses the following arguments:

* Date object - Date object to extract the string value from.
* format string - String that specifies the format of the Date data to be extracted. For information about creating a date format, see [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html).

For example, the following expression returns the month as a string value from the Date object stored in the Timestamp field:

```
${time:extractStringFromDate(record:value('/Timestamp'), "MMM")}
```

Return type: String.

### time:extractStringFromDateTZ(\<Date object>, \<time zone>, \<format string>)

Extracts a string value from a Date object, converting the GMT time in the Date object to the specified date format and time zone. The function adjusts for daylight savings when given the time zone in the appropriate format.

Uses the following arguments:

* Date object - Date object to use.
*   time zone - Time zone to use in the conversion.

    To convert the time zone and adjust for daylight savings, use the \<Area>/\<Location> format, such as America/Mexico\_City. See the following link for a list of valid time zones in this format: [https://www.vmware.com/support/developer/vc-sdk/visdk400pubs/ReferenceGuide/timezone.html](https://www.vmware.com/support/developer/vc-sdk/visdk400pubs/ReferenceGuide/timezone.html).

    Short time zone IDs such as EST and CST return data, but note that these time zones do not adjust for daylight savings, and should be avoided because they can stand for multiple time zones, e.g. CST stands for both Central Standard Time and China Standard Time.

    You can use numeric time zones with the GMT prefix, such as GMT-0500 or GMT-8:00, but these time zones also do not account for daylight savings. Note that numeric-only time zones such as -500 are not supported.
* format string - String that specifies the format to use to express the date. For information about creating a date format, see [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html).

Return type: String.

Returns an empty string when the time zone is not specified or invalid.

For example, the following expression converts a Date object to a string date while adjusting to the America/New\_York time zone and daylight savings. It expresses the date in the format, "03/30/2016 12:15:33 AM":

```
${time:extractStringFromDateTZ(record:value('/datetime'), 'America/New_York', 'MM-dd-yyyy hh:mm:ss a')}
```

### time:millisecondsToDateTime(\<long>)

Converts an epoch or UNIX time in milliseconds to a Date object.If the epoch or UNIX time is in seconds, multiply the value by 1000 to produce a value in the milliseconds range. For example, the following expression multiplies the epoch time in seconds stored in the epochTime field and then converts the value to a Date object:

```
${millisecondsToDateTime(record:value("/epochTime")* 1000)}
```

Return type: Date object.

### time:now()

Returns the current time of the Data Pipelines machine as a java.util.Date object.For example, you might use the function in the Expression Evaluator to create a field with the time of processing.Return type: Datetime.

### time:timeZoneOffset(\<time zone>)

Returns the time zone offset in milliseconds for the specified time zone. The time zone offset is the difference in hours and minutes from Coordinated Universal Time (UTC).

Uses the following argument:

* time zone - Time zone to use.You can use the following time zone formats:
  * \<area>/\<location> - For a list of valid time zones in this format, see [https://www.vmware.com/support/developer/vc-sdk/visdk400pubs/ReferenceGuide/timezone.html](https://www.vmware.com/support/developer/vc-sdk/visdk400pubs/ReferenceGuide/timezone.html).
  * Numeric time zones with the GMT prefix, such as GMT-0500 or GMT-8:00. Note that numeric-only time zones such as -500 are not supported.
  * Short time zone IDs such as EST and CST - These time zones should generally be avoided because they can stand for multiple time zones, e.g. CST stands for both Central Standard Time and China Standard Time.

For example, the following expression returns the time zone offset using the time zone specified in the TZ field:

```
${time:timeZoneOffset(record:value('/TZ'))}
```

Return type: Long.

### time:trimDate(\<datetime>)

Trims the date portion of a datetime value by setting the date portion to January 1, 1970.

For example, if the current time of the Data Collector machine is Jul 25, 2016 5:18:05 PM, then ${time.trimDate(time:now())} returns: Jan 1, 1970 5:18:05 PM.

Return type: Datetime.

### time:trimTime(\<datetime>)

Trims the time portion of a datetime value by setting the time portion to 00:00:00.

For example, if the value of the pickup\_datetime field is Jul 22, 2016 11:16:10 AM, then ${time:trimTime(record:value("/pickup\_datetime"))} returns: Jul 22, 2016 12:00:00 AM.Return type: Datetime.
