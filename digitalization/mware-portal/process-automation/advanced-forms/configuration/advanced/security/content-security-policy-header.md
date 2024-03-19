# Content-Security-Policy header

### What is the Content-Security-Policy header? <a href="#what-is-the-content-security-policy-header" id="what-is-the-content-security-policy-header"></a>

The [`Content-Security-Policy` HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP), also known as CSP header, is a relatively recent HTTP header which "helps to detect and mitigate certain types of attacks, including [Cross Site Scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site\_scripting) and data injection attacks".

### Disabling inline scripts and CSS <a href="#disabling-inline-scripts-and-css" id="disabling-inline-scripts-and-css"></a>

#### Introduction <a href="#introduction" id="introduction"></a>

Some organizations set the `Content-Security-Policy` header to strict values which disallow inline JavaScript and CSS within HTML pages, for example with `default-src 'self'`.

Advanced Forms no longer produces inline scripts and CSS by default, which allows for these strict values of the `Content-Security-Policy` header. While Advanced Forms already takes measures against XSS and data injection, disabling the use of inline scripts can make Advanced Forms even safer by default.

#### Configuration property <a href="#configuration-property" id="configuration-property"></a>

The following XForms property allows you to re-enable inline scripts and CSS. The default is `false`:

```
<property 
    as="xs:boolean" 
    name="oxf.xforms.inline-resources"                            
    value="true"/>
```

We recommend leaving the value to the default of `false`.

### Generating the Content-Security-Policy header <a href="#generating-the-content-security-policy-header" id="generating-the-content-security-policy-header"></a>

In many cases, the `Content-Security-Policy` header is generated more globally by a reverse proxy or server.

But Advanced Forms is able to produce that header as well, whether just for testing or for deployment. To enable this, simply uncomment the following entry in the Advanced Forms WAR file's `web.xml`:

```
<init-param>
    <param-name>content-security-policy</param-name>
    <param-value>default-src 'self'; img-src 'self' data:</param-value>
</init-param>
```

You can set the `param-value` to any legal value supported by web browsers. In this example, `default-src 'self'` "Refers to the origin from which the protected document is being served, including the same URL scheme and port number. You must include the single quotes." ([Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src))

_NOTE: The `content-security-policy` name must remain in lowercase. It is a configuration parameter name, not the actual header name._
