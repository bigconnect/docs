# Web UI properties

The following properties can be used to configure the Web UI.

### `web-ui.authentication.type` <a href="#web-ui-authentication-type" id="web-ui-authentication-type"></a>

* **Type:** string
* **Allowed values:** `FORM`, `FIXED`, `CERTIFICATE`, `KERBEROS`, `JWT`, `OAUTH2`
* **Default value:** `FORM`

The authentication mechanism to allow user access to the Web UI. See Web UI Authentication.

### `web-ui.enabled` <a href="#web-ui-enabled" id="web-ui-enabled"></a>

* **Type:** boolean
* **Default value:** `true`

This property controls whether or not the Web UI is available.

### `web-ui.shared-secret` <a href="#web-ui-shared-secret" id="web-ui-shared-secret"></a>

* **Type:** string
* **Default value:** randomly generated unless set

The shared secret is used to generate authentication cookies for users of the Web UI. If not set to a static value, any coordinator restart generates a new random value, which in turn invalidates the session of any currently logged in Web UI user.

### `web-ui.session-timeout` <a href="#web-ui-session-timeout" id="web-ui-session-timeout"></a>

* **Type:** duration
* **Default value:** `1d`

The duration how long a user can be logged into the Web UI, before the session times out, which forces an automatic log-out.

### `web-ui.user` <a href="#web-ui-user" id="web-ui-user"></a>

* **Type:** string
* **Default value:** None

The username automatically used for authentication to the Web UI with the `fixed` authentication type. See Web UI Authentication.
