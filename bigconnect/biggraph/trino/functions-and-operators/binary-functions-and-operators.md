# Binary functions and operators

### Binary operators <a href="#binary-operators" id="binary-operators"></a>

The `||` operator performs concatenation.

### Binary functions <a href="#binary-functions" id="binary-functions"></a>

concat(_binary1_, _..._, _binaryN_) → varbinary

Returns the concatenation of `binary1`, `binary2`, `...`, `binaryN`. This function provides the same functionality as the SQL-standard concatenation operator (`||`).

length(_binary_) → bigint

Returns the length of `binary` in bytes.

lpad(_binary_, _size_, _padbinary_) → varbinary

Left pads `binary` to `size` bytes with `padbinary`. If `size` is less than the length of `binary`, the result is truncated to `size` characters. `size` must not be negative and `padbinary` must be non-empty.

rpad(_binary_, _size_, _padbinary_) → varbinary

Right pads `binary` to `size` bytes with `padbinary`. If `size` is less than the length of `binary`, the result is truncated to `size` characters. `size` must not be negative and `padbinary` must be non-empty.

substr(_binary_, _start_) → varbinary

Returns the rest of `binary` from the starting position `start`, measured in bytes. Positions start with `1`. A negative starting position is interpreted as being relative to the end of the string.

substr(_binary_, _start_, _length_) → varbinary

Returns a substring from `binary` of length `length` from the starting position `start`, measured in bytes. Positions start with `1`. A negative starting position is interpreted as being relative to the end of the string.

reverse(_binary_) → varbinary

Returns `binary` with the bytes in reverse order.

### Base64 encoding functions <a href="#base64-encoding-functions" id="base64-encoding-functions"></a>

The Base64 functions implement the encoding specified in [**RFC 4648**](https://datatracker.ietf.org/doc/html/rfc4648.html).

from\_base64(_string_) → varbinary

Decodes binary data from the base64 encoded `string`.

to\_base64(_binary_) → varchar

Encodes `binary` into a base64 string representation.

from\_base64url(_string_) → varbinary

Decodes binary data from the base64 encoded `string` using the URL safe alphabet.

to\_base64url(_binary_) → varchar

Encodes `binary` into a base64 string representation using the URL safe alphabet.

from\_base32(_string_) → varbinary

Decodes binary data from the base32 encoded `string`.

to\_base32(_binary_) → varchar

Encodes `binary` into a base32 string representation.

### Hex encoding functions <a href="#hex-encoding-functions" id="hex-encoding-functions"></a>

from\_hex(_string_) → varbinary

Decodes binary data from the hex encoded `string`.

to\_hex(_binary_) → varchar

Encodes `binary` into a hex string representation.

### Integer encoding functions <a href="#integer-encoding-functions" id="integer-encoding-functions"></a>

from\_big\_endian\_32(_binary_) → integer

Decodes the 32-bit two’s complement big-endian `binary`. The input must be exactly 4 bytes.

to\_big\_endian\_32(_integer_) → varbinary

Encodes `integer` into a 32-bit two’s complement big-endian format.

from\_big\_endian\_64(_binary_) → bigint

Decodes the 64-bit two’s complement big-endian `binary`. The input must be exactly 8 bytes.

to\_big\_endian\_64(_bigint_) → varbinary

Encodes `bigint` into a 64-bit two’s complement big-endian format.

### Floating-point encoding functions <a href="#floating-point-encoding-functions" id="floating-point-encoding-functions"></a>

from\_ieee754\_32(_binary_) → real

Decodes the 32-bit big-endian `binary` in IEEE 754 single-precision floating-point format. The input must be exactly 4 bytes.

to\_ieee754\_32(_real_) → varbinary

Encodes `real` into a 32-bit big-endian binary according to IEEE 754 single-precision floating-point format.

from\_ieee754\_64(_binary_) → double

Decodes the 64-bit big-endian `binary` in IEEE 754 double-precision floating-point format. The input must be exactly 8 bytes.

to\_ieee754\_64(_double_) → varbinary

Encodes `double` into a 64-bit big-endian binary according to IEEE 754 double-precision floating-point format.

### Hashing functions <a href="#hashing-functions" id="hashing-functions"></a>

crc32(_binary_) → bigint

Computes the CRC-32 of `binary`. For general purpose hashing, use `xxhash64()`, as it is much faster and produces a better quality hash.

md5(_binary_) → varbinary

Computes the MD5 hash of `binary`.

sha1(_binary_) → varbinary

Computes the SHA1 hash of `binary`.

sha256(_binary_) → varbinary

Computes the SHA256 hash of `binary`.

sha512(_binary_) → varbinary

Computes the SHA512 hash of `binary`.

spooky\_hash\_v2\_32(_binary_) → varbinary

Computes the 32-bit SpookyHashV2 hash of `binary`.

spooky\_hash\_v2\_64(_binary_) → varbinary

Computes the 64-bit SpookyHashV2 hash of `binary`.

xxhash64(_binary_) → varbinary

Computes the xxHash64 hash of `binary`.

murmur3(_binary_) → varbinary

Computes the 128-bit [MurmurHash3](https://wikipedia.org/wiki/MurmurHash) hash of `binary`.

> SELECT murmur3(from\_base64(‘aaaaaa’)); – ba 58 55 63 55 69 b4 2f 49 20 37 2c a0 e3 96 ef

### HMAC functions <a href="#hmac-functions" id="hmac-functions"></a>

hmac\_md5(_binary_, _key_) → varbinary

Computes HMAC with MD5 of `binary` with the given `key`.

hmac\_sha1(_binary_, _key_) → varbinary

Computes HMAC with SHA1 of `binary` with the given `key`.

hmac\_sha256(_binary_, _key_) → varbinary

Computes HMAC with SHA256 of `binary` with the given `key`.

hmac\_sha512(_binary_, _key_) → varbinary

Computes HMAC with SHA512 of `binary` with the given `key`.
