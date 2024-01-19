# Data Drift Functions

Use data drift functions to create alerts when data drift occurs. You can use these functions in data drift rules.

You can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following data drift functions:

### drift:names(\<field path>, \<ignoreWhenMissing: true | false>)

Triggers an alert when key values change in a map or list-map field. When key values are field names, triggers an alert when field names change. Use with list-map or map fields.

Use the ignoreWhenMissing flag to determine the behavior when the field is missing. When set to "true", a missing field causes no errors. When set to "false", a missing field generates an alert for the record missing the field, and for the next record that includes the field.

For example, to trigger an alert when any field name in the record changes and ignore empty records, you can use the root field in the expression as follows:

```
${drift:names('/', true)}
```

### drift:order(\<field path>, \<ignoreWhenMissing: true | false>)

Triggers an alert when the field order changes for fields within a list-map field. Use only with list-map fields.Note that this function triggers alerts when field names change as well.

Use the ignoreWhenMissing flag to determine the behavior when the field is missing. When set to "true", a missing field causes no errors. When set to "false", a missing field generates an alert for the record missing the field, and for the next record that includes the field.

To alert on field order or name changes within a delimited record and ignore empty records, use the root field as follows:

```
${drift:order('/', true)}
```

### drift:size(\<field path>, \<ignoreWhenMissing: true | false>)

Triggers an alert when the number of fields changes within a list, list-map, or map field. Use with list, list-map, or map fields.

Use the ignoreWhenMissing flag to determine the behavior when the field is missing. When set to "true", a missing field causes no errors. When set to "false", a missing field generates an alert for the record missing the field, and for the next record that includes the field.

To alert when the number of columns change within the Depts map field and when a record is missing the Depts field, use the following expression:

```
${drift:size('/Depts', false)}
```

### drift:type(\<field path>, \<ignoreWhenMissing: true | false>)

Triggers an alert when the data type of the specified field changes. Evaluates only the specified field.

Use the ignoreWhenMissing flag to determine the behavior when the field is missing. When set to "true", a missing field causes no errors. When set to "false", a missing field generates an alert for the record missing the field, and for the next record that includes the field.

For example, to receive alerts when the data type of the ID field changes and when the ID field does not exist in the record, use the following expression:

```
${drift:type('/ID', false)}
```
