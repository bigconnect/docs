# Delimited Data Record Functions

Use delimited data record functions to process delimited data with the list root field type. If you configured an origin to process the delimited data with the list-map root field type, you can use standard record functions.

For more information about the delimited data root field types, see Delimited Data Root Field Type.

You can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following delimited data record functions:

### **record:dExists()**&#x20;

Determines if the specified header name exists in the record.&#x20;

Return type: Boolean.

### **record:dHasDupHeader()**

Determines if the record has duplicate header names.&#x20;

Return type: Boolean.

### **record:dIndex(\<header name>)**&#x20;

Returns the index associated with the specified header name. Use to find the position of a header in the record.&#x20;

Return type: Integer, starting with 0 for the first position.

### **record:dIsDupHeader(\<header name>)**&#x20;

Determines if the specified header name is used more than once in a record.&#x20;

Return type: Boolean.

### **record:dToMap()**

Converts a record with a List root field to a List-Map root field. Use to convert delimited List records to List-Map to allow easier use with standard record functions. For more information about root field types, see Delimited Data Root Field Type.

Return type: Records with a List-Map root field.

To use record:dToMap(), use a slash as the root output field as follows:

<figure><img src="../../../../.gitbook/assets/image (1064).png" alt=""><figcaption></figcaption></figure>

### **record:dValue(\<header name>)**&#x20;

Returns the value associated with the specified header name.

Return type: String.

### **record:dValueAt(\<header index>)**&#x20;

Returns the value at the specified header index position.

Return type: String.
