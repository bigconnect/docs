# Authentication and SSO

### Getting started with SSO

* OpenID Connect (OIDC)
* Google

#### Using the OAuth token

When a user logs in using SSO then you will be able to access the OAuth token via the binding: `{{ Current User.OAuthToken }}`.

This is useful if you need to use an API that requires a session based authorisation header.
