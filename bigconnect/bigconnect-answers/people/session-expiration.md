# Session expiration

By default, Answers sessions are valid for two weeks after a user last authenticated (e.g. by entering their email address/password or via an SSO provider). For example, even if you visit your Answers instance every day, you’ll still have to log in again every two weeks.

## Session age

The session age is the maximum time that a person stays logged into Answers (even if the person closes the browser).

You can set the environment variable `MAX_SESSION_AGE`:

```
# Change session expiration to 24 hours
MAX_SESSION_AGE=1440 java -jar answers.jar
```

or set the Java system property:

```
java -DMAX_SESSION_AGE=1440 -jar answers.jar
```

`MAX_SESSION_AGE` is in minutes.

## Session timeout

The session timeout is the maximum time that a person can be inactive (for example, if someone leaves Answers open in a long-forgotten browser tab).

You can toggle this setting from **Admin** > **Authentication**, or set the environment variable `MB_SESSION_TIMEOUT`.

Session timeout is null by default. You can use a session timeout to log people out earlier than the max session age.

## Session cookies

Answers also supports using session cookies, which mean users will only stay authenticated until they close their browser. This can be enabled on a per-user basis by unchecking the “Remember me” box when logging in. Once the user closes their browser, the next time they visit Answers they’ll have to log in again. Session expiration still applies, so even if you leave your browser open forever, you’ll still be required to re-authenticate after two weeks or whatever session expiration you’ve configured.

You can tell Answers to always use session cookies with the environment variable or Java system property `MB_SESSION_COOKIES`:

```
MB_SESSION_COOKIES=true java -jar answers.jar

```

Setting this environment variable will override the behavior of the “Remember me” checkbox and enforce the use of session cookies for all users.

Note that browsers may use “session restoring”, which means they automatically restore their previous session when reopened. In this case, the browser effectively acts as if it was never closed; session cookies will act the same as permanent cookies. For browsers that support this feature, this behavior is usually configurable.
