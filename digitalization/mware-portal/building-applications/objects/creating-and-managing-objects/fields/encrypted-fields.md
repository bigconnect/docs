# Encrypted Fields

MWARE Portal provides the encrypted field type for storing sensitive user data. To use encrypted fields, first add an encryption algorithm and key to your portal properties. Then add an encrypted field to your object definition. Once data is encrypted, only users with the decryption key can read it.

IMPORTANT

Encrypted fields store string values and cannot be used to encrypt images or file types. Filtering, sorting, and indexing are not supported for encrypted fields. Additionally, field encryption is not available for proxy object definitions.

### Adding an Encryption Algorithm and Key <a href="#adding-an-encryption-algorithm-and-key" id="adding-an-encryption-algorithm-and-key"></a>

To use encryption algorithms and keys for objects, you must add these properties to your `portal-ext.properties` file:

```properties
object.encryption.algorithm=[value]

object.encryption.key=[value]
```

Supported algorithm values include `AES`, `Blowfish`, and `DES`. Encryption key values must use base64 encoding. For example:

```properties
object.encryption.algorithm=AES

object.encryption.key=D9z5Rwxkn+8SctNWW/q/OA==
```

### Adding an Encrypted Field <a href="#adding-an-encrypted-field" id="adding-an-encrypted-field"></a>

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _Objects_.
2. Begin editing the desired object definition.
3. Go to the _Fields_ tab and click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ).
4. Enter a _Label_ and _Field Name_.
5. Select the _Encrypted_ field type.
6. Determine whether the field is mandatory.
7.  Click _Save_.

    NOTE

    You cannot save encrypted fields without an encryption algorithm and key.

You can use this field to store encrypted string values. If desired, you can configure the field limit the number of characters allowed.

### Accessing Encrypted Data <a href="#accessing-encrypted-data" id="accessing-encrypted-data"></a>

Only users with the decryption key can read encrypted field values. If your portal instance has a valid encryption key, authorized users can view and export the field’s data via the portal UI or REST APIs. However, if an encryption key is lost or becomes invalid, users cannot access or export entries for any object definition with encrypted fields. When attempted, the portal returns a 400 error and directs users to either insert a valid encryption key or remove the object’s encrypted fields to recover your entries.

IMPORTANT

Losing your key can result in data loss. Please manage your encryption key with care. You are responsible for ensuring your data and key are backed up and stored in a safe place.

\
