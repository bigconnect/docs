# URL functions

### Extraction functions <a href="#extraction-functions" id="extraction-functions"></a>

The URL extraction functions extract components from HTTP URLs (or any valid URIs conforming to [**RFC 2396**](https://datatracker.ietf.org/doc/html/rfc2396.html)). The following syntax is supported:

```
[protocol:][//host[:port]][path][?query][#fragment]
```



The extracted components do not contain URI syntax separators such as `:` or `?`.

url\_extract\_fragment(_url_) → varchar

Returns the fragment identifier from `url`.

url\_extract\_host(_url_) → varchar

Returns the host from `url`.

url\_extract\_parameter(_url_, _name_) → varchar

Returns the value of the first query string parameter named `name` from `url`. Parameter extraction is handled in the typical manner as specified by [**RFC 1866#section-8.2.1**](https://datatracker.ietf.org/doc/html/rfc1866.html#section-8.2.1).

url\_extract\_path(_url_) → varchar

Returns the path from `url`.

url\_extract\_port(_url_) → bigint

Returns the port number from `url`.

url\_extract\_protocol(_url_) → varchar

Returns the protocol from `url`:

```
SELECT url_extract_protocol('http://localhost:8080/req_path');
-- http

SELECT url_extract_protocol('https://127.0.0.1:8080/req_path');
-- https

SELECT url_extract_protocol('ftp://path/file');
-- ftp
```

url\_extract\_query(_url_) → varchar

Returns the query string from `url`.

### Encoding functions <a href="#encoding-functions" id="encoding-functions"></a>

url\_encode(_value_) → varchar

Escapes `value` by encoding it so that it can be safely included in URL query parameter names and values:

* Alphanumeric characters are not encoded.
* The characters `.`, `-`, `*` and `_` are not encoded.
* The ASCII space character is encoded as `+`.
* All other characters are converted to UTF-8 and the bytes are encoded as the string `%XX` where `XX` is the uppercase hexadecimal value of the UTF-8 byte.

url\_decode(_value_) → varchar

Unescapes the URL encoded `value`. This function is the inverse of `url_encode()`.
