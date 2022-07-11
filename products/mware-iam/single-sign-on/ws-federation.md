# WS-Federation

WS-Federation (Web Services Federation) describes the management and brokering of trust relationships and security token exchange across Web services and organizational boundaries. WS-Federation is a part of the larger WS-Security framework. For example, WS-Federation builds on the Security Token Service (STS) by providing mechanisms that facilitate interactions. In the WS-Federation Model, an Identity Provider is a Security Token Service (STS).

## Configuring WS-Federation Single Sign-On <a href="#configuring-ws-federation-single-sign-on" id="configuring-ws-federation-single-sign-on"></a>

WSO2 Identity Server's passive security token service (Passive STS) is used as the WS-Federation implementation. The Passive STS is capable of issuing SAML 1.1 and 2.0 security tokens.

{% hint style="info" %}
To request a SAML 2.0 security token, the Request Security Token (RST) should be sent to the passive STS endpoint with the TokenType 'SAMLV2.0' when sending the token request. If there is no RST specified, WSO2 Identity Server will issue a SAML 1.1 token by default.
{% endhint %}

To configure WS-Federation SSO:

1. Expand the **Inbound Authentication Configuration** followed by the **WS-Federation (Passive) Configuration** section and provide the following values.

* **Passive STS Realm** - This should be an unique identifier for the web app. Provide the same realm name given to the web app you are configuring WS-Federation for.
* **Passive STS WReply URL** - Provide the URL of the web app you are configuring WS-Federation for.  This endpoint URL will handle the token response.

Here, the expiration time should be specified in milliseconds.

![ws-federation-passive.png](https://is.docs.wso2.com/en/latest/assets/img/tutorials/ws-federation-passive.png)

2\. Expand the Claim Configuration section and map the relevant claims.

3\. Click **Update** to save changes.
