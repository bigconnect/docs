# Encrypt and Decrypt Fields

The Encrypt and Decrypt Fields processor encrypts or decrypts field values.

You can use the processor to encrypt one or more fields in a record. You can also use the processor to decrypt one or more fields that were encrypted by another Encrypt and Decrypt Fields processor. You cannot use the processor to perform encryption and decryption at the same time. Use an additional processor when you want to perform both tasks.

The Encrypt and Decrypt Fields processor uses the Amazon AWS Encryption SDK to encrypt and decrypt fields. When encrypting fields, the processor encrypts the data key and any additional encryption details, and stores the encrypted details along with the encrypted data. When decrypting fields, the processor extracts the encrypted data key and additional details, decrypts the key, and then uses it to decrypt the data.

You can use Amazon AWS Key Management Service (KMS) as a key provider for the processor, or you can supply the data key in the processor configuration properties. When using Amazon AWS KMS, you specify the KMS Key Amazon Resource Name (ARN). You can use an instance profile or AWS access key pairs to connect to Amazon AWS. When using a user-supplied key, you specify a Base64 encoded key and can optionally configure a key ID.

For both key provider types, you specify the cipher suite and frame size to use. When encrypting data, you can optionally define an encryption context and configure data key caching.

Note: When decrypting fields that were encrypted by an Encrypt and Decrypt Fields processor, you need to use the same key provider, cipher suite, and any additional details, such as encryption contexts, that were used by the processor that encrypted the data.

For information about the structure of AWS-encrypted data, see the [AWS Encryption SDK documentation](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/message-format.html).

### Supported Data Types <a href="#ariaid-title2" id="ariaid-title2"></a>

When encrypting a field, the Encrypt and Decrypt Fields processor includes the data type of the field in the encrypted data. When decrypting the same field, the processor restores the field to its original data type.

The Encrypt and Decrypt Fields processor can encrypt or decrypt string or byte array data. So you can use the processor to encrypt or decrypt data that can be converted to string or byte array.

You can use the Encrypt and Decrypt Fields processor to encrypt or decrypt the following data types:

* Boolean
* Byte
* Byte Array
* Character
* Date
* Datetime
* Decimal
* Double
* Float
* Integer
* Long
* Short
* String
* Time
* Zoned Datetime

### Key Provider <a href="#ariaid-title3" id="ariaid-title3"></a>

When you use the Encrypt and Decrypt Fields processor, you specify the key provider for the stage.

You can use Amazon AWS Key Management System (KMS) as the key provider or you can use your own user-supplied provider or key:

**Amazon AWS KMS**

Uses a master key provided by the AWS KMS service.

Requires configuring the KMS Key ARN property to identify the Amazon Resource Name (ARN) for the Customer Master Keys (CMK). For information about locating the key ARN, see the [AWS KMS documentation](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys.html#find-cmk-id-arn).

You can optionally use AWS Access Key ID and Secret Access Key to connect to AWS.User supplied keyRequires specifying a Base64 encoded master key.

You can use credential functions to access a key from a supported credential store. You can also use the base64EncodeString() function to encode the string returned by the function.

The length of the encoded key must match the length expected by the selected cipher. For example, when using a 256-bit (32 bytes) cipher suite, the key must be 32 bytes in length.

You can optionally include a string key ID to be used when encrypting the data.

#### AWS Credentials <a href="#ariaid-title4" id="ariaid-title4"></a>

When you use Amazon AWS KMS as the key provider, Data Pipelines must pass credentials to AWS.

Use one of the following methods to pass AWS credentials:

**Instance profile**

When Data Pipelines runs on an Amazon EC2 instance that has an associated instance profile, Data Pipelines uses the instance profile credentials to automatically authenticate with AWS.To use an instance profile, do not configure the Access Key ID and Secret Access Key properties.For more information about associating an instance profile with an EC2 instance, see the Amazon EC2 documentation.

**AWS access key pair**

When Data Pipelines does not run on an Amazon EC2 instance or when the EC2 instance doesn’t have an instance profile, you must specify the Access Key ID and Secret Access Key properties in the stage.

Tip: To secure sensitive information such as access key pairs, you can use runtime resources or credential stores.

### Cipher Suite <a href="#ariaid-title5" id="ariaid-title5"></a>

When you use the Encrypt and Decrypt Fields processor, you specify the cipher suite to use. The processor uses the selected cipher suite to encrypt or decrypt the data.

The processor provides the following cipher suites:

* ALG\_AES\_256\_GCM\_IV12\_TAG16\_HKDF\_SHA384\_ECDSA\_P384 (default)
* ALG\_AES\_192\_GCM\_IV12\_TAG16\_HKDF\_SHA384\_ECDSA\_P384
* ALG\_AES\_128\_GCM\_IV12\_TAG16\_HKDF\_SHA256\_ECDSA\_P256
* ALG\_AES\_256\_GCM\_IV12\_TAG16\_HKDF\_SHA256 (no signature)
* ALG\_AES\_192\_GCM\_IV12\_TAG16\_HKDF\_SHA256 (no signature)
* ALG\_AES\_128\_GCM\_IV12\_TAG16\_HKDF\_SHA256 (no signature)
* ALG\_AES\_256\_GCM\_IV12\_TAG16\_NO\_KDF (not recommended)
* ALG\_AES\_192\_GCM\_IV12\_TAG16\_NO\_KDF (not recommended)
* ALG\_AES\_128\_GCM\_IV12\_TAG16\_NO\_KDF (not recommended)

For an overview of how the AWS Encryption SDK supports cipher suites, see the [AWS Encryption SDK documentation](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/supported-algorithms.html). The documentation also provides additional details about [cipher suites](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/algorithms-reference.html).

### Encryption Contexts <a href="#ariaid-title6" id="ariaid-title6"></a>

You can specify encryption contexts to be included in the encrypted data. Encryption contexts, also known as additional authenticated data (AAD), are key value pairs that are encrypted and included with the encrypted data.

Optionally use encryption contexts as an additional tool to prevent tampering with encrypted data.

When used to encrypt data, the encryption contexts are required to decrypt the data as well.

### Data Key Caching <a href="#ariaid-title7" id="ariaid-title7"></a>

By default, the Encrypt and Decrypt Fields processor generates a new data key for each encryption operation. You can enable caching and reusing data keys to increase pipeline performance when security considerations allow.

Consider the possible security ramifications before enabling data key caching. This [AWS blog post](https://aws.amazon.com/blogs/security/aws-encryption-sdk-how-to-decide-if-data-key-caching-is-right-for-your-application/) describes some of the issues to consider. For details on how data key caching works, see the [AWS Encryption SDK documentation](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/data-caching-details.html).

When you enable data key caching, you configure the following properties:

* Cache Capacity
* Max Data Key Age
* Records per Data Key
* Bytes per Data Key

### Encrypt and Decrypt Records <a href="#ariaid-title8" id="ariaid-title8"></a>

You can use the Encrypt and Decrypt Fields processor to encrypt or decrypt a whole record by serializing the record to a single field before passing it to the processor.

You can use the Data Generator processor to serialize the record to the root field of the record. When you configure the Data Generator processor, you specify the data format to use for the serialized record. Use a text-based format, such as JSON, which results in a String field, or a binary format such as Avro which results in a Byte Array field.

### Configuring an Encrypt and Decrypt Field Processor <a href="#ariaid-title9" id="ariaid-title9"></a>

Configure an Encrypt and Decrypt Field processor to encrypt or decrypt field values.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Action tab, configure the following properties:

    | Action Property | Description                                                                                                                                                                                            |
    | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Mode            | The action for the processor to perform: encrypting or decrypting data in the specified fields.                                                                                                        |
    | Fields          | <p>Field paths for the fields to encrypt.</p><p></p><p>Tip: To encrypt an entire record, you can use a Data Generator processor earlier in the pipeline to serialize the record to a single field.</p> |
3.  On the Key Provider tab, configure the following properties:

    | Key Provider Property    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Master Key Provider      | <p>The data key provider for encoding or decoding data:</p><ul><li>Amazon AWS KMS - Uses data keys from the Amazon AWS Key Management Service.</li><li>User Supplied Provider - Uses a user-supplied key provider (configured in system properties).</li><li>User Supplied Key - Uses a Base64 encoded user-supplied key.</li></ul>                                                                                                                                                                                                                                                                                                          |
    | Cipher                   | <p>The cipher suite to use for encoding or decoding data:</p><ul><li>ALG_AES_256_GCM_IV12_TAG16_HKDF_SHA384_ECDSA_P384 (default)</li><li>ALG_AES_192_GCM_IV12_TAG16_HKDF_SHA384_ECDSA_P384</li><li>ALG_AES_128_GCM_IV12_TAG16_HKDF_SHA256_ECDSA_P256</li><li>ALG_AES_256_GCM_IV12_TAG16_HKDF_SHA256 (no signature)</li><li>ALG_AES_192_GCM_IV12_TAG16_HKDF_SHA256 (no signature)</li><li>ALG_AES_128_GCM_IV12_TAG16_HKDF_SHA256 (no signature)</li><li>ALG_AES_256_GCM_IV12_TAG16_NO_KDF (not recommended)</li><li>ALG_AES_192_GCM_IV12_TAG16_NO_KDF (not recommended)</li><li>ALG_AES_128_GCM_IV12_TAG16_NO_KDF (not recommended)</li></ul> |
    | Frame Size               | <p>The frame size in bytes. Use to divide data into multiple frames for encryption.</p><p>To divide data into multiple frames, specify the frame size to use. To use a single frame, enter 0.</p><p>Default is 4096. With this default, data less than 4096 bytes is encrypted in a single frame.</p>                                                                                                                                                                                                                                                                                                                                        |
    | Access Key ID            | <p>AWS access key ID.</p><p>Required when using Amazon AWS KMS for the key provider and not using instance profile credentials.</p><p>For an Amazon AWS KMS key provider only.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Secret Access Key        | <p>AWS secret access key.</p><p>Required when using the Amazon AWS KMS for the key provider and not using instance profile credentials.</p><p>For an Amazon AWS KMS key provider only.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | KMS Key ARN              | <p>Amazon Resource Name (ARN) for the KMS key.</p><p>For information about locating the key ARN, see the <a href="https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys.html#find-cmk-id-arn">AWS KMS documentation</a>.</p><p>Required when using an Amazon AWS KMS key provider.</p>                                                                                                                                                                                                                                                                                                                                          |
    | Base64 Encoded Key       | <p>Base64 encoded data key to use when using a user-supplied key.</p><p>You can use credential functions to access a key from a supported credential store. You can also use the base64EncodeString() function to encode the string returned by the function.</p><p>The length of the encoded key must match the length expected by the selected cipher. For example, when using a 256-bit (32 bytes) cipher suite, the key must be 32 bytes in length.</p>                                                                                                                                                                                  |
    | Key ID                   | <p>An optional key ID to use in addition to the Base64 encoded key when using a user-supplied key.</p><p>Use a string value.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Encryption Context (AAD) | Key value pairs to be used as encryption contexts, also known as additional authenticated data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Data Key Caching         | Enables caching and reusing data keys. Use to improve performance when security considerations allow.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Cache Capacity           | The maximum number of keys to cache in memory.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Max Data Key Age         | The maximum number of seconds that a data key can be used before the data key is retired.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Max Records per Data Key | The maximum number of fields that a data key can encrypt before the data key is retired.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Max Bytes per Data Key   | The maximum number of bytes that a data key can be used to encrypt before the data key is retired.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
