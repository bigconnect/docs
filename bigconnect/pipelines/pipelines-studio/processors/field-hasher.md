# Field Hasher

The Field Hasher processor uses an algorithm to encode data. Use the processor to encode highly-sensitive data. For example, you might use the Field Hasher processor to encode social security or credit card numbers.

Field Hasher provides several methods to enable hashing individual fields or the entire record. You can hash any field that can be converted to a string. The resulting hash is a string value.

You can configure the Field Hasher processor to use MD5, SHA1, SHA-256, SHA-512, or MurmurHash3 128 to hash field values. You can optionally add a single field separator character to fields before hashing.

### Hash Methods <a href="#ariaid-title2" id="ariaid-title2"></a>

Field Hasher provides several methods to hash data. When you hash a field more than once, Field Hasher uses the existing hash when generating the next hash.

Field Hasher hashes in the following order. When using multiple hash methods, note that the order can affect how data is hashed:

1.  Hash in Place - Field Hasher replaces the original data in a field with hashed values.

    You can specify multiple fields to be hashed with the same algorithm. You can also use different algorithms to hash different sets of fields.
2.  Hash to Target - Field Hasher hashes data in a field and writes it to the specified field, header attribute, or both. It leaves the original data in place.

    If the specified target field or attribute does not exist, Field Hasher creates it.

    If you specify multiple fields to be hashed with the same algorithm, Field Hasher hashes the fields together.

    If any of the fields are already hashed, Field Hasher uses existing hash values to generate the new hash value.
3.  Hash Record - Field Hasher hashes the record and writes it to the specified field, header attribute, or both. You can include the record header in the hash.

    If the specified target field or attribute does not exist, Field Hasher creates it.

    If the record includes fields that are already hashed, Field Hasher uses the hash values when hashing the record.

### Field Separator <a href="#ariaid-title3" id="ariaid-title3"></a>

You can configure the Field Hasher processor to add a field separator character to the end of all fields to be hashed. You might want to add a field separator character when you hash multiple fields to a single field or when you hash an entire record.

When you use a field separator, the Field Hasher processor adds the character to the end of each field to be hashed before they are hashed, so the field separator character is hashed with the field. Note that since the field separator is added to each field, then the last field in a set of fields or the last field in a record also includes the field separator character in the hash.

When you enable the use of a field separator, you can select one of the character options - Tab, Semicolon, Comma, and Space - or you can select Other and enter the code for any UTF-8 character.

### List, Map, and List-Map Fields <a href="#ariaid-title4" id="ariaid-title4"></a>

Field Hasher does not hash list, map, or list-map fields, but can hash field data within the list, map, and list-map fields. To hash data within a list, map, or list-map field, select the field that contains the actual data to be hashed.

When hashing the entire record, Field Hasher hashes the data within list, map, and list-map fields.

### Configuring a Field Hasher <a href="#ariaid-title5" id="ariaid-title5"></a>

Configure a Field Hasher to encode sensitive data. You can hash the entire record or specific fields. You can also hash fields together to a target field or attribute header.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  To hash a field, click the Hash Field tab and optionally configure a field separator character:

    | Hash Field Property | Description                                                                                                                                                                                                                                                                                                                                                |
    | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Field Separator     | <p>A single character to use as a field separator. The configured field separator is added to the end of all fields before they are hashed. Select one of the following options:</p><ul><li>Tab</li><li>Semicolon</li><li>Comma</li><li>Space</li><li>Other</li></ul><p>When selecting other, enter the character code for the UTF-8 character to use.</p> |
3.  To hash fields in place, configure the following Hash in Place properties for each hash type that you want to use. Click Add to use additional hash types.

    | Hash in Place Property | Description                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Fields to Hash         | <p>One or more fields to hash with the same hash type.</p><p>You can specify individual fields or use a field path expression to specify a set of fields.</p>                                                                                                                                                                                                                                                                 |
    | Hash Type              | <p>Algorithm to use to hash field values:</p><ul><li>MD5 - Produces a 128-bit (16-byte) hash value, typically expressed in text format as a 32 digit hexadecimal number.</li><li>SHA1 - Produces a 160-bit (20-byte) hash value.</li><li>SHA256 - Produces a 256-bit (32-byte) hash value.</li><li>SHA512 - Produces a 512 bit (64-byte) hash value.</li><li>MURMUR3_128 - Produces a 128-bit (16 byte) hash value.</li></ul> |
4.  To hash one or more fields together and write them to a field or attribute header, configure the following Hash to Target properties. Click Add to hash additional fields.

    | Hash to Target Property | Description                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Fields to Hash          | <p>One or more fields to hash and write to a target field or header attribute.</p><p>If you enter more than one field, the processor hashes them together.</p><p>You can specify individual fields or use a field path expression to specify a set of fields.</p>                                                                                                                                                             |
    | Hash Type               | <p>Algorithm to use to hash field values:</p><ul><li>MD5 - Produces a 128-bit (16-byte) hash value, typically expressed in text format as a 32 digit hexadecimal number.</li><li>SHA1 - Produces a 160-bit (20-byte) hash value.</li><li>SHA256 - Produces a 256-bit (32-byte) hash value.</li><li>SHA512 - Produces a 512 bit (64-byte) hash value.</li><li>MURMUR3_128 - Produces a 128-bit (16 byte) hash value.</li></ul> |
    | Target Field            | Field in the record to use for hashed data. If the field does not exist, Field Hasher creates the field.                                                                                                                                                                                                                                                                                                                      |
    | Header Attribute        | Attribute in the record header to use for hashed data. If the attribute does not exist, Field Hasher creates the attribute.                                                                                                                                                                                                                                                                                                   |
5.  To configure field-level error handling, configure the following property on the Hash Field tab:

    | Field Error Handling Property | Description                                                                                                                                                                                                                                                                                                                                                 |
    | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | On Field Issue                | <p>Determines the action to take if a specified field to hash is missing from the record, contains a null value, or is a List, Map, or List-Map data type:</p><ul><li>Include without Processing - Drops the target field from the record and continues processing.</li><li>Send to Error - Passes the record to the pipeline for error handling.</li></ul> |
6.  To hash the entire record, on the Hash Record tab, configure the following properties:

    | Hash Record Property  | Description                                                                                                                                                                                                                                                                                                                                                |
    | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Hash Entire Record    | Hashes the entire record and writes it to a target field, attribute header, or both.                                                                                                                                                                                                                                                                       |
    | Include Record Header | Includes the record header in the hash.                                                                                                                                                                                                                                                                                                                    |
    | Field Separator       | <p>A single character to use as a field separator. The configured field separator is added to the end of all fields before they are hashed. Select one of the following options:</p><ul><li>Tab</li><li>Semicolon</li><li>Comma</li><li>Space</li><li>Other</li></ul><p>When selecting other, enter the character code for the UTF-8 character to use.</p> |
    | Target Field          | Field in the record to use for hashed data. If the field does not exist, Field Hasher creates the field.                                                                                                                                                                                                                                                   |
    | Header Attribute      | Attribute in the record header to use for hashed data. If the attribute does not exist, Field Hasher creates the attribute.                                                                                                                                                                                                                                |
