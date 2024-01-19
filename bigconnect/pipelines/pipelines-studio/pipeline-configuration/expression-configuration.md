# Expression Configuration

Use the expression language to configure expressions and conditions in processors, such as the Expression Evaluator or Stream Selector. Some destination properties also allow the expression language, such as the directory template for the Hadoop FS destination.

You can use the expression language to define any stage or pipeline property that represents a numeric or string value. You can also use field path expressions to select the fields to use in some processors.

Use expression completion to determine where you can use an expression and the expression elements that you can use in that location.

You can use the following elements in an expression:

* Constants
* Datetime variables
* Field names
* Functions
* Literals
* Operators
* Runtime parameters
* Runtime properties
* Runtime resources

## Basic Syntax <a href="#ariaid-title2" id="ariaid-title2"></a>

Precede all expressions with a dollar sign and enclose them with curly brackets, as follows: `${<expression>}`.

For example, to add 2 + 2, use the following syntax: ${2 + 2}.

## Using Field Names in Expressions

When a pipeline is valid for preview, expression completion provides available field names in a list. When a list is not available, use the appropriate format for the field name.

When you use a field name in an expression, use the following syntax:

```
${record:value("/<field name>")}
```

Note: You can use single or double quotation marks to surround a field name.

For example, the following expressions both concatenate the values from the DATE field with values from the TIME field:

```
${record:value('/DATE')} ${record:value('/TIME')}
${record:value("/DATE")} ${record:value("/TIME")}
```

### Field Names with Special Characters <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use quotation marks and the backslash character to handle special characters in field names.

Expression completion provides the correct syntax for field names with special characters. But when you need to enter the field names manually, be sure to use the following guidelines:Use quotation marks around field names with special charactersWhen a field name includes special characters, surround the field name with single or double quotation marks as follows:

```
/"<field w/specialcharacter>"
```

Some examples:

```
/"Stream$ets"
/'city&state'
/"product names"
```

When using multiple sets of quotation marks, alternate between types as you goThroughout the expression language, when using quotation marks, you can use single or double quotation marks. But make sure to alternate between the types when nesting quotation marks.For example:

```
${record:value('/"Stream$ets"'}
${record:value("/'city&state'"}
```

Use a backslash as an escape characterTo use a quotation mark or backslash in a field name, use a backslash ( \ ).Add additional backslashes as necessary to escape quotation marks.For example, to use a field named "ID's" as a required field, you would use a single backslash:

```
/ID\'s
```

To use the same field in an expression, you might need additional backslashes as follows:

```
${record:value('/ID\\'s')}
```

## Referencing Field Names and Field Paths <a href="#ariaid-title5" id="ariaid-title5"></a>

When a pipeline is valid for preview, you can generally select fields from a list. When a list is not available or when you are defining a new field name, you need to use the appropriate format for the field name.

To reference a field, you specify the path of the field. A field path describes a data element in a record using a syntax similar to files in directories. The complexity of a field path differs based on the type of data in the record:

Simple maps or JSON objectsWith simple maps or JSON objects, the fields are one level removed from the root. Reference the field as follows:

```
/<field name>
```

So, to reference a CITY field in a simple JSON object, enter `/CITY`. A simple expression that calls the field might look like this:

```
${record:value('/CITY')}
```

Complex maps or JSON objectsTo reference a field in a complex map or JSON object, include the path to the field, as follows:

```
/<path to field>/<field name>
```

For example, the following field path describes an employeeName field several levels deep in a JSON object: `/region/division/group/employeeName`. An expression that calls the field might look like this:

```
${record:value("/region/division/group/employeeName")}
```

Arrays or listsTo reference a field in an array or list, include the index and path to the field, as follows:

```
[<index value>]/<path to field>/<field name>
```

For example, the following field path describes the same employeeName field in the third region index in an array: `[2]/east/HR/employeeName`.An expression that calls the field might look like this:

```
${record:value('[2]/east/HR/employeeName')}
```

Delimited records can be structured as lists. For more information, see [Delimited Data Root Field Type](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Data\_Formats/Delimited.html#concept\_zcg\_bm4\_fs).TextTo reference text when a record is a line of text, use the following field name:

```
/text
```

### Wildcard Use for Arrays and Maps

In some processors, you can use the asterisk wildcard (\*) as indices in an array or key values in a map. Use a wildcard to help define the field paths for maps and arrays.

You can use the asterisk wildcard as follows:\[\*]Matches all values for the specified index in an array. For example, the following field path represents the social security number of every employee in every division:

```
/Division[*]/Employee[*]/SSN
```

/\*Matches all values for the specified keys in a map. For example, the following field path represents all employee information in the first division:

```
/Division[0]/Employee[*]/*
```

## Field Path Expressions <a href="#ariaid-title7" id="ariaid-title7"></a>

You can use field path expressions in certain processors to determine the set of fields that you want the processor to use.

For example, you want to use the Field Remover processor to remove all fields that start with the same prefix. Instead of manually entering each field name, you can use a field path expression to specify the fields to remove.

### Supported Stages <a href="#ariaid-title8" id="ariaid-title8"></a>

You can use field path expressions to specify the fields to use in the following processors:

* Field Hasher processor
* Field Masker processor
* Field Remover processor
* Field Replacer processor
* Field Type Converter processor
* TensorFlow Evaluator processor
* Value Replacer processor (deprecated)

### Field Path Expression Syntax <a href="#ariaid-title9" id="ariaid-title9"></a>

When creating a field path expression, you can use a combination of standard expression language syntax with field path expression syntax. You can use the following components in field path expressions:Root field and relative pathsAs with specifying any field path, begin a field path expression with a slash ( / ) to indicate the location of the fields in relation to the root field. Then, continue defining the field path as appropriate.For example, the following field path expression uses a wildcard to specify all fields in the record:

```
/*
```

Wildcard charactersYou can use the asterisk character ( \* ) and question mark character ( ? ) as wildcards, as follows:

*   Use the asterisk wildcard to represent one or more characters.For example, to perform an action on all fields in a Stores map field, you can use the following field path expression:

    ```
    /Stores/*
    ```
*   Use the question mark wildcard to represent exactly one character.For example, the following expression includes all fields that have a two-character prefix followed by an underscore:

    ```
    /??_*
    ```

Brackets for position predicatesYou can specify a field based on its position in a list field. After the name of the list field, specify the position surrounded by brackets ( \[ ] ). Note that position numbering starts with 0.For example, the following expression calls the fourth item in a colors list field:

```
/colors[3]
```

Brackets for complex expressionsYou can configure field path expressions that use functions, typically field functions, to define a specific subset of fields to return. When configuring complex expressions, surround the expression with brackets ( \[ ] ), as follows:

```
/*[${<complex expression>}]
```

For example, the following expression returns all fields with an "info" field attribute set to any value:

```
/*[${f:attribute('info') == '*'}]
```

Field functionsUse field functions to determine the fields to use based on field-related information, such as `f:type` for the data type of the field, `f:value` for the value of the field, or `f:attribute` for an attribute or attribute value of the field.For example, you can use the Field Type Converter processor to convert all Integer fields with the following expression:

```
/*[${f:type() == 'INTEGER'}]
```

For more information about field functions, see [Field Functions](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Expression\_Language/Functions.html#concept\_gfs\_w55\_3cb).Other functionsYou can use other functions, such as record, string, or time functions, as part of complex field path expressions.For example, the following expression defines the subset of fields where the region attribute is set to the results of the storeId field:

```
/*[${f:attribute('region') == record:value('/storeId')}]
```

## Data Type Coercion <a href="#ariaid-title10" id="ariaid-title10"></a>

When an expression requires, the expression language attempts implicit data type conversion - called data type coercion. When coercion is not possible, BigConnect Pipelines passes the error records to the stage for error handling.

For example, you have an Expression Evaluator stage configured to send error records to the pipeline for error handling, and the pipeline writes error records to a file. The Expression Evaluator includes an expression that treats string data as integers. When the field includes integer or valid numeric data, the expression language coerces the data type. If the field includes a date, that record is written to the error records file.

To avoid coercion errors, you can use the Field Type Converter earlier in the pipeline to convert data to the appropriate data type.
