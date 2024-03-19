# API Headers Reference

This article documents the available headers you can use when making Headless API requests:

* [`Accept`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#accept)
* [`Accept-Language`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#accept-language)
* [`Authorization`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#authorization)
* [`Content-Type`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#content-type)
* [`Cookie`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#cookie)

### [`Accept`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#accept) <a href="#accept" id="accept"></a>

Indicates what format the response content should take. Defaults to `json`.

#### [Valid Options](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#valid-options) <a href="#valid-options" id="valid-options"></a>

* `json`
* `xml`

#### [Example](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#example) <a href="#example" id="example"></a>

```bash
curl --header 'Accept: application/xml' 'example.com/o/headless-admin-user/v1.0/user-accounts'
```

### [`Accept-Language`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#accept-language) <a href="#accept-language" id="accept-language"></a>

For resources containing content translated into multiple languages, indicates the language for the returned response content.

#### [Valid Options](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#valid-options-1) <a href="#valid-options-1" id="valid-options-1"></a>

All valid language tags (an ISO-639 language identifier plus ISO-3166-1 alpha-2 country identifier); e.g. `en-US`, `es-ES`. Defaults to the default language of the site that houses the requested content.

#### [Example](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#example-1) <a href="#example-1" id="example-1"></a>

```log
curl --header 'Accept-Language: pt-BR' 'example.com/o/headless-delivery/v1.0/sites/20124/blog-postings'
```

### [`Authorization`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#authorization) <a href="#authorization" id="authorization"></a>

Identifies which User is making the request, using DXP’s Authentication framework. (See also [`Cookie`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#cookie).) If both this and `Cookie` are not supplied, the request is attempted as a guest (unauthenticated) user.

#### [Valid Options](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#valid-options-2) <a href="#valid-options-2" id="valid-options-2"></a>

* `Basic` + Base64-encoded credentials
* `Bearer` + OAuth token

#### [Example](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#example-2) <a href="#example-2" id="example-2"></a>

```log
curl --header 'Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQK' 'example.com/o/headless-admin-user/v1.0/user-accounts'
```

### [`Content-Type`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#content-type) <a href="#content-type" id="content-type"></a>

Allows uploading binary data.

#### [Valid Options](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#valid-options-3) <a href="#valid-options-3" id="valid-options-3"></a>

* `multipart/form-data`

#### [Example](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#example-3) <a href="#example-3" id="example-3"></a>

```log
curl --form 'file=@myfile.txt' --header 'Content-Type: multipart/form-data; boundary=ARBITRARY' 'example.com'
```

### [`Cookie`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#cookie) <a href="#cookie" id="cookie"></a>

Identifies the User making the request, using DXP’s Authentication framework. (See also [`Authorization`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#authorization).) If both this and `Authorization` are not supplied, the request is attempted as a guest (unauthenticated) user.

#### [Valid Options](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#valid-options-4) <a href="#valid-options-4" id="valid-options-4"></a>

* `JSESSIONID`

#### [Example](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#example-4) <a href="#example-4" id="example-4"></a>

```log
curl --header 'Cookie: JSESSIONID=6349351B37C3EE1F6BA4E128107E9A34' 'example.com/o/headless-admin-user/v1.0/user-accounts'
```

### [`X-Accept-All-Languages`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#x-accept-all-languages) <a href="#x-accept-all-languages" id="x-accept-all-languages"></a>

Indicates that the server should return the requested content in all available languages. (See also [`Accept-Language`](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#accept-language).)

#### [Valid Options](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#valid-options-5) <a href="#valid-options-5" id="valid-options-5"></a>

* `true`
* `false`

#### [Example](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-headers-reference#example-5) <a href="#example-5" id="example-5"></a>

```log
curl --header 'X-Accept-All-Languages: true' 'example.com/o/headless-admin-user/v1.0/user-accounts'
```

\
