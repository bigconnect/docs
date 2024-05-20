# mod\_brotli

### Sample Configurations

#### Compression and TLS

Some web applications are vulnerable to an information disclosure attack when a TLS connection carries compressed data. For more information, review the details of the "BREACH" family of attacks.

This is a simple configuration that compresses common text-based content types.

#### Compress only a few types

```
AddOutputFilterByType BROTLI_COMPRESS text/html text/plain text/xml text/css text/javascript application/javascrip
```

### Enabling Compression

#### Compression and TLS

Some web applications are vulnerable to an information disclosure attack when a TLS connection carries compressed data. For more information, review the details of the "BREACH" family of attacks.

#### Output Compression

Compression is implemented by the `BROTLI_COMPRESS` [filter](https://httpd.apache.org/docs/2.4/filter.html). The following directive will enable compression for documents in the container where it is placed:

```
SetOutputFilter BROTLI_COMPRESS
SetEnvIfNoCase Request_URI \.(?:gif|jpe?g|png)$ no-brotli
```

If you want to restrict the compression to particular MIME types in general, you may use the [`AddOutputFilterByType`](https://httpd.apache.org/docs/2.4/mod/mod\_filter.html#addoutputfilterbytype) directive. Here is an example of enabling compression only for the html files of the Apache documentation:

```
<Directory "/your-server-root/manual">
    AddOutputFilterByType BROTLI_COMPRESS text/html
</Directory>
```

#### Note

The `BROTLI_COMPRESS` filter is always inserted after RESOURCE filters like PHP or SSI. It never touches internal subrequests.

#### Note

There is an environment variable `no-brotli`, set via [`SetEnv`](https://httpd.apache.org/docs/2.4/mod/mod\_env.html#setenv), which will disable brotli compression for a particular request, even if it is supported by the client.[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html#page-header)

### Dealing with proxy servers

The [`mod_brotli`](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html) module sends a `Vary: Accept-Encoding` HTTP response header to alert proxies that a cached response should be sent only to clients that send the appropriate `Accept-Encoding` request header. This prevents compressed content from being sent to a client that will not understand it.

If you use some special exclusions dependent on, for example, the `User-Agent` header, you must manually configure an addition to the `Vary` header to alert proxies of the additional restrictions. For example, in a typical configuration where the addition of the `BROTLI_COMPRESS` filter depends on the `User-Agent`, you should add:

```
Header append Vary User-Agent
```

If your decision about compression depends on other information than request headers (_e.g._ HTTP version), you have to set the `Vary` header to the value `*`. This prevents compliant proxies from caching entirely.

#### Example

```
Header set Vary *
```

[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html#page-header)

### Serving pre-compressed content

Since [`mod_brotli`](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html) re-compresses content each time a request is made, some performance benefit can be derived by pre-compressing the content and telling mod\_brotli to serve them without re-compressing them. This may be accomplished using a configuration like the following:

```
<IfModule mod_headers.c>
    # Serve brotli compressed CSS files if they exist
    # and the client accepts brotli.
    RewriteCond "%{HTTP:Accept-encoding}" "br"
    RewriteCond "%{REQUEST_FILENAME}\.br" "-s"
    RewriteRule "^(.*)\.css"              "$1\.css\.br" [QSA]

    # Serve brotli compressed JS files if they exist
    # and the client accepts brotli.
    RewriteCond "%{HTTP:Accept-encoding}" "br"
    RewriteCond "%{REQUEST_FILENAME}\.br" "-s"
    RewriteRule "^(.*)\.js"               "$1\.js\.br" [QSA]


    # Serve correct content types, and prevent double compression.
    RewriteRule "\.css\.br$" "-" [T=text/css,E=no-brotli:1]
    RewriteRule "\.js\.br$"  "-" [T=text/javascript,E=no-brotli:1]


    <FilesMatch "(\.js\.br|\.css\.br)$">
      # Serve correct encoding type.
      Header append Content-Encoding br

      # Force proxies to cache brotli &
      # non-brotli css/js files separately.
      Header append Vary Accept-Encoding
    </FilesMatch>
</IfModule>
```

[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html#page-header)

### BrotliAlterETag Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description) | How the outgoing ETag header should be modified during compression |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)           | `BrotliAlterETag AddSuffix\|NoChange\|Remove`                      |
| [Default:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Default)         | `BrotliAlterETag AddSuffix`                                        |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)         | server config, virtual host                                        |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)           | Extension                                                          |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)           | mod\_brotli                                                        |

The `BrotliAlterETag` directive specifies how the ETag hader should be altered when a response is compressed.

AddSuffix

Append the compression method onto the end of the ETag, causing compressed and uncompressed representations to have unique ETags. In another dynamic compression module, mod\_deflate, this has been the default since 2.4.0. This setting prevents serving "HTTP Not Modified" (304) responses to conditional requests for compressed content.

NoChange

Don't change the ETag on a compressed response. In another dynamic compression module, mod\_deflate, this has been the default prior to 2.4.0. This setting does not satisfy the HTTP/1.1 property that all representations of the same resource have unique ETags.

Remove

Remove the ETag header from compressed responses. This prevents some conditional requests from being possible, but avoids the shortcomings of the preceding options.

[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html#page-header)

### BrotliCompressionMaxInputBlock Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description) | Maximum input block size               |
| ------------------------------------------------------------------------------------- | -------------------------------------- |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)           | `BrotliCompressionMaxInputBlock value` |
| [Default:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Default)         | `(automatic)`                          |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)         | server config, virtual host            |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)           | Extension                              |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)           | mod\_brotli                            |

The `BrotliCompressionMaxInputBlock` directive specifies the maximum input block size between 16 and 24, with the caveat that larger block sizes require more memory.

[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html#page-header)

### BrotliCompressionQuality Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description) | Compression quality              |
| ------------------------------------------------------------------------------------- | -------------------------------- |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)           | `BrotliCompressionQuality value` |
| [Default:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Default)         | `BrotliCompressionQuality 5`     |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)         | server config, virtual host      |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)           | Extension                        |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)           | mod\_brotli                      |

The `BrotliCompressionQuality` directive specifies the compression quality (a value between 0 and 11). Higher quality values result in better, but also slower compression.

[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html#page-header)

### BrotliCompressionWindow Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description) | Brotli sliding compression window size |
| ------------------------------------------------------------------------------------- | -------------------------------------- |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)           | `BrotliCompressionWindow value`        |
| [Default:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Default)         | `BrotliCompressionWindow 18`           |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)         | server config, virtual host            |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)           | Extension                              |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)           | mod\_brotli                            |

The `BrotliCompressionWindow` directive specifies the brotli sliding compression window size (a value between 10 and 24). Larger window sizes can improve compression quality, but require more memory.

[![top](https://httpd.apache.org/docs/2.4/images/up.gif)](https://httpd.apache.org/docs/2.4/mod/mod\_brotli.html#page-header)

### BrotliFilterNote Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description) | Places the compression ratio in a note for logging |
| ------------------------------------------------------------------------------------- | -------------------------------------------------- |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)           | `BrotliFilterNote [type] notename`                 |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)         | server config, virtual host                        |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)           | Extension                                          |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)           | mod\_brotli                                        |

The `BrotliFilterNote` directive specifies that a note about compression ratios should be attached to the request. The name of the note is the value specified for the directive. You can use that note for statistical purposes by adding the value to your [access log](https://httpd.apache.org/docs/2.4/logs.html#accesslog).

#### Example

```
BrotliFilterNote ratio

LogFormat '"%r" %b (%{ratio}n) "%{User-agent}i"' brotli
CustomLog "logs/brotli_log" brotli
```

If you want to extract more accurate values from your logs, you can use the type argument to specify the type of data left as a note for logging. type can be one of:

`Input`Store the byte count of the filter's input stream in the note.`Output`Store the byte count of the filter's output stream in the note.`Ratio`Store the compression ratio (`output/input * 100`) in the note. This is the default, if the type argument is omitted.

Thus you may log it this way:

#### Accurate Logging

```
BrotliFilterNote Input instream
BrotliFilterNote Output outstream
BrotliFilterNote Ratio ratio

LogFormat '"%r" %{outstream}n/%{instream}n (%{ratio}n%%)' brotli
CustomLog "logs/brotli_log" brotli
```

#### See also

* [`mod_log_config`](https://httpd.apache.org/docs/2.4/mod/mod\_log\_config.html)
