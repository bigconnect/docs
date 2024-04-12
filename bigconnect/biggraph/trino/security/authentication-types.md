# Authentication types

Trino supports multiple authentication types to ensure all users of the system are authenticated. Different authenticators allow user management in one or more systems. Using TLS and a configured shared secret are required for all authentications types.

You can configure one or more authentication types with the `http-server.authentication.type` property. The following authentication types and authenticators are available:

* `PASSWORD` for
  * Password file authentication
  * LDAP authentication
  * Salesforce authentication
* `OAUTH2` for OAuth 2.0 authentication
* `KERBEROS` for Kerberos authentication
* `CERTIFICATE` for Certificate authentication
* `JWT` for JWT authentication
* `HEADER` for Header authenticator

Get started with a basic password authentication configuration backed by a password file:

```
http-server.authentication.type=PASSWORD
```

### Multiple authentication types <a href="#multiple-authentication-types" id="multiple-authentication-types"></a>

You can use multiple authentication types, separated with commas in the configuration:

```
http-server.authentication.type=PASSWORD,CERTIFICATE
```

Authentication is performed in order of the entries, and first successful authentication results in access, using the mapped user from that authentication method.

### Multiple password authenticators <a href="#multiple-password-authenticators" id="multiple-password-authenticators"></a>

You can use multiple password authenticator types by referencing multiple configuration files:

```
http-server.authentication.type=PASSWORD
password-authenticator.config-files=etc/ldap1.properties,etc/ldap2.properties,etc/password.properties
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In the preceding example, the configuration files `ldap1.properties` and `ldap2.properties` are regular LDAP authenticator configuration files. The `password.properties` is a password file authenticator configuration file.

Relative paths to the installation directory or absolute paths can be used.

User authentication credentials are first validated against the LDAP server from `ldap1`, then the separate server from `ldap2`, and finally the password file. First successful authentication results in access, and no further authenticators are called.

### Multiple header authenticators[#](https://trino.io/docs/current/security/authentication-types.html#multiple-header-authenticators) <a href="#multiple-header-authenticators" id="multiple-header-authenticators"></a>

You can use multiple header authenticator types by referencing multiple configuration files:

```
http-server.authentication.type=HEADER
header-authenticator.config-files=etc/xfcc.properties,etc/azureAD.properties
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Relative paths to the installation directory or absolute paths can be used.

The pre-configured headers are first validated against the `xfcc` authenticator, then the `azureAD` authenticator. First successful authentication results in access, and no further authenticators are called.
