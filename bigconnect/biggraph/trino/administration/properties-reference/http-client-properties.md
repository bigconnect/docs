# HTTP client properties

HTTP client properties allow you to configure the connection from Trino to external services using HTTP.

The following properties can be used after adding the specific prefix to the property. For example, for OAuth 2.0 authentication, you can enable HTTP for interactions with the external OAuth 2.0 provider by adding the prefix `oauth2-jwk` to the `http-client.connect-timeout` property, and increasing the connection timeout to ten seconds by setting the value to `10`:

```
oauth2-jwk.http-client.connect-timeout=10s
```



The following prefixes are supported:

* `oauth2-jwk` for OAuth 2.0 authentication
* `jwk` for JWT authentication
* `exchange` to configure data transfer between Trino nodes in addition to Exchange properties

### General properties <a href="#general-properties" id="general-properties"></a>

#### `http-client.connect-timeout` <a href="#http-client-connect-timeout" id="http-client-connect-timeout"></a>

* **Type:** duration
* **Default value:** `5s`
* **Minimum value:** `0ms`

Timeout value for establishing the connection to the external service.

#### `max-content-length` <a href="#max-content-length" id="max-content-length"></a>

* **Type:** duration
* **Default value:** `16MB`

Maximum content size for each HTTP request and response.

#### `http-client.request-timeout` <a href="#http-client-request-timeout" id="http-client-request-timeout"></a>

* **Type:** duration
* **Default value:** `5m`
* **Minimum value:** `0ms`

Timeout value for the overall request.

### TLS and security properties <a href="#tls-and-security-properties" id="tls-and-security-properties"></a>

#### `http-client.https.excluded-cipher` <a href="#http-client-https-excluded-cipher" id="http-client-https-excluded-cipher"></a>

* **Type:** string

A comma-separated list of regexes for the names of cipher algorithms to exclude.

#### `http-client.https.included-cipher` <a href="#http-client-https-included-cipher" id="http-client-https-included-cipher"></a>

* **Type:** string

A comma-separated list of regexes for the names of the cipher algorithms to use.

#### `http-client.https.hostname-verification` <a href="#http-client-https-hostname-verification" id="http-client-https-hostname-verification"></a>

* **Type:** boolean
* **Default value:** `true`

Verify that the server hostname matches the server DNS name in the SubjectAlternativeName (SAN) field of the certificate.

#### `http-client.key-store-password` <a href="#http-client-key-store-password" id="http-client-key-store-password"></a>

* **Type:** string

Password for the keystore.

#### `http-client.key-store-path` <a href="#http-client-key-store-path" id="http-client-key-store-path"></a>

* **Type:** string

File path on the server to the keystore file.

#### `http-client.secure-random-algorithm` <a href="#http-client-secure-random-algorithm" id="http-client-secure-random-algorithm"></a>

* **Type:** string

Set the secure random algorithm for the connection. The default varies by operating system. Algorithms are specified according to standard algorithm name documentation.

Possible types include `NativePRNG`, `NativePRNGBlocking`, `NativePRNGNonBlocking`, `PKCS11`, and `SHA1PRNG`.

#### `http-client.trust-store-password` <a href="#http-client-trust-store-password" id="http-client-trust-store-password"></a>

* **Type:** string

Password for the truststore.

#### `http-client.trust-store-path` <a href="#http-client-trust-store-path" id="http-client-trust-store-path"></a>

* **Type:** string

File path on the server to the truststore file.

### Proxy properties <a href="#proxy-properties" id="proxy-properties"></a>

#### `http-client.http-proxy` <a href="#http-client-http-proxy" id="http-client-http-proxy"></a>

* **Type:** string

Host and port for an HTTP proxy with the format `example.net:8080`.

#### `http-client.http-proxy.secure` <a href="#http-client-http-proxy-secure" id="http-client-http-proxy-secure"></a>

* **Type:** boolean
* **Default value:** `false`

Enable HTTPS for the proxy.

#### `http-client.socks-proxy` <a href="#http-client-socks-proxy" id="http-client-socks-proxy"></a>

* **Type:** string

Host and port for a SOCKS proxy.

### Request logging[#](https://trino.io/docs/current/admin/properties-http-client.html#request-logging) <a href="#request-logging" id="request-logging"></a>

#### `http-client.log.compression-enabled` <a href="#http-client-log-compression-enabled" id="http-client-log-compression-enabled"></a>

* **Type:** boolean
* **Default value:** `true`

Enable log file compression. The client uses the `.gz` format for log files.

#### `http-client.log.enabled` <a href="#http-client-log-enabled" id="http-client-log-enabled"></a>

* **Type:** boolean
* **Default value:** `false`

Enable logging of HTTP requests.

#### `http-client.log.flush-interval` <a href="#http-client-log-flush-interval" id="http-client-log-flush-interval"></a>

* **Type:** duration
* **Default value:** `10s`

Frequency of flushing the log data to disk.

#### `http-client.log.max-history` <a href="#http-client-log-max-history" id="http-client-log-max-history"></a>

* **Type:** integer
* **Default value:** `15`

Retention limit of log files in days. Files older than the `max-history` are deleted when the HTTP client creates files for new logging periods.

#### `http-client.log.max-size` <a href="#http-client-log-max-size" id="http-client-log-max-size"></a>

* **Type:** data size
* **Default value:** `1GB`

Maximum total size of all log files on disk.

#### `http-client.log.path`# <a href="#http-client-log-path" id="http-client-log-path"></a>

* **Type:** string
* **Default value:** `var/log/`

Sets the path of the log files. All log files are named `http-client.log`, and have the prefix of the specific HTTP client added. For example, `jwk-http-client.log`.

#### `http-client.log.queue-size` <a href="#http-client-log-queue-size" id="http-client-log-queue-size"></a>

* **Type:** integer
* **Default value:** `10000`
* **Minimum value:** `1`

Size of the HTTP client logging queue.
