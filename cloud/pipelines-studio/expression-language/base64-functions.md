# Base64 Functions

Use Base64 functions to encode or decode data using Base64.

You can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following Base64 functions:

### base64:decodeBytes(\<string>)

Returns a decoded byte array from a Base64 encoded string.Return type: Byte Array.Uses the following argument:

* string - The Base64 encoded string to decode.

For example, ${base64:decodeBytes(record:value("/ID"))} decodes the Base64 encoded string in the ID field as a byte array value.

### base64:decodeString(\<string>, \<charset>)

Returns a decoded string from a Base64 encoded string using the specified character set.

Return type: String.

Uses the following arguments:

* string - The Base64 encoded string to decode.
* charset - The character set to use to decode the data.

For example, ${base64:decodeString(record:value("/ID"), "UTF-8")} decodes the Base64 encoded string in the ID field using the UTF-8 character set as a string value.

### base64:encodeBytes(\<byte array>, \<urlSafe: true | false>)

Returns a Base64 encoded string value of the specified byte array.

Return type: String.

Uses the following arguments:

* byte array - The byte array to encode using Base64.
* urlSafe - When set to true, encodes the data so that it can be safely sent in a URL.

For example, ${base64:encodeBytes(record:value("/ID"), true)} uses Base64 to encode the byte array in the ID field such that the encoded data is URL safe.

### base64:encodeString(\<string>, \<urlSafe: true | false>, \<charset>)

Returns a Base64 encoded string value of the specified string.

Return type: String.

Uses the following arguments:

* string - The string to encode using Base64.
* urlSafe - When set to true, encodes the data so that it can be safely sent in a URL.
* charset - The character set to use to encode the data.

For example, ${base64:encodeString(record:value("/ID"), false, "UTF-8")} uses Base64 to encode the string in the ID field using the UTF-8 character set such that the encoded data is not URL safe.
