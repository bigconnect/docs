# Protocols

A protocol is used by a network listener to determine the nature of the communication expected by the listener. For example, a protocol determines the timeout of a request, as well as whether HTTPS should be used. The reason the protocol is separate to the listener is so that the same protocol configuration can be used to listen on multiple ports.

Existing protocols can be configured from the admin console or with asadmin commands. The protocol configurations can be found in the Admin Console under **Configurations** → **your-config** → **Network Config** → **Protocols**.

![Protocol Configuration View Location](https://docs.payara.fish/community/docs/\_images/http/protocol-config.png)

### Standard Configuration Settings

The following are the main configuration settings for network listener protocols:

#### Security

Whether security is enabled for the protocol. When disabled, all the SSL/TLS options are ignored.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.security-enabled=[true|false]
```

#### HTTP Settings

The following HTTP settings can be configured:

**Server Name**

The name of the server. This will be sent as the `Server` header in responses.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.server-name-enabled=value
```

**Default Virtual Server**

The default virtual server used for the protocol. Establishes the request priority when multiple virtual servers are configured.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.default-virtual-server=value
```

**Redirect Port**

The port used for SSL transport redirection.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.redirect-port=value
```

**Max Connections**

The maximum number of requests per connection to keep in keep-alive mode.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.max-connections=value
```

**Timeout**

The amount of time in seconds before a connection is deemed as idle. After this, the connection will no longer be in keep-alive state.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.timeout-seconds=value
```

**Upload Timeout Enabled**

Enables closing a connection that is reading bytes slowly after the `configuration-http-upload-timeout-millis` setting is reached.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.upload-timeout-enabled=value
```

**Connection Upload Timeout**

The timeout, in milliseconds, for uploads.

|   | To disable this timeout, set the value to `-1` |
| - | ---------------------------------------------- |

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.connection-upload-timeout-millis=value
```

**Request Timeout**

The timeout, in seconds, for requests.

{% hint style="info" %}
To disable this timeout, set the value to `-1`
{% endhint %}

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.request-timeout-seconds=value
```

**Send Buffer Size**

The size, in bytes, of the send buffer.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.send-buffer-size-bytes=value
```

**Header Buffer Length**

The size, in bytes, of the buffer used to read the request data.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.header-buffer-length-bytes=value
```

**Max Post Size**

The maximum size, in bytes, of POST actions.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.max-post-size-bytes=value
```

**Max Form Post Size**

The maximum size, in bytes, of a POST form.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.max-form-post-size-bytes=value
```

**Max Save Post Size**

The maximum size, in bytes, of a POST request that will be saved during authentication.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.max-save-post-size-bytes=value
```

**URI Encoding**

The character set used to decode the request URIs received.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.uri-encoding=value
```

**Compression**

Whether to enable HTTP/1.1 GZIP compression to save server bandwidth. Will be ignored when HTTP/2 is used.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.compression=[off/on/force]
```

**Compression Level**

The level of compression to be used:

* `-1` corresponds to the default level
* `0` is no compression
* `1` is the best speed
* `9` is the best compression

**Asadmin Command:**

`set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.compression-level=[-1-9]`

**Compression Strategy**

Sets the strategy to be used during HTTP compression.

**Asadmin Command:**

`set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.compression-strategy=[Default/Filtered/Huffman Only]`

**Compressible Mime Types**

Comma separated list of MIME types for which HTTP compression is used.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.compressable-mime-type=value
```

**Compression Minimum Size**

The minimum size of a file that determines whether compression is applied.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.compression-min-size-bytes=value
```

**No-Compression User Agents**

Comma separated list of regular expressions matching user agents for which compression should not be used.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.no-compression-user-agents=value
```

**Restricted User Agents**

List of restricted user agents for which HTTP compression is applied.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.restricted-user-agents=value
```

**Default Response Type**

The default response type if none are matched. Specified as a semicolon delimited string consisting of content type, encoding, language, charset.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.default-response-type=value
```

**Adapter**

The class name of the static resources adapter.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.adapter=value
```

**Comet Support**

Whether comet support is enabled or not.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.comet-support-enabled=[true|false]
```

**DNS Lookup**

Enables Domain Name System lookups.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.dns-lookup-enabled=[true|false]
```

**Trace Enabled**

Enables the TRACE operation.

|   | Setting this to false helps prevent cross-site scripting attacks. |
| - | ----------------------------------------------------------------- |

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.trace-enabled=[true|false]
```

**Auth Pass Through**

Enable this if the network listener receives traffic from an SSL terminating proxy server.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.auth-pass-through-enabled=[true|false]
```

**Chunking Enabled**

Enables HTTP response chunking.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.chunking-enabled=[true|false]
```

**XPoweredBy**

Used to overwrite the default X-Powered-By header string.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.xpowered-by=value
```

**XFrame Options**

When enabled, the X-Frame-Options header will be set to SAMEORIGIN. This can still be overridden by a deployed application.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.xframe-options=[true|false]
```

**Encoded Slash**

Allow encoded slashes (e.g. `%2F`) in URIs.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.encoded-slash-enabled=[true|false]
```

**Websockets Support Enabled**

Allows Websockets to be used.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.websockets-support-enabled=[true|false]
```

**Scheme Mapping**

HTTP header name used for identifying the originating protocol of an HTTP request.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.scheme-mapping=value
```

**Remote User Mapping**

HTTP header name used for identifying the originating user of an HTTP request.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.remote-user-mapping=value
```

**Enable Cookie SameSite**

Enabling to set all cookies to use the SameSite value.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.cookie-same-site-enabled=[true|false]
```

**Cookie SameSite Value**

The values to be used:

* `Lax`: Cookies are not sent on normal cross-site subrequests
* `Strict`: Cookies will only be sent in a first-party context
* `None`: Cookies will be sent in all contexts

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.cookie-same-site-value=[Lax|Strict|None]
```

Users can set up the SameSite Cookie attribute at the **application level** by using the `payara-web.xml` deployment descriptor.

Set this attribute by using the `cookie-properties` tag with a child property named `cookieSameSite`:

```xml
<payara-web-app >
    <cookie-properties>
        <property name="cookieSameSite" value="Strict" />
    </cookie-properties>
</payara-web-app>
```

#### HTTP/2 Settings

The following HTTPv2 specific settings can be configured:

**HTTP/2 Enabled**

Whether to enable HTTP/2 support.

{% hint style="info" %}
HTTP/2 support can only be enabled on a secure network listener
{% endhint %}

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-enabled=[true|false]
```

**Disable HTTP/2 Cipher Check**

Whether TLS connections can be established using insecure cipher suites.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-disable-cipher-check=[true|false]
```

**HTTP/2 Max Concurrent Streams**

The number of maximum concurrent streams allowed per HTTP/2 connection.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-max-concurrent-streams=value
```

**HTTP/2 Initial Window Size**

The initial window size in bytes.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-initial-window-size-in-bytes=value
```

**HTTP/2 Max Frame Payload Size**

The maximum size of the HTTP/2 frame payload to be accepted.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-max-frame-payload-size-in-bytes=value
```

**HTTP/2 Max Header List Size**

The maximum size, in bytes, of the header list.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-max-header-list-size-in-bytes=value
```

**Streams High Water Mark**

The maximum number of streams that are allowed. Excess streams will be cleaned out periodically in proportion to the maximum number of streams

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-streams-high-water-mark=value
```

**Streams Clean Percentage**

The number of streams to process when the high water mark is exceeded.

{% hint style="info" %}
Only closed streams will be removed.
{% endhint %}

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-clean-percentage=value
```

**Streams Clean Frequency Check**

The number of streams that must be closed before checking if the number of streams exceeds the high water mark.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-clean-frequency-check=value
```

**HTTP/2 Push**

Enables server push. This is enabled by default.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.http.http2-push-enabled=[true|false]
```

#### SSL/TLS Settings

The following SSL/TLS settings can be configured:

**TLS V1.2 Enabled**

Whether to enable TLS V1.2 or not. Will be ignored if TLS is disabled.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.tls12-enabled=[true|false]
```

**TLS V1.3 Enabled**

Whether to enable TLS V1.3 or not. Will be ignored if TLS is disabled.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.tls13-enabled=[true|false]
```

{% hint style="info" %}
Support for TLS 1.3 is available with JDK 8 versions after **JDK 1.8.0u261**, unless you’re using **Zulu JDK 1.8.0u222 - 1.8.0u252**, in which case you’ll also need to use the Java option **-XX:+UseOpenJSSE**. This option makes OpenJSSE default TLS provider. OpenJSEE is a JSEE provider created by Azul to support TLS 1.3 on JDK 8. See [TLS 1.3 Support in Zulu 8 with OpenJSSE](https://docs.azul.com/openjsse/index.htm) for more information. Also note that this flag can cause problems with the HTTP/2 support. If you’re using a lower version than 1.8.0u222, checkbox to enable TLS 1.3 will not be visible on the web administration console.

You will need to add the following Java Option: **-Dfish.payara.clientHttpsProtocol=TLSv1.3** to the asadmin script for TLS 1.3 to work with asadmin CLI. This sets the TLS version to 1.3 which will be used by the asadmin client.
{% endhint %}

**Client Authentication**

When enabled, clients will be required to authenticate themselves to the server.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.client-auth-enabled=[true|false]
```

**Certificate Nickname**

The alias of the certificate to be supplied on secure requests. The certificate should be present in the server keystore.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.cert-nickname=value
```

**Key Store**

The name of the keystore file used to secure communication handled by this network listener.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.key-store=value
```

**Trust Store**

The name of the trust store file where trusted certificates are stored.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.trust-store=value
```

**Trust Algorithm**

The name of the trust management algorithm (e.g. `PKIX`) to use for certification path validation.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.trust-algorithm=value
```

**Max Certificate Length**

Maximum number of non self-issued intermediate certificates that can exist in a certification path. Only used if the trust algorithm is set to `PKIX`.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.trust-max-cert-length=value
```

**Enabled Ciphers**

A list of the server enabled ciphers. This string is stores as a comma separated list of the enabled ciphers with a `+` or a `-` at the start of each, depending on whether the cipher is enabled or disabled. E.g. `+TLS_RSA_WITH_AES_256_CBC_SHA,+SSL_RSA_WITH_3DES_EDE_CBC_SHA`.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.ssl3-tls-ciphers=value
```

**Handshake Timeout**

The timeout, in milliseconds, for a handshake. After this timeout the handshake will be aborted.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.handshake-timeout-millis=value
```

**TLS Rollback**

Whether TLS rollback is enabled or not.

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.tls-rollback-enabled=[true|false]
```

**HSTS Enabled**

Whether HSTS is enabled. When enabled, the server will respond to requests with the `Strict-Transport-Security` header. While these requests often return a 301 directing the client to the secure site, this header instructs the requester to make all connections to this site secure for the next year. The header takes the following form:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

The last 2 attributes are added based on the other HSTS configuration options.

{% hint style="info" %}
HSTS can be enabled on any HTTP listener. However, it’s meant to be enabled on a secure HTTP listener if an insecure HTTP listener redirects to the secure listener. When browsers receive the `Strict-Transport-Security` header over HTTPS, they will use HTTPS immediately for all future connections requested over plain HTTP. If the header is retrieved over HTTP, it is ignored by browsers due to security reasons. More information in [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security).
{% endhint %}

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.hsts-enabled=[true|false]
```

When the configuration value is set to `true`, the HSTS header is added with a `max-age` property value of `31536000`. The `max-age` property is not configurable.

**HSTS Subdomains**

When enabled in combination with HSTS, `Strict-Transport-Security` headers will include the `includeSubDomains` attribute. When this is configured, the client will also assume subdomains of the targeted resource require secure connections. The `subdomains` property is not enabled by default and has to be enabled with the following command:

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.hsts-sub-domains=[true|false]
```

**HSTS Preload**

When enabled in combination with HSTS, `Strict-Transport-Security` headers will include the `preload` attribute. When this is configured, the domain of the target resource will be added to the browser preload list, meaning that initial requests to this resource in future won’t be insecure. The `preload` property is not enabled by default and has to be enabled with the following command:

**Asadmin Command:**

```shell
set configs.config.server-config.network-config.protocols.protocol.${protocol-name}.ssl.hsts-preload=[true|false]
```

### Main Asadmin Commands

The following asadmin commands can be used to manage protocol records.

#### `create-protocol`

The `create-protocol` command creates a bare protocol to be configured with `create-http` and `create-ssl`.

```shell
asadmin create-protocol [--help]
           [--securityenabled={false|true}]
           [--target target]
           protocol-name
```

#### `create-http`

The `create-http` command creates a set of HTTP parameters for a created protocol.

```shell
asadmin  create-http [--help]
           --default-virtual-server virtual-server
           [--request-timeout-seconds timeout]
           [--timeout-seconds timeout]
           [--max-connection max-keepalive]
           [--dns-lookup-enabled={false|true}]
           [--servername server-name]
           [--target target]
           protocol-name
```

#### `create-ssl`

The `create-ssl` command creates a set of SSL parameters for a created protocol.

```shell
asadmin create-ssl [--help]
        --certname certname
        --type protocol
        [--ssl3tlsciphers ssl3tlsciphers]
        [--tlsrollbackenabled={false|true}]
        [--clientauthenabled={false|true}]
        [--target target]
        listener-id
```
