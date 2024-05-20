# URL functions

### Extraction functions[#](https://trino.io/docs/current/functions/url.html#extraction-functions) <a href="#extraction-functions" id="extraction-functions"></a>

The URL extraction functions extract components from HTTP URLs (or any valid URIs conforming to [**RFC 2396**](https://datatracker.ietf.org/doc/html/rfc2396.html)). The following syntax is supported:

```
[protocol:][//host[:port]][path][?query][#fragment]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The extracted components do not contain URI syntax separators such as `:` or `?`.

url\_extract\_fragment(_url_) → varchar[#](https://trino.io/docs/current/functions/url.html#url\_extract\_fragment)

Returns the fragment identifier from `url`.

url\_extract\_host(_url_) → varchar[#](https://trino.io/docs/current/functions/url.html#url\_extract\_host)

Returns the host from `url`.

url\_extract\_parameter(_url_, _name_) → varchar[#](https://trino.io/docs/current/functions/url.html#url\_extract\_parameter)

Returns the value of the first query string parameter named `name` from `url`. Parameter extraction is handled in the typical manner as specified by [**RFC 1866#section-8.2.1**](https://datatracker.ietf.org/doc/html/rfc1866.html#section-8.2.1).

url\_extract\_path(_url_) → varchar[#](https://trino.io/docs/current/functions/url.html#url\_extract\_path)

Returns the path from `url`.

url\_extract\_port(_url_) → bigint[#](https://trino.io/docs/current/functions/url.html#url\_extract\_port)

Returns the port number from `url`.

url\_extract\_protocol(_url_) → varchar[#](https://trino.io/docs/current/functions/url.html#url\_extract\_protocol)

Returns the protocol from `url`:

```
SELECT url_extract_protocol('http://localhost:8080/req_path');
-- http

SELECT url_extract_protocol('https://127.0.0.1:8080/req_path');
-- https

SELECT url_extract_protocol('ftp://path/file');
-- ftp
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)url\_extract\_query(_url_) → varchar[#](https://trino.io/docs/current/functions/url.html#url\_extract\_query)

Returns the query string from `url`.

### Encoding functions[#](https://trino.io/docs/current/functions/url.html#encoding-functions) <a href="#encoding-functions" id="encoding-functions"></a>

url\_encode(_value_) → varchar[#](https://trino.io/docs/current/functions/url.html#url\_encode)

Escapes `value` by encoding it so that it can be safely included in URL query parameter names and values:

* Alphanumeric characters are not encoded.
* The characters `.`, `-`, `*` and `_` are not encoded.
* The ASCII space character is encoded as `+`.
* All other characters are converted to UTF-8 and the bytes are encoded as the string `%XX` where `XX` is the uppercase hexadecimal value of the UTF-8 byte.

url\_decode(_value_) → varchar[#](https://trino.io/docs/current/functions/url.html#url\_decode)

Unescapes the URL encoded `value`. This function is the inverse of `url_encode()`.
