# Field Type Converter

The Field Type Converter processor converts the data types of fields to compatible data types. You might use the processor to convert the data types of fields before performing calculations. You can also use the processor to change the scale of decimal data.

You configure the processor to use one of the following methods to convert data types:

By Field Name

Convert the data type of a field with the specified name. For example, you can convert a field named `dropoff_datetime` with the String data type to the Date data type.

You can specify individual field names or use field path expressions to specify sets of fields to convert.

By Data Type

Convert the data type of all fields with the specified type. For example, you can convert all fields with the Decimal data type to the String data type.

You can convert data types by field name or by data type. You cannot use both methods in the same stage.

Configure the conversion for an appropriate, compatible data type. Consider the actual data in the field as well, since even valid conversions can truncate data. For example, converting a field from Integer to Decimal is valid. Converting a field from Decimal to Integer is also valid, but the conversion can truncate any decimal values in the data.

Tip: You can use data preview to verify the data in the fields.When you convert string data to a Date, Datetime, or Time data type or when you convert date, datetime, or time data to the String data type, you specify the date format that you want to use. You can use any valid format.

### Valid Type Conversions <a href="#ariaid-title2" id="ariaid-title2"></a>

The following table lists the data types that can be converted to another data type. List, Map, and List-Map data types cannot be converted.

| Target Data Type | Source Data Type                                                               |
| ---------------- | ------------------------------------------------------------------------------ |
| Boolean          | Byte, Decimal, Double, Float, Integer, Long, Short, String                     |
| Byte             | Decimal, Double, Float, Integer, Long, Short, String                           |
| Byte Array       | String                                                                         |
| Character        | String                                                                         |
| Date             | Datetime, Long, String, Time                                                   |
| Datetime         | Date, Long, String                                                             |
| Decimal          | Byte, Double, Float, Integer, Long, Short, String                              |
| Double           | Byte, Decimal, Integer, Float, Long, Short, String                             |
| Float            | Byte, Decimal, Double, Integer, Long, Short, String                            |
| Integer          | Boolean1, Byte, Decimal, Double, Float, Long, Short, String                    |
| Long             | Boolean1, Byte, Date, Datetime, Decimal, Double, Float, Integer, Short, String |
| Short            | Boolean1, Byte, Decimal, Double, Float, Integer, Long, String                  |
| String           | All supported data types except List, Map, and List-Map                        |
| Time             | Date, Datetime, String, Long                                                   |
| Zoned Datetime   | Date, Datetime, String                                                         |

1When converting from a Boolean data type, the processor converts TRUE to 1 and FALSE to 0.

### Expressions to Convert by Field Name <a href="#ariaid-title3" id="ariaid-title3"></a>

When converting data types by field name, you can specify field path expressions to specify sets of fields to convert.

For example, let's say that a pipeline uses the JDBC Query Consumer origin to read data from an Oracle table with the following columns:

| Column Name                                                                                                                                | Data Type                                                                                                                        | Precision                                                                                                     | Scale                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| <p><code>order_id</code></p><p><code>item</code></p><p><code>quantity</code></p><p><code>cost_per_item</code></p><p><code>total</code></p> | <p><code>Number</code></p><p><code>Char</code></p><p><code>Number</code></p><p><code>Number</code></p><p><code>Number</code></p> | <p><code>8</code></p><p><code>n/a</code></p><p><code>3</code></p><p><code>10</code></p><p><code>20</code></p> | <p><code>0</code></p><p><code>n/a</code></p><p><code>0</code></p><p><code>2</code></p><p><code>0</code></p> |

The JDBC Query Consumer origin converts all Oracle columns with the Number data type to the Decimal data type, storing the precision and scale for the column in field attributes. You'd like to use the Field Type Converter processor to convert each field with the Decimal data type to the Integer, Long, or Float data type, based on the precision and scale of the field.

Instead of specifying each field name to convert the field to the appropriate data type, you can use field path expressions to specify sets of fields to convert.

For example, the following field path expression returns all decimal fields with a scale of 0 and a precision less than 10, which you can then convert to the Integer data type:

`/*[${f:attribute('scale') == 0 && f:attribute('precision') < 10 }]`

The following field path expression returns all decimal fields with a scale of 0 and a precision greater than or equal to 10, which you can then convert to the Long data type:

`/*[${f:attribute('scale') == 0 && f:attribute('precision') >= 10 }]`

And the following field path expression returns all decimal fields with a scale greater than 0, which you can then convert to the Float data type:

`/*[${f:attribute('scale') > 0}]`

You specify these field path expressions when you configure the Field Type Converter processor, as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldTypeConverter-FieldPathExp.png)

### Changing the Scale of Decimal Fields <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use the Field Type Converter processor to change the scale of decimal fields. For example, you might have a decimal field with the value 12345.6789115, and you'd like to decrease the scale to 4 so that the value is 12345.6789.

To change the scale, you configure the processor to convert decimal fields to the Decimal data type, specifying the scale to use. When you decrease the scale, you can also specify the rounding strategy. For example, you can configure the processor to round up or round down.

You can change the scale of a decimal field by name. Or you can change the scale for all fields with the Decimal data type.

### Configuring a Field Type Converter <a href="#ariaid-title5" id="ariaid-title5"></a>

Configure a Field Type Converter processor to convert the data types of fields.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Conversions tab, configure the following properties:

    | Field Type Converter Property | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Conversion Method             | Specifies whether to convert data types by field name or by data type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Fields to Convert             | <p>One or more fields to convert to the same data type. Used when converting by field name only.</p><p>You can use the asterisk wildcard to represent array indices and map elements.</p><p>You can specify individual field names or use field path expressions to specify sets of fields.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Source Type                   | Data type of the fields that you want to convert. Used when converting by data type only.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Convert to Type               | Data type for conversion. Select a valid type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Data Locale                   | <p>Locale of field data.</p><p>Can determine how the processor formats converted data, such as using a comma or period as a decimal separator.</p><p>Available for types affected by locale.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Treat Input Field as Date     | <p>Treats the input field as a datetime when converting a long field to the String data type.</p><p>Select when you want to convert a timestamp in a long field, such as an epoch or UNIX time, to a string such as "2017-02-01 12:00:00". The processor first converts the long value to a datetime, and then to a string using the specified date format.</p><p>When cleared, the processor converts a long value such as 1485979200 to the string value "1485979200".</p>                                                                                                                                                                                                                                                     |
    | Scale                         | <p>Scale to use when converting to the Decimal data type.</p><p>Enter a zero or positive number to indicate the number of digits to the right of the decimal point. If you enter a negative number, the processor multiplies the unscaled value of the number by ten to the power of the negation of the scale.</p><p>For more information about specifying the scale, see <a href="https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html">https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html</a>.</p>                                                                                                                                                                                            |
    | Rounding Strategy             | <p>Rounding strategy to use during the Decimal scale conversion.</p><p>For a description of each rounding strategy, see <a href="https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html">https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html</a>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Date Format                   | <p>Format of the date, datetime, or time data to be converted. Use to convert datetime data without time zone or UTC offset details to Date, Datetime, or Time. Or to convert date, datetime, or time data to String.</p><p>Select the format to use or create a custom format.</p><p>To convert datetime data with the time zone or offset information, use the Zoned Datetime Format property.</p><p>Note: Data preview displays date, datetime, and time data using the default format of the browser locale. For example, if the browser uses the en_US locale, preview displays dates using the following format: MMM d, y h:mm:ss a.</p>                                                                                   |
    | Other Date Format             | <p>Use to enter a custom date format.</p><p>For more information about creating a custom date format, see the <a href="https://docs.oracle.com/javase/tutorial/i18n/format/simpleDateFormat.html">Oracle Java documentation</a>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Zoned Datetime Format         | <p>Format for the date, datetime, or time data to be converted. Use to convert datetime data with time zone or offset information to the Zoned Datetime format or to convert Zoned Datetime data to String.Select one of the following options:</p><ul><li>yyyy-MM-dd'T'HH:mm:ssX - Use for datetime values with a UTC offset.</li><li>yyyy-MM-dd'T'HH:mm:ssX[VV] - Use for datetime values with a UTC offset and time zone. If the datetime value does not include a UTC offset, the stage uses the minimum offset for the specified time stamp.</li><li>Other - Use to enter a different zone datetime format.</li></ul><p>To convert datetime data without time zone or offset information, use the Date Format property.</p> |
    | Other Zoned Datetime Format   | <p>Use to enter a custom zoned datetime format.</p><p>For more information about creating a custom zoned datetime format, see the <a href="https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html">Oracle Java documentation</a>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Charset                       | <p>Character encoding of the data to be converted.</p><p>Available for types affected by encoding.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
3. To configure additional type conversions, click the Add icon and then repeat the previous step. You can use simple or bulk edit mode to configure additional conversions.You can configure additional conversions by field name or additional conversions by data type. You cannot use both methods in the same stage.
