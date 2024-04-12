# SET SESSION AUTHORIZATION

### Synopsis[#](https://trino.io/docs/current/sql/set-session-authorization.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SET SESSION AUTHORIZATION username
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/set-session-authorization.html#description) <a href="#description" id="description"></a>

Changes the current user of the session. For the `SET SESSION AUTHORIZATION username` statement to succeed, the original user (that the client connected with) must be able to impersonate the specified user. User impersonation can be enabled in the system access control.

### Examples[#](https://trino.io/docs/current/sql/set-session-authorization.html#examples) <a href="#examples" id="examples"></a>

In the following example, the original user when the connection to Trino is made is Kevin. The following sets the session authorization user to John:

```
SET SESSION AUTHORIZATION 'John';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Queries will now execute as John instead of Kevin.

All supported syntax to change the session authorization users are shown below.

Changing the session authorization with single quotes:

```
SET SESSION AUTHORIZATION 'John';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Changing the session authorization with double quotes:

```
SET SESSION AUTHORIZATION "John";
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Changing the session authorization without quotes:

```
SET SESSION AUTHORIZATION John;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/set-session-authorization.html#see-also) <a href="#see-also" id="see-also"></a>

[RESET SESSION AUTHORIZATION](https://trino.io/docs/current/sql/reset-session-authorization.html)

\
