# Binary

### Binary Operators <a href="#binary-operators" id="binary-operators"></a>

The `||` operator performs concatenation.

### Binary Functions <a href="#binary-functions" id="binary-functions"></a>

* `concat`(_binary1_, _..._, _binaryN_) → varbinary

Returns the concatenation of `binary1`, `binary2`, `...`, `binaryN`. This function provides the same functionality as the SQL-standard concatenation operator (`||`).

* `length`(_binary_) → bigint

Returns the length of `binary` in bytes.

* `lpad`(_binary_, _size_, _padbinary_) → varbinary

Left pads `binary` to `size` bytes with `padbinary`. If `size` is less than the length of `binary`, the result is truncated to `size` characters. `size` must not be negative and `padbinary` must be non-empty.

* `rpad`(_binary_, _size_, _padbinary_) → varbinary

Right pads `binary` to `size` bytes with `padbinary`. If `size` is less than the length of `binary`, the result is truncated to `size` characters. `size` must not be negative and `padbinary` must be non-empty.

* `substr`(_binary_, _start_) → varbinary

Returns the rest of `binary` from the starting position `start`, measured in bytes. Positions start with `1`. A negative starting position is interpreted as being relative to the end of the string.

* `substr`(_binary_, _start_, _length_) → varbinary

Returns a substring from `binary` of length `length` from the starting position `start`, measured in bytes. Positions start with `1`. A negative starting position is interpreted as being relative to the end of the string.

### Base64 Encoding Functions <a href="#base64-encoding-functions" id="base64-encoding-functions"></a>

The Base64 functions implement the encoding specified in [**RFC 4648**](https://tools.ietf.org/html/rfc4648.html).

* `from_base64`(_string_) → varbinary

Decodes binary data from the base64 encoded `string`.

* `to_base64`(_binary_) → varchar

Encodes `binary` into a base64 string representation.

* `from_base64url`(_string_) → varbinary

Decodes binary data from the base64 encoded `string` using the URL safe alphabet.

* `to_base64url`(_binary_) → varchar

Encodes `binary` into a base64 string representation using the URL safe alphabet.

### Hex Encoding Functions <a href="#hex-encoding-functions" id="hex-encoding-functions"></a>

* `from_hex`(_string_) → varbinary

Decodes binary data from the hex encoded `string`.

* `to_hex`(_binary_) → varchar

Encodes `binary` into a hex string representation.

### Integer Encoding Functions <a href="#integer-encoding-functions" id="integer-encoding-functions"></a>

* `from_big_endian_32`(_binary_) → integer

Decodes the 32-bit two’s complement big-endian `binary`. The input must be exactly 4 bytes.

* `to_big_endian_32`(_integer_) → varbinary

Encodes `integer` into a 32-bit two’s complement big-endian format.

* `from_big_endian_64`(_binary_) → bigint

Decodes the 64-bit two’s complement big-endian `binary`. The input must be exactly 8 bytes.

* `to_big_endian_64`(_bigint_) → varbinary

Encodes `bigint` into a 64-bit two’s complement big-endian format.

### Floating-Point Encoding Functions <a href="#floating-point-encoding-functions" id="floating-point-encoding-functions"></a>

* `from_ieee754_32`(_binary_) → real

Decodes the 32-bit big-endian `binary` in IEEE 754 single-precision floating-point format. The input must be exactly 4 bytes.

* `to_ieee754_32`(_real_) → varbinary

Encodes `real` into a 32-bit big-endian binary according to IEEE 754 single-precision floating-point format.

* `from_ieee754_64`(_binary_) → double

Decodes the 64-bit big-endian `binary` in IEEE 754 double-precision floating-point format. The input must be exactly 8 bytes.

* `to_ieee754_64`(_double_) → varbinary

Encodes `double` into a 64-bit big-endian binary according to IEEE 754 double-precision floating-point format.

### Hashing Functions <a href="#hashing-functions" id="hashing-functions"></a>

* `crc32`(_binary_) → bigint

Computes the CRC-32 of `binary`. For general purpose hashing, use `xxhash64()`, as it is much faster and produces a better quality hash.

* `md5`(_binary_) → varbinary

Computes the MD5 hash of `binary`.

* `sha1`(_binary_) → varbinary

Computes the SHA1 hash of `binary`.

* `sha256`(_binary_) → varbinary

Computes the SHA256 hash of `binary`.

* `sha512`(_binary_) → varbinary

Computes the SHA512 hash of `binary`.

* `spooky_hash_v2_32`(_binary_) → varbinary

Computes the 32-bit SpookyHashV2 hash of `binary`.

* `spooky_hash_v2_64`(_binary_) → varbinary

Computes the 64-bit SpookyHashV2 hash of `binary`.

* `xxhash64`(_binary_) → varbinary

Computes the xxHash64 hash of `binary`.

### HMAC Functions <a href="#hmac-functions" id="hmac-functions"></a>

* `hmac_md5`(_binary_, _key_) → varbinary

Computes HMAC with MD5 of `binary` with the given `key`.

* `hmac_sha1`(_binary_, _key_) → varbinary

Computes HMAC with SHA1 of `binary` with the given `key`.

* `hmac_sha256`(_binary_, _key_) → varbinary

Computes HMAC with SHA256 of `binary` with the given `key`.

* `hmac_sha512`(_binary_, _key_) → varbinary

Computes HMAC with SHA512 of `binary` with the given `key`.
