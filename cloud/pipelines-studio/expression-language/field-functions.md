# Field Functions

You can use field functions in field path expressions that determine the set of fields that a processor uses. Each function is evaluated against a set of matching fields individually.

You can use the following field functions in Protector stages, such as the Protector: Expression processor:

* f:attribute()
* f:name()
* f:type()
* f:path()
* f:value()

For more information about field path expressions, see Field Path Expressions. For a list of stages where you can use field functions, see Supported Stages.

### f:attribute(\<attribute name>)

Returns the value of the specified field attribute.Uses the following argument:

* attribute name - A string that specifies a field attribute name.

Return type: String.

The following field path expression returns only the fields where the region attribute is set to `west`:

```
/*[${f:attribute('region') == 'west'}]
```

### f:getSiblingWithName(\<field name>)

Returns the sibling field - a field with the same parent field - that has the name matching `<field name>`, if the field exists.Uses the following argument:

* field name - A string that specifies a field name.

Return type: Field.

Note: The returned field object supports the `value()` and `type()` field functions.For example, the following expression returns the value from the sibling field named `SalePrice`:

```
${f:getSiblingWithName('SalePrice').value}
```

### f:hasSiblingWithName(\<field name>)

Returns `true` if there is a sibling field with a name matching `<field name>`, otherwise returns `false`.Uses the following argument:

* field name - A string that specifies a field name.

Return type: Boolean.

For example, the following conditional expression returns `true` if there is a sibling field named `SalePrice`:

```
${f:hasSiblingWithName('SalePrice')}
```

### f:hasSiblingWithValue(\<field name>, \<field value>)

Returns `true` if there is a sibling field with a name matching `<field name>` that has value matching `<field value>`, otherwise returns `false`.

Uses the following arguments:

* field name - A string that specifies a field name.
* field value - The value stored in the field. The field determines the data type.

Return type: Boolean.

For example, the following conditional expression returns `true` if there is a sibling field named `SalePrice` that has a Float value of 0.99:

```
${f:hasSiblingWithValue('SalePrice',0.99)}
```

### f:index()

Returns the index within the parent list field. Returns -1 if the field is not in a list.

Return type: Integer.

For example, the following conditional expression returns `true` for the first field - the field with an index of 0 - from a `RankedItem` list field:

```
${str:endsWith(f:parentPath(),"/RankedItem") && f:index() == 0}
```

### f:name()

Returns the field name.

Return type: String.

For example, you might use this function in a Protector: Expression processor to replace sensitive data such as social security numbers with the name of the field, e.g. `ssn`.

### f:parent()

Returns the parent field.

Return type: Field.

Note: The returned field object supports the `value()` and `type()` field functions.For example, the following conditional expression returns `true` if the parent field is a map field:

```
${f:parent().type =='MAP')}
```

### f:parentPath()

Returns the path of the parent field.

Return type: String.

For example, the following conditional expression returns `true` if the parent field path ends in `Item`:

```
${str:endsWith(f:parentPath(),'Item')}
```

### f:path()

Returns the path of a field. Use to return the fields with a specified path or where the path is as defined in the expression.

Return type: String.

For example, the following field path expression returns all datetime fields except for the audit\_date field:

```
/*[${f:type() == 'DATETIME' && f:path() != '/audit_date'}]
```

### f:type()

Returns the data type of a field. Use to return the fields with a specified type or where the field type is as defined in the expression.

Important: Use all caps when specifying a data type in an expression and enclose the string in quotation marks. For a list of valid data types, see Data Collector Data Types.

Return type: String.

For example, the following field path expression returns all boolean fields:

```
/*[${f:type() == 'BOOLEAN'}]
```

### f:value()

Returns the value of the field. Use to return the fields with a specified value or where the value is as defined in the expression.

Return type: Dependent on the field type.

For example, the following field path expression returns only the fields where the value is greater than 0.

```
/*[${f:value() > 0}]
```

