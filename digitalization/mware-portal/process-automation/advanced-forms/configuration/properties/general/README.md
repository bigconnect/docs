# General

### Default values <a href="#default-values" id="default-values"></a>

For the latest default values of general properties, see `properties-base.xml`.

### XML entity expansion <a href="#xml-entity-expansion" id="xml-entity-expansion"></a>

For security reasons, Advanced Forms disables XML _external_ entities. Other entities are enabled, but subject to a limit. This limit is set to 100,000 entity expansions.

Advanced Forms has a new global setting to control (internal) XML entity expansion. Previously, XML entity expansion, including character entities, was enabled but subject to a limit. Since this version, you can configure XML entity expansion. By default, for security reasons, and since entities are rarely used, this is set to 0. To restore the previous behavior, set this property to a positive number:

```
<property
    as="xs:integer"
    name="oxf.xml-parsing.entity-expansion-limit"
    value="0"/>
```

### URL rewriting <a href="#url-rewriting" id="url-rewriting"></a>

#### oxf.url-rewriting.service.base-uri <a href="#oxf.url-rewriting.service.base-uri" id="oxf.url-rewriting.service.base-uri"></a>

| Name          | `oxf.url-rewriting.service.base-uri`                          |
| ------------- | ------------------------------------------------------------- |
| Purpose       | specify the base URL for rewriting some internal service URLs |
| Type          | `xs:anyURI`                                                   |
| Default Value | Empty. Rewriting is done against the incoming request.        |

Usually Advanced Forms uses the host, port, and context name as seen by the browser, such as:

```
http://www.mycompany.com/forms
```

to infer how to reach itself when calling some service URLs. But in some cases, Advanced Forms cannot reach to itself this way and an explicit base URL must be specified with this property.

Such cases include:

* accessing the server through different host names (like `https://foo/forms` and `https://bar/forms` reaching the same Advanced Forms instance)
* accessing the embedded eXist database (for demo purposes) when the request goes through a reverse proxy

When you are in such configurations, please make sure to set `oxf.url-rewriting.service.base-uri` to point to the local servlet container instance, for example:

```
<property
    as="xs:anyURI"  
    name="oxf.url-rewriting.service.base-uri"              
    value="http://localhost:8080/forms"/>
```

This property is only used for the following:

* access to the embedded eXist database
* access to custom services located in the Advanced Forms web app (there are none by default)

You _don't need_ to set this property if:

* you do not use the embedded eXist or custom services
* or you use the embedded eXist database or a custom service and
  * you are running your servlet container on a local computer for testing or deployment
  * or your external server name and port are accessible from the servlet container

When things don't work out of the box, typically when the network setup contains a front-end web server and/or prevents a network connection from the servlet container to itself, setting it to the following is usually enough:

```
<property
    as="xs:anyURI"
    name="oxf.url-rewriting.service.base-uri"
    value="http://localhost:8080/forms"/>
```

Make sure to adjust the port and prefix as needed.

### Encryption properties <a href="#encryption-properties" id="encryption-properties"></a>

#### oxf.crypto.password <a href="#oxf.crypto.password" id="oxf.crypto.password"></a>

This property is used to create a private key used for encryption. You must change the default value of the password, even though a random seed is used.

```
<property
  as="xs:string"
  name="oxf.crypto.password"
  value="CHANGE THIS PASSWORD"/>
```

This property is used for:

* Field-level encryption
* Advanced Forms version encryption for cached assets URLs
  * if `oxf.xforms.resources.encode-version` is `true`, which is the default
* Upload events encryption
* Hashes for the `metadata` format
* Hashing internal upload URLs to prevent against tampering
* Form data encryption for the "Test PDF" feature
* The XForms `hmac()` function

The following uses are considered legacy and not in use by default in Advanced Forms anymore:

* The `$instance` URL parameter encryption
* Form static/dynamic state with client state handling encryption

Advanced Forms will cause an error when starting if the default value for `oxf.crypto.password` is used. This is to prevent you from using the default value in production.

In addition, a password strength checker will also cause an error if the password is too weak. Ideally, use a randomly-generated strong password.

#### oxf.crypto.check-password-strength <a href="#oxf.crypto.check-password-strength" id="oxf.crypto.check-password-strength"></a>

This property enables or disables the password strength checker. The default is `true` and enables the checker.

```
<property
    as="xs:boolean"
    name="oxf.crypto.check-password-strength"
    value="true"/>
```

When this is set to `true`, passwords are checked upon first use for strength. This is a baseline check only. In any case, you should use strong passwords and keep them secret, especially for production use.

The following passwords are checked:

* `oxf.crypto.password`
* `oxf.fr.access-token.password`
* `oxf.fr.field-encryption.password`

#### oxf.crypto.key-length <a href="#oxf.crypto.key-length" id="oxf.crypto.key-length"></a>

This property specifies the length of the AES encryption key. The default is 128 bits.

```
<property
  as="xs:integer"
  name="oxf.crypto.key-length"
  value="128"/>
```

Higher strength encryption is usually not enabled by default in the JVM. See [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files](http://www.oracle.com/technetwork/java/javase/downloads/jce-6-download-429243.html). When higher strength encryption is available, this value can be changed to 256, for example.

#### oxf.crypto.hash-algorithm <a href="#oxf.crypto.hash-algorithm" id="oxf.crypto.hash-algorithm"></a>

This property specifies the default hash algorithm. The default is `SHA-256`

```
<property
  as="xs:string"
  name="oxf.crypto.hash-algorithm"
  value="SHA-256"/>
```

Not all encryption strengths are enabled by default in the JVM. See [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files](http://www.oracle.com/technetwork/java/javase/downloads/jce-6-download-429243.html).

Advanced Forms uses hash algorithms in at least the following cases:

* to encode random identifiers, such as document ids in Form Runner
* for internal caching purposes
* XForms
  * keys for client-side scripts
  * keys for aggregated JavaScript and CSS resources
  * keys for dynamic xf:output resources
  * HMAC for server-side uploaded files

### Global properties <a href="#global-properties" id="global-properties"></a>

#### oxf.cache.size <a href="#oxf.cache.size" id="oxf.cache.size"></a>

| Name          | `oxf.cache.size`                                |
| ------------- | ----------------------------------------------- |
| Purpose       | set the size of the Advanced Forms object cache |
| Type          | `xs:integer`                                    |
| Default Value | 1000                                            |

Advanced Forms uses an efficient caching system. Advanced Forms automatically determines what can be cached and when to expire objects. This size is reasonable for most applications. A bigger cache tends to make the application faster, but it uses more memory.&#x20;

#### oxf.xpath.environment-variable.enabled <a href="#oxf.xpath.environment-variable.enabled" id="oxf.xpath.environment-variable.enabled"></a>

Access to environment variables with the `environment-variable()` function is disabled by default. If you wish to make this XPath function available, set the following property to `true`.

```
<property 
    as="xs:boolean" 
    name="oxf.xpath.environment-variable.enabled"
    value="true"/>
```

#### oxf.cache.xpath.size <a href="#oxf.cache.xpath.size" id="oxf.cache.xpath.size"></a>

| Name          | `oxf.cache.xpath.size`                         |
| ------------- | ---------------------------------------------- |
| Purpose       | set the size of the Advanced Forms XPath cache |
| Type          | `xs:integer`                                   |
| Default Value | 5000                                           |

This property configures the maximum number of compiled XPath expressions to keep in the XPath cache.

{% hint style="info" %}
_A profiler run shows that 2000 cache entries takes, for fairly typical XPath expressions, about 5 MB of memory._
{% endhint %}

#### Showing the Advanced Forms version number <a href="#showing-the-orbeon-forms-version-number" id="showing-the-orbeon-forms-version-number"></a>

This property controls whether Advanced Forms outputs its version number to the client web browser:

* at the bottom of pages, in particular with Form Runner
* in the `<xh:meta name="generator" content="…">` element
* in combined JavaScript and CSS resource files built by the XForms engine

```
<property
  as="xs:boolean"
  name="oxf.show-version"
  value="false"/>
```

Default:

* `prod` mode: `false`
* `dev` mode: `true`

#### XSLT output location mode <a href="#xslt-output-location-mode" id="xslt-output-location-mode"></a>

During development, the following XSLT transformer configuration helps with line number errors. The following values are allowed:

* `none`: no XSLT output line number information provided. This is recommended for deployment.
* `dumb`: minimal XSLT output line number information provided.
* `smart`: maximal XSLT output line number information provided. This is recommended for development.

Copy

```
<property
    as="xs:string"
    processor-name="oxf:builtin-saxon"
    name="location-mode"
    value="none"/>

<property
    as="xs:string"
    processor-name="oxf:unsafe-builtin-saxon"
    name="location-mode"
    value="none"/>
```

Default:

* `prod` mode: `none`
* `dev` mode: `smart`

### HTTP Server <a href="#http-server" id="http-server"></a>

#### Errors and exceptions <a href="#errors-and-exceptions" id="errors-and-exceptions"></a>

The following property specifies whether the server is allowed to send detailed error and exceptions messages to the browser:

Copy

```
<property
    as="xs:boolean"
    name="oxf.http.exceptions"
    value="false"/>
```

Default:

* `prod` mode: exceptions are not sent to the browser
* `dev` mode: exceptions are sent to the browser

#### Caching <a href="#caching" id="caching"></a>

Advanced Forms allows you to control the value of the `Cache-Control` HTTP caching header it sets in HTTP responses. For pages, i.e. the HTML sent to the browsers for your forms, the default value allows browsers to cache the page but requires the browser to revalidate the cached content with the server. For services, like APIs provided by Form Runner, caching is disabled. You can change these defaults with the following properties.

Copy

```
<property as="xs:string"  name="oxf.http.page.cache-headers">
    Cache-Control: private, max-age=0; Pragma:
</property>
<property as="xs:string"  name="oxf.http.service.cache-headers">
    Cache-Control: no-store
</property>
```

### HTTP Client <a href="#http-client" id="http-client"></a>

See [HTTP client configuration properties](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/898/digitalization/mware-portal/process-automation/advanced-forms/configuration/properties/general/http-client).

### Epilogue and theme properties <a href="#epilogue-and-theme-properties" id="epilogue-and-theme-properties"></a>

#### oxf.epilogue.theme <a href="#oxf.epilogue.theme" id="oxf.epilogue.theme"></a>

| Name          | `oxf.epilogue.theme`             |
| ------------- | -------------------------------- |
| Purpose       | specifies the theme stylesheet   |
| Type          | `xs:anyURI`                      |
| Default Value | `oxf:/config/theme-examples.xsl` |

This can be overwritten for a given app by placing a file `theme.xsl` inside the app directory.

#### oxf.epilogue.theme.embeddable <a href="#oxf.epilogue.theme.embeddable" id="oxf.epilogue.theme.embeddable"></a>

| Name          | `oxf.epilogue.theme.embeddable`                                                   |
| ------------- | --------------------------------------------------------------------------------- |
| Purpose       | specifies the theme stylesheet to use when within a portlet or in embeddable mode |
| Type          | `xs:anyURI`                                                                       |
| Default Value | `oxf:/config/theme-portlet-examples.xsl`                                          |

This can be overwritten for a given app by placing a file `theme-embeddable.xsl` inside the app directory.

#### oxf.epilogue.theme.renderer <a href="#oxf.epilogue.theme.renderer" id="oxf.epilogue.theme.renderer"></a>

| Name          | `oxf.epilogue.theme.renderer`                                                                                         |
| ------------- | --------------------------------------------------------------------------------------------------------------------- |
| Purpose       | specifies the theme stylesheet to use when using the XForms filter, whether in integrated or separate deployment mode |
| Type          | `xs:anyURI`                                                                                                           |
| Default Value | `oxf:/config/theme-plain.xsl`                                                                                         |

#### oxf.epilogue.theme.error <a href="#oxf.epilogue.theme.error" id="oxf.epilogue.theme.error"></a>

| Name          | `oxf.epilogue.theme.error`                              |
| ------------- | ------------------------------------------------------- |
| Purpose       | specifies the theme stylesheet to use on the error page |
| Type          | `xs:anyURI`                                             |
| Default Value | `oxf:/config/theme-error.xsl`                           |

#### oxf.epilogue.use-theme <a href="#oxf.epilogue.use-theme" id="oxf.epilogue.use-theme"></a>

| Name          | `oxf.epilogue.use-theme`                   |
| ------------- | ------------------------------------------ |
| Purpose       | whether a theme stylesheet must be applied |
| Type          | `xs:boolean`                               |
| Default Value | `true`                                     |

#### oxf.epilogue.output-xhtml <a href="#oxf.epilogue.output-xhtml" id="oxf.epilogue.output-xhtml"></a>

| Name          | `oxf.epilogue.output-xhtml`                   |
| ------------- | --------------------------------------------- |
| Purpose       | whether to output XHTML to the browser or not |
| Type          | `xs:boolean`                                  |
| Default Value | `false`                                       |

#### oxf.epilogue.renderer-rewrite <a href="#oxf.epilogue.renderer-rewrite" id="oxf.epilogue.renderer-rewrite"></a>

| Name          | `oxf.epilogue.renderer-rewrite`                                           |
| ------------- | ------------------------------------------------------------------------- |
| Purpose       | whether the XForms renderer used in separate deployment must rewrite URLs |
| Type          | `xs:boolean`                                                              |
| Default Value | `false`                                                                   |

#### oxf.epilogue.process-svg <a href="#oxf.epilogue.process-svg" id="oxf.epilogue.process-svg"></a>

| Name          | `oxf.epilogue.process-svg`                                  |
| ------------- | ----------------------------------------------------------- |
| Purpose       | whether SVG content must be converted server-side to images |
| Type          | `xs:boolean`                                                |
| Default Value | `true`                                                      |

### Email processor properties <a href="#email-processor-properties" id="email-processor-properties"></a>

#### Global SMTP host <a href="#global-smtp-host" id="global-smtp-host"></a>

Configure the SMTP host for all email processors. This global property can be overridden by local processor configurations.

Copy

```
<property
    as="xs:string"
    processor-name="oxf:email"
    name="smtp-host"
    value="mail.example.org"/>
```

#### Global SMTP port <a href="#global-smtp-port" id="global-smtp-port"></a>

Configure the SMTP port for all email processors. This global property can be overridden by local processor configurations.

Copy

```
<property
    as="xs:string"
    processor-name="oxf:email"
    name="smtp-port"
    value="25"/>
```

#### Global SMTP username <a href="#global-smtp-username" id="global-smtp-username"></a>

Configure the SMTP username for all email processors. This global property can be overridden by local processor configurations.

Copy

```
<property
    as="xs:string"
    processor-name="oxf:email"
    name="username"
    value="john"/>
```

#### Global SMTP password <a href="#global-smtp-password" id="global-smtp-password"></a>

Configure the SMTP password for all email processors. This global property can be overridden by local processor configurations.

Copy

```
<property
    as="xs:string"
    processor-name="oxf:email"
    name="password"
    value="secret"/>
```

#### Global SMTP encryption <a href="#global-smtp-encryption" id="global-smtp-encryption"></a>

Configure the SMTP encryption for all email processors. This global property can be overridden by local processor configurations.

Copy

```
<property
    as="xs:string"
    processor-name="oxf:email"
    name="encryption"
    value="tls"/>
```

#### Test SMTP host <a href="#test-smtp-host" id="test-smtp-host"></a>

Configure a test SMTP host for all email processors. This global property when specified overrides all the other SMTP host configurations for all email processors, whether in the processor configuration or using the `smtp-host` property.

Copy

```
<property
    as="xs:string"
    processor-name="oxf:email"
    name="test-to"
    value="joe@example.org"/>
```

This property can easily be commented out for deployment, or placed in `properties-local-dev.xml`.

#### Test recipient <a href="#test-recipient" id="test-recipient"></a>

Configure a test recipient email address for all email processors. This global property when specified overrides all the other SMTP recipient configurations for all email processors.

Copy

```
<property
    as="xs:string"
    processor-name="oxf:email"
    name="test-to"
    value="joe@example.org"/>
```

This property can easily be commented out for deployment, or placed in `properties-local-dev.xml` .

### Rarely used properties <a href="#rarely-used-properties" id="rarely-used-properties"></a>

#### oxf.log4j-config <a href="#oxf.log4j-config" id="oxf.log4j-config"></a>

| Name                        | `oxf.log4j-config`                                       |
| --------------------------- | -------------------------------------------------------- |
| Purpose                     | specify the location of the Log4j 1.x configuration file |
| Type                        | `xs:anyURI`                                              |
| Default Value in Properties | `oxf:/config/log4j.xml`                                  |

{% hint style="info" %}
_You don't usually need to modify this property and the default location for `log4j.xml` should be preserved._
{% endhint %}

#### oxf.log4j2-config <a href="#oxf.log4j2-config" id="oxf.log4j2-config"></a>

| Name                        | `oxf.log4j2-config`                                      |
| --------------------------- | -------------------------------------------------------- |
| Purpose                     | specify the location of the Log4j 2.x configuration file |
| Type                        | `xs:anyURI`                                              |
| Default Value in Properties | `oxf:/config/log4j2.xml`                                 |

{% hint style="info" %}
_You don't usually need to modify this property and the default location for `log4j2.xml` should be preserved._
{% endhint %}

#### oxf.pipeline.processors <a href="#oxf.pipeline.processors" id="oxf.pipeline.processors"></a>

| Name          | `oxf.pipeline.processors`                                                              |
| ------------- | -------------------------------------------------------------------------------------- |
| Purpose       | specify the URL of the XML file with processor definitions for the XPL pipeline engine |
| Type          | `xs:anyURI`                                                                            |
| Default Value | `oxf:/processors.xml`                                                                  |

{% hint style="info" %}
_You don't usually need to modify this property._
{% endhint %}

#### oxf.validation.processor <a href="#oxf.validation.processor" id="oxf.validation.processor"></a>

| Name          | `oxf.validation.processor`                 |
| ------------- | ------------------------------------------ |
| Purpose       | control the automatic processor validation |
| Type          | `xs:boolean`                               |
| Default Value | Enabled                                    |

Many processors validate their configuration input with a schema. This validation is automatic and allows meaningful error reporting. To potentially improve the performance of the application, validation can be disabled in production environments.

{% hint style="info" %}
&#x20;_It is strongly discouraged to disable validation, as validation can highly contribute to the robustness of the application._
{% endhint %}

#### oxf.validation.user <a href="#oxf.validation.user" id="oxf.validation.user"></a>

| Name          | `oxf.validation.user`           |
| ------------- | ------------------------------- |
| Purpose       | control user-defined validation |
| Type          | `boolean`                       |
| Default Value | Enabled                         |

User-defined validation is activated in the XML Pipeline Definition Language with the attributes `schema-href` and `schema-uri`. To potentially improve the performance of the application, validation can be disabled in production environments.

{% hint style="info" %}
_It is strongly discouraged to disable validation, as validation can highly contribute to the robustness of the application._
{% endhint %}

#### sax.inspection <a href="#sax.inspection" id="sax.inspection"></a>

| Name          | `sax.inspection`             |
| ------------- | ---------------------------- |
| Purpose       | enable inspection SAX events |
| Type          | `xs:boolean`                 |
| Default Value | `false`                      |

SAX is the underlying mechanism in Advanced Forms by which processors receive and generate XML data. Given only the constraints of the SAX API, it is possible for a processor to generate an invalid sequence of SAX events. Another processor that receives that invalid sequence of events may or may not be able to deal with it without throwing an exception. Some processors try to process invalid SAX events, while others throw exceptions. This means that when a processor generating an invalid sequence of SAX events is used in a pipeline, the problem might go unnoticed, or it might cause some other processor downstream to throw an exception.

To deal more efficiently with those cases, the `sax.inspection` property can be set to `true`. When it is set to `true`, the pipeline engine checks the outputs of every processor at runtime and makes sure that valid SAX events are generated. When an error is detected, an exception is thrown right away, with information about the processor that generated the invalid SAX events.

There is a performance penalty for enabling SAX events inspection. So this property should not be enabled on a production system.

{% hint style="info" %}
_You don't usually need to enable this property._
{% endhint %}
