# Functions

### Record Functions <a href="#ariaid-title2" id="ariaid-title2"></a>

Use record functions to determine information about a record, such as the stage that created it or whether a field exists in the record.

You can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following general record functions:

**record:attribute(\<attribute name>)**

Returns the value of the specified record attribute. Use to return data, for example, for the "tag" attribute that you set using the File Tail origin.Uses the following argument:

* attribute name - Name of the record header attribute.

Return type: String.

For example, the following expression returns the value of a tag attribute added by a File Tail origin:

```
${record:attribute('tag')}
```

**record:attributeOrDefault(\<attribute name> , \<default value>)**

Returns the value of the specified record header attribute. When the attribute does not exist or has no value, returns the specified default value.Uses the following arguments:

* attribute name - Name of the record header attribute.
* default value - Value to use when the record header attribute does not exist or has no value.

Return type: Returns a string value when returning the field attribute value. Otherwise, returns the type of the specified default value.

For example, the following expression returns the value of the tag attribute, and returns NA when no value exists:

```
${record:attributeOrDefault('tag', 'NA')}
```

**record:creator()**

Returns the instance name of the stage that created the record, typically an origin stage. Data is stored in the stageCreator field of a record.

Return type: String.

For example, the following expression returns the instance name of the stage that creates each record:

```
${record:creator()}
```

**record:eventCreation()**

Returns the epoch timestamp when an event occurred. Use to return the creation timestamp from an event record. Returns null values when applied to other records.

Return type: String.

For example, the following expression returns the time that the event-generating stage generated an event:

```
${record:eventCreation()}
```

**record:eventType()**

Returns the event type from an event record. Returns null values when applied to other records.Event generating stages create different event types. For information about the possible event types, see "Event Records" in the stage documentation.

Return type: String.

For example, the following expression returns the event type from an event record:

```
${record:eventType()}
```

**record:eventVersion()**

Returns the event version from an event record. Returns null values when applied to other records.

Return type: String.

For example, the following expression returns the event version from an event record:

```
${record:eventVersion()}
```

**record:exists(\<field path>)**

Determines if the specified field is in a record. When the field is not in the record, returns 0. When the field is in the record, returns 1.

Return type: Boolean.

For example, the following expression returns 1 when the Name field exists in a record:

```
${record:exists('/Name')}
```

It returns 0 when the Name field is not part of the record.

**record:fieldAttribute(\<field path>, \<field attribute name>)**

Returns the value for the specified field attribute.Uses the following arguments:

* field path - Path to the field with the attribute.
* field attribute name - Name of the field attribute.

Return type: String

For example, the following expression returns the value for the pk attribute on the ID field:

```
record:fieldAttribute('/ID', 'pk')
```

**record:fieldAttributeOrDefault(\<field path>, \<field attribute name>, \<default value>)**

Returns the value for the field attribute when it exists. If the attribute does not exist, returns the specified default value.Uses the following arguments:

* field path - Path to the field with the attribute.
* field attribute name - Name of the field attribute.
* default value - Value to use when the field attribute does not exist or has no value.

Return type: Returns a string value when returning the field attribute value. Otherwise, returns the type of the specified default value.

For example, the following expression returns the value for the pk attribute on the ID field when available, otherwise, it returns "n".

```
record:fieldAttributeOrDefault('/ID', 'pk', 'n')
```

**record:id()**

Returns the record ID. The record ID is also stored in the sourceId record header attribute.

Return type: String.

**record:path()**

Returns the stages in the pipeline that processed a record. Data is stored in the stagesPath field of a record. The field path uses the following format:

```
<origin instance name>:<next stage instance name>:<next stage instance name>:... 
```

Return type: String.

**record:type(\<field path>)**

Checks the data type of a field. Possible return values are:

* Boolean
* Char
* Byte
* Short
* Integer
* Long
* Float
* Double
* Date
* Datetime
* Decimal
* String
* Time
* Byte-Array
* Map
* List

Return type: String.

**record:value(\<field path>)**

Returns the value of the field in the appropriate data type.

Return type: When the field includes a value, returns the type based on the field value. When the field is null, the function returns no value.

Nulls are coerced based on the expected return type of the function. For example, if an EL is expected to return a string, null is coerced to an empty string. However, a null integer is coerced to 0.

**record:valueOrDefault(\<field path> , \<default value>)**

Returns the value of the field in the appropriate data type. If the field does not exist or if the field is null, returns the default value.Use to provide a default value when a field does not exist or when a field is null.

Return type: Variable, based on whether the function returns the field value or default value.

### Delimited Data Record Functions <a href="#ariaid-title3" id="ariaid-title3"></a>

Use delimited data record functions to process delimited data with the list root field type. If you configured an origin to process the delimited data with the list-map root field type, you can use standard record functions.

For more information about the delimited data root field types, see Delimited Data Root Field Type.

You can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following delimited data record functions:

**record:dExists()**&#x20;

Determines if the specified header name exists in the record.&#x20;

Return type: Boolean.

**record:dHasDupHeader()**

Determines if the record has duplicate header names.&#x20;

Return type: Boolean.

**record:dIndex(\<header name>)**&#x20;

Returns the index associated with the specified header name. Use to find the position of a header in the record.&#x20;

Return type: Integer, starting with 0 for the first position.

**record:dIsDupHeader(\<header name>)**&#x20;

Determines if the specified header name is used more than once in a record.&#x20;

Return type: Boolean.

**record:dToMap()**

Converts a record with a List root field to a List-Map root field. Use to convert delimited List records to List-Map to allow easier use with standard record functions. For more information about root field types, see Delimited Data Root Field Type.

Return type: Records with a List-Map root field.

To use record:dToMap(), use a slash as the root output field as follows:

<figure><img src="../../../.gitbook/assets/image (1064).png" alt=""><figcaption></figcaption></figure>

**record:dValue(\<header name>)**&#x20;

Returns the value associated with the specified header name.

Return type: String.

**record:dValueAt(\<header index>)**&#x20;

Returns the value at the specified header index position.

Return type: String.

