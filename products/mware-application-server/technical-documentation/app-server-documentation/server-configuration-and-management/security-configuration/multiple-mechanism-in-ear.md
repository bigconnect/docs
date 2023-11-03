---
description: Multiple HttpAuthenticationMechanisms in EAR Application
---

# Multiple Mechanism in EAR

Within the Security API specification, an `HttpAuthenticationMechanism` can be defined to retrieve the user credentials from the HTTP Request. These credentials are then used by any configured identity stores to validate the user.

However, a restriction of the Security API specification is that only one `HttpAuthenticationMechanism` can be defined per application. This restriction is also in place for EAR applications file where it’s possible to have multiple web modules.

Payara Server allows to circumvent this restriction so that multiple authentication mechanisms are used in an EAR application that hosts more than one Web module. To this effect, each web module must configure its own authentication mechanism separately.

### How to Configure <a href="#configure" id="configure"></a>

In order to configure a specific `HttpAuthenticationMechanism` for a Web application, define the `fish.payara.security.mechanism` parameter in the _web.xml_ deployment descriptor like in the following case:

```xml
<context-param>
    <param-name>fish.payara.security.mechanism</param-name>
    <param-value>JWT</param-value>
</context-param>
```

The following mechanisms can be used:

| Value                             | Description                                                               |
| --------------------------------- | ------------------------------------------------------------------------- |
| Basic                             | Use _BasicAuthenticationMechanism_ as mechanism.                          |
| Form                              | Use _FormAuthenticationMechanism_ as mechanism.                           |
| CustomForm                        | Use _CustomFormAuthenticationMechanism_ as mechanism.                     |
| JWT                               | Use the custom Payara _JWTAuthenticationMechanism_ as mechanism.          |
| Certificate                       | Use the custom Payara _CertificateAuthenticationMechanism_ as mechanism.  |
| Azure                             | Use the custom Payara _AzureOpenIdAuthenticationMechanism_ as mechanism.  |
| Google                            | Use the custom Payara _GoogleOpenIdAuthenticationMechanism_ as mechanism. |
| OAuth2                            | Use the custom Payara _OAuth2AuthenticationMechanism_ as mechanism.       |
| \<any Fully Qualified Class Name> | Use the `HttpAuthenticationMechanism` indicated by the class name.        |
