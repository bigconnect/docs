# Salesforce authentication

Trino can be configured to enable frontend password authentication over HTTPS for clients, such as the CLI, or the JDBC and ODBC drivers. The username and password (or password and security token concatenation) are validated by having the Trino coordinator perform a login to Salesforce.

This allows you to enable users to authenticate to Trino via their Salesforce basic credentials. This can also be used to secure the Web UI.

Note

This is _not_ a Salesforce connector, and does not allow users to query Salesforce data. Salesforce authentication is simply a means by which users can authenticate to Trino, similar to LDAP authentication or Password file authentication.

Using TLS and a configured shared secret is required for Salesforce authentication.

### Salesforce authenticator configuration <a href="#salesforce-authenticator-configuration" id="salesforce-authenticator-configuration"></a>

To enable Salesfore authentication, set the password authentication type in `etc/config.properties`:

```
http-server.authentication.type=PASSWORD
```



In addition, create a `etc/password-authenticator.properties` file on the coordinator with the `salesforce` authenticator name:

```
password-authenticator.name=salesforce
salesforce.allowed-organizations=<allowed-org-ids or all>
```



The following configuration properties are available:

| Property                           | Description                                                                                                                                                                                                                                          |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `salesforce.allowed-organizations` | Comma separated list of 18 character Salesforce.com Organization IDs for a second, simple layer of security. This option can be explicitly ignored using `all`, which bypasses any check of the authenticated user’s Salesforce.com Organization ID. |
| `salesforce.cache-size`            | Maximum number of cached authenticated users. Defaults to `4096`.                                                                                                                                                                                    |
| `salesforce.cache-expire-duration` | How long a cached authentication should be considered valid. Defaults to `2m`.                                                                                                                                                                       |

### Salesforce concepts <a href="#salesforce-concepts" id="salesforce-concepts"></a>

There are two Salesforce specific aspects to this authenticator. They are the use of the Salesforce security token, and configuration of one or more Salesforce.com Organization IDs.

#### Security token <a href="#security-token" id="security-token"></a>

Credentials are a user’s Salesforce username and password if Trino is connecting from a whitelisted IP, or username and password/security token concatenation otherwise. For example, if Trino is _not_ whitelisted, and your password is `password` and security token is `token`, use `passwordtoken` to authenticate.

You can configure a public IP for Trino as a trusted IP by whitelisting an IP range.

#### Salesforce.com organization IDs <a href="#salesforce-com-organization-ids" id="salesforce-com-organization-ids"></a>

You can configure one or more Salesforce Organization IDs for additional security. When the user authenticates, the Salesforce API returns the _18 character_ Salesforce.com Organization ID for the user. The Trino Salesforce authenticator ensures that the ID matches one of the IDs configured in `salesforce.allowed-organizations`.

Optionally, you can configure `all` to explicitly ignore this layer of security.

Admins can find their Salesforce.com Organization ID using the Salesforce Setup UI. This will be the 15 character ID, which can be converted to the 18 character ID.
