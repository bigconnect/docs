# Url

### Encode uri

The "encodeURI" function is used to encode a Uniform Resource Identifier (URI) component by replacing certain characters with escape sequences that represent the UTF-8 encoding of the character. It takes a single argument, which is the string to be encoded.

Example:

handleBars

```Text
{{ encodeURI 'https://myurl?Hello There' }}
```

In the above example, the "encodeURI" function is called with the string "[https://myurl?Hello](https://myurl/?Hello) There". It encodes the string by replacing the space character with `%20`, which is the escape sequence for a space in URI encoding.

Output

```Text
https://myurl?Hello%20There
```

The "encodeURI" function is commonly used to encode special characters in a URL so that they can be safely transmitted or used as part of a URL.

Please note that the "encodeURI" function encodes the entire URI component, including special characters such as colons, slashes, and question marks. If you only want to encode specific parts of a URL, you may need to use other encoding functions such as "encodeURIComponent" or "encodeURIComponen<|endoftext|>

### Escape

The "escape" function takes in a single argument, "str", which is the string that you want to escape. It returns the escaped version of the string by replacing characters with escape sequences.

For example, if you use the "escape" function with the argument "[https://myurl?Hello+There"](https://myurl/?Hello+There%22), it will return "[https://myurl?Hello%20There"](https://myurl/?Hello%20There%22). In this case, the space character ' ' is replaced with '%20', which is the escape sequence for a space in URLs.

This function is useful when you need to use a string in a URL or any other context where certain characters need to be escaped to ensure proper functionality or compatibility.

### Decode uri

The "decodeURI" function takes in a single argument, "str", which is the URI component that you want to decode. It returns the decoded version of the URI component.

For example, if you use the "decodeURI" function with the argument "[https://myurl?Hello%20There"](https://myurl/?Hello%20There%22), it will return "[https://myurl?Hello](https://myurl/?Hello) There". In this case, the `%20` escape sequence is replaced with a space character.

This function is useful when you have a URI component that has been encoded using escape sequences, and you want to decode it to its original form. It reverses the encoding performed by the "encodeURI" or "escape" functions, allowing you to retrieve the original content of the URI component.

### Url resolve

The "urlResolve" function takes two arguments: "base" and "href". It resolves the given "href" URL relative to the "base" URL, similar to how a browser resolves URLs for anchor tags.

For example, if you use the "urlResolve" function with the arguments "[https://myurl](https://myurl/)" and "/api/test", it will return "[https://myurl/api/test"](https://myurl/api/test%22). The function combines the base URL and the href URL to form a complete, resolved URL.

This function is helpful when you need to construct absolute URLs based on a base URL and a relative URL. It ensures that the resulting URL is correctly resolved according to the rules of URL resolution.

### Url parse

The "urlParse" function is used to parse a URL string and convert it into an object.

Example usage:

handleBars

```Text
{{ urlParse 'https://myurl/api/test' }}
```

This example demonstrates how to use the "urlParse" function by passing the URL string '[https://myurl/api/test'](https://myurl/api/test'). The function will parse the URL and return an object representing the parsed components of the URL.

Please note that the specific properties available in the parsed object may vary depending on the structure of the URL being parsed.

### Strip querystring

The "stripQuerystring" function removes the query string from a given URL. It takes a URL as input and returns the URL without the query string. This function is useful when you want to work with just the base URL and remove any query parameters.

Example:

handleBars

```Text
{{ stripQuerystring 'https://myurl/api/test?foo=bar' }}
```

This example removes the query string from the URL '[https://myurl/api/test?foo=bar](https://myurl/api/test?foo=bar)' and returns '[https://myurl/api/test'](https://myurl/api/test').

### Strip protocol

The "stripProtocol" function removes the protocol (e.g., "http://" or "[https://"](https://"/)) from a given URL. It takes a URL as input and returns the URL without the protocol. This function is useful when you want to display a URL without the protocol, especially in cases where you want to display media on secure connections.

Example:

handleBars

```Text
{{ stripProtocol 'https://myurl/api/test' }}
```

This example removes the protocol from the URL '[https://myurl/api/test](https://myurl/api/test)' and returns 'myurl/api/test'.
