# Virtual Servers

A virtual server is an object that allows one physical server instance to appear as many. In some ways it is analogous to a network card on a PC: if you have multiple network cards, the network will consider them as two separate hosts despite them being part of one. Virtual Servers do the same thing to your servers, which means for example you could have multiple domain names (not administrative domains) associated with the same server.

When deploying an application, you have the option to deploy to a specific Virtual Server, which will have multiple network listeners associated with it.

## Configuration <a href="#configuration" id="configuration"></a>

Existing virtual servers can be configured from the admin console or with `set` asadmin commands. The virtual server configurations can be found in the admin console under **Configurations** → **your-config** → **Virtual Servers**.

![Configuration Page Location](https://docs.payara.fish/community/docs/\_images/http/virtual-server-config.png)

The following fields can be configured:

***

### ID

The unique ID for the virtual server. The only field which cannot be altered from the admin console. The ID cannot begin with a number.

**Asadmin Command:**

```shell
set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.id=value
```

### Hosts

A comma separated list of values allowed in the `host` request header to select this virtual server. Each virtual server with the same connection group must have a unique host for that group.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.hosts=value,value2`

***

### State

The enabled state of the virtual server. There are 3 options for this property:

* **On** → This means that the virtual server is active and will be ready to serve requests.
* **Off** → This means that the virtual server is inactive, and will return a 404 error for all requests made through it. This will look from the outside as if no applications are deployed to this server.
* **Disabled** → This means that the virtual server is inactive, and will return a 403 error for all requests made through it. This will look from the outside as if the server is intentionally down for maintenance.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.state=on/off/disabled`

***

### SSO

Allows multiple web applications deployed to the same virtual server to share the user authentication state. This would mean that signing into one application would implicitly sign you into all of the others. There are 3 options for this property:

* **True** (enabled) → This enables SSO for applications on the same virtual server.
* **False** (disabled) → This disables SSO for applications on the same virtual server.
* **Inherit** (controlled by HTTP service) → This will inherit from the value specified in the **HTTP Service**.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.sso-enabled=true/false/inherit`

***

### SSO Cookie Http Only

Determines whether to add the `HttpOnly` flag to the `JSESSIONIDSSO` cookie. In practice, this will disallow JavaScript access to the cookie in the browser. This prevents malicious XSS attacks hijacking a Java session. It is enabled by default.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.sso-cookie-http-only=true/false`

***

### Network Listeners

Comma separated list of network listeners which applications deployed to this virtual server are available on.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.network-listeners=listener-1,listener-2`

***

### Default Web Module

This defines which application deployed to this virtual server will be requested if no modules are resolved in the request. For example if `app1` and `app2` have context roots of `/app1` and `/app2` respectively, `app1` will be returned when the user requests `/`.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.default-web-module=app1`

***

### Log File

This defines a file where log messages coming from this virtual server are redirected to. This allows you to separate application logs from server logs.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.log-file=/dir/to/logs/file.log`

***

### Docroot

This defines the directory used as the document root for this virtual server. A document root is a directory where static files can be placed and served from. For example, if you have a web application deployed to [`http://localhost:8080/app1`](http://localhost:8080/app1) and the corresponding virtual server has a docroot value of `/opt/server/docs`, then requesting [`http://localhost:8080/app1/img.jpg`](http://localhost:8080/app1/img.jpg) will fetch the file at `/opt/server/docs/img.jpg`.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.docroot=/dir/to/docs/`

***

**Access Logging**

Enables access logging for this virtual server. Access logging logs all requests made to that virtual server. There are 3 options for this property:

* **True** (enabled) → This enables access logging for applications on the same virtual server.
* **False** (disabled) → This disables access logging for applications on the same virtual server.
* **Inherit** (controlled by HTTP service) → This will inherit from the value specified in the **HTTP Service**.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.access-logging=true/false/inherit`

***

### Access Log Directory

Defines the directory used for access logs.

**Asadmin Command:**

`set configs.config.server-config.http-service.virtual-server.${virtual-server-name}.access-log=/dir/to/logs`

***

#### Additional properties

Defines additional properties for the configuration of a virtual server.

| Key                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Default Value                                     |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------- |
| **sso-max-inactive-seconds**  | The number of seconds of no activity after which a user’s SSO record becomes eligible for purging.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 300                                               |
| **sso-reap-interval-seconds** | The number of seconds between purges of SSO records.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 60                                                |
| **setCacheControl**           | Comma separated list of Cache-Control response directives (See section 14.9 of: [https://www.ietf.org/rfc/rfc2616.txt](https://www.ietf.org/rfc/rfc2616.txt)).                                                                                                                                                                                                                                                                                                                                                                                                                       | none                                              |
| **allowLinking**              | <p>If set to true, resources that are symbolic links will be served for all applications deployed to this virtual server. Can be overridden for an individual application in the glassfish-web.xml with:</p><pre><code>&#x3C;glassfish-web-app>
  &#x3C;property name="allowLinking" value="false" />
&#x3C;/glassfish-web-app>
</code></pre><p>Note: Setting this property to <code>true</code> on Windows systems exposes JSP source code.</p>                                                                                                                                     | false                                             |
| **accessLogWriteInterval**    | The number of seconds between each time the log is written to disk. The access log is written when the buffer is full or when the interval expires. If the value is 0, the buffer is always written even if it’s not full.                                                                                                                                                                                                                                                                                                                                                           | 300                                               |
| **accessLogBufferSize**       | The size, in bytes, of the buffer where access log calls are stored.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 32768                                             |
| **allowRemoteAddress**        | Comma separated list of regular expression patterns to which the remote client’s IP address is compared. The IP must match one of the patterns to be accepted.                                                                                                                                                                                                                                                                                                                                                                                                                       | none                                              |
| **denyRemoteAddress**         | Comma separated list of regular expression patterns to which the remote client’s IP address is compared. The IP must not match any of the patterns to be accepted.                                                                                                                                                                                                                                                                                                                                                                                                                   | none                                              |
| **allowRemoteHost**           | Comma separated list of regular expression patterns to which the remote client’s host name is compared. The host name must match one of the patterns to be accepted.                                                                                                                                                                                                                                                                                                                                                                                                                 | none                                              |
| **denyRemoteHost**            | Comma separated list of regular expression patterns to which the remote client’s host name is compared. The host name must not match any of the patterns to be accepted.                                                                                                                                                                                                                                                                                                                                                                                                             | none                                              |
| **authRealm**                 | Specifies the name attribute of a security realm, which overrides the server’s default realm for standalone web applications deployed to this virtual server. Can be overridden for an individual application with a realm defined in the web.xml.                                                                                                                                                                                                                                                                                                                                   | none                                              |
| **securePagesWithPragma**     | Set this property to false to ensure that for all web applications on this server file downloads using SSL work properly in Internet Explorer.                                                                                                                                                                                                                                                                                                                                                                                                                                       | true                                              |
| **contextXmlDefault**         | Sets the directory of the context.xml used for this virtual server, if one is used. For more information on the context.xml file, see [https://tomcat.apache.org/tomcat-5.5-doc/config/context.html](https://tomcat.apache.org/tomcat-5.5-doc/config/context.html).                                                                                                                                                                                                                                                                                                                  | none                                              |
| **alternatedocroot\_n**       | Specifies an alternate document root, where n is a positive integer that allows specification of more than one.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | none                                              |
| **send-error\_n**             | <p>Specifies custom error page mappings for the virtual server, which are inherited by all web applications deployed on the virtual server, unless overridden in the web.xml. The value is a string, composed of three values:</p><ul><li>code → The HTTP response status code for the custom error page.</li><li>path → The path of the file to be returned, relative to the <code>/config</code> directory.</li><li>reason (optional) → The text of the reason string.</li></ul><p>E.g. <code>send-error_1="code=404 path=/dir/of/file/error.html reason=UNAUTHORIZED"</code>.</p> | none                                              |
| **redirect\_n**               | <p>Specifies a redirect from an old url to a new one. The value is a string, composed of two values:</p><ul><li><code>from</code> → The prefix of the requested URI to match.</li><li><code>url-prefix</code> → The prefix of the new URI to return to the client. The <strong>from</strong> prefix is replaced by this value.</li></ul><p>E.g. <code>redirect_1="from=/fake url-prefix=http://another"</code>.</p>                                                                                                                                                                  | none                                              |
| **listener\_n**               | Specifies the fully qualified class name of a custom Catalina listener, where n is a positive integer that allows the specification of more than one. The listener class must implement the **`org.apache.catalina.ContainerListener`** or **`org.apache.catalina.LifecycleListener`** interface.                                                                                                                                                                                                                                                                                    | none                                              |
| **errorReportValve**          | Specifies the fully qualified class name of a custom valve that produces default error pages for applications on this virtual server. Specify an empty string to disable the default error page mechanism for this virtual server.                                                                                                                                                                                                                                                                                                                                                   | **`org.apache.catalina.valves.ErrorReportValve`** |
