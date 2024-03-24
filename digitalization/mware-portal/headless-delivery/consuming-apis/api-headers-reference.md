# API Headers Reference

This article documents the available headers you can use when making Headless API requests:

* `Accept`
* `Accept-Language`
* `Authorization`
* `Content-Type`
* `Cookie`

### `Accept` <a href="#accept" id="accept"></a>

Indicates what format the response content should take. Defaults to `json`.

#### Valid Options <a href="#valid-options" id="valid-options"></a>

* `json`
* `xml`

#### Example <a href="#example" id="example"></a>

```bash
curl --header 'Accept: application/xml' 'example.com/o/headless-admin-user/v1.0/user-accounts'
```

### `Accept-Language` <a href="#accept-language" id="accept-language"></a>

For resources containing content translated into multiple languages, indicates the language for the returned response content.

#### Valid Options <a href="#valid-options-1" id="valid-options-1"></a>

All valid language tags (an ISO-639 language identifier plus ISO-3166-1 alpha-2 country identifier); e.g. `en-US`, `es-ES`. Defaults to the default language of the site that houses the requested content.

#### Example <a href="#example-1" id="example-1"></a>

```log
curl --header 'Accept-Language: pt-BR' 'example.com/o/headless-delivery/v1.0/sites/20124/blog-postings'
```

### `Authorization` <a href="#authorization" id="authorization"></a>

Identifies which User is making the request, using the portal’s Authentication framework. (See also `Cookie`.) If both this and `Cookie` are not supplied, the request is attempted as a guest (unauthenticated) user.

#### Valid Options <a href="#valid-options-2" id="valid-options-2"></a>

* `Basic` + Base64-encoded credentials
* `Bearer` + OAuth token

#### Example <a href="#example-2" id="example-2"></a>

```log
curl --header 'Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQK' 'example.com/o/headless-admin-user/v1.0/user-accounts'
```

### `Content-Type` <a href="#content-type" id="content-type"></a>

Allows uploading binary data.

#### Valid Options <a href="#valid-options-3" id="valid-options-3"></a>

* `multipart/form-data`

#### Example <a href="#example-3" id="example-3"></a>

```log
curl --form 'file=@myfile.txt' --header 'Content-Type: multipart/form-data; boundary=ARBITRARY' 'example.com'
```

### `Cookie` <a href="#cookie" id="cookie"></a>

Identifies the User making the request, using the portal’s Authentication framework. (See also `Authorization`.) If both this and `Authorization` are not supplied, the request is attempted as a guest (unauthenticated) user.

#### Valid Options <a href="#valid-options-4" id="valid-options-4"></a>

* `JSESSIONID`

#### Example <a href="#example-4" id="example-4"></a>

```log
curl --header 'Cookie: JSESSIONID=6349351B37C3EE1F6BA4E128107E9A34' 'example.com/o/headless-admin-user/v1.0/user-accounts'
```

### `X-Accept-All-Languages` <a href="#x-accept-all-languages" id="x-accept-all-languages"></a>

Indicates that the server should return the requested content in all available languages. (See also `Accept-Language`.)

#### Valid Options <a href="#valid-options-5" id="valid-options-5"></a>

* `true`
* `false`

#### Example <a href="#example-5" id="example-5"></a>

```log
curl --header 'X-Accept-All-Languages: true' 'example.com/o/headless-admin-user/v1.0/user-accounts'
```

\
