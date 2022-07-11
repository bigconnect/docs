# Architecture

MWARE Identity Server (IS) is a product built on top of WSO2 IAM. Based on the OSGi specification, it enables easy customization and extension through its componentized architecture. This topic describes the architecture of the Identity Server. The users are given the choice of deployment to on-premise servers, private cloud or public cloud without configuration changes.

MWARE Identity Server is used directly by multiple users, through its user-friendly Management Console. Apart from the default admin user (with the user name ‘admin’), other users can be created later by the admin users that have the privileges to create a new user, or by signing up. Each user can have roles, where each role can have privileges assigned to them. A user’s roles can be changed at any time by the admin user.

Apart from such registered users, Identity Server is also used as an identity provider for third party applications, which also have their own sets of users.

### Architecture and process flow

The following diagram depicts the architecture of the Identity Server and the various processes that take place within it.

![](<../../../.gitbook/assets/image (708) (1).png>)

### Authentication framework

The following are the authenticator types in the authentication framework.

#### **Inbound authenticators**

The responsibility of inbound authenticators is to identify and parse all the incoming authentication requests and then build the corresponding response. A given inbound authenticator has two parts.

1. Request Processor
2. Response Builder

For each protocol supported by the Identity Server, there should be an inbound authenticator. This architecture component includes inbound authenticators for [Security Assertion Markup Language (SAML) 2.0](http://saml.xml.org/saml-specifications), [OpenID Connect](http://openid.net/connect/), [OAuth 2.0](https://oauth.net/2/) , and [WS-Federation (passive)](http://docs.oasis-open.org/wsfed/federation/v1.2/ws-federation.html) . In other words, the responsibility of the SAML 2.0 request processor is to accept a SAML request from a service provider, validate the SAML request and then build a common object model understood by the authentication framework and handover the request to it. The responsibility of the SAML response builder is to accept a common object model from the authentication framework and build a SAML response out of it. Both the request processors and the response builders are protocol aware, while the authentication framework is not coupled to any protocol.

#### **Local authenticators**

The responsibility of the local authenticators is to authenticate the user with locally available credentials. This can be either user name /password or even IWA (Integrated Windows Authentication). Local authenticators are decoupled from the Inbound Authenticators. Once the initial request is handed over to the authentication framework from an inbound authenticator, the authentication framework talks to the service provider configuration component to find the set of local authenticators registered with the service provider corresponding to the current authentication request.

Once the local authentication is successfully completed, the local authenticator will notify the framework. The framework will now decide no more authentication is needed and hand over the control to the corresponding response builder of the inbound authenticator.

You can develop your own local authenticators and plug them into the Identity Server.

#### Outbound/federated authenticators

The responsibility of the federated authenticators is to authenticate the user with an external system. This can be with Facebook, Google, Yahoo, LinkedIn, Twitter, Salesforce or any other identity provider. Federated authenticators are decoupled from the Inbound Authenticators. Once the initial request is handed over to the authentication framework from an inbound authenticator, the authentication framework talks to the service provider configuration component to find the set of federated authenticators registered with the service provider corresponding to the current authentication request.

A federated authenticator has no value unless it is associated with an identity provider. The Identity Server out-of-the-box supports Security Assertion Markup Language (SAML) 2.0, OpenID Connect, OAuth 2.0, and WS-Federation (passive). The SAML 2 .0 federated authenticator itself has no value. It has to be associated with an Identity Provider. Google Apps can be an identity provider - with the SAML 2.0 federated authenticator. This federated authenticator knows how to generate a SAML request to the Google Apps and process a SAML response from it.

There are two parts in a federated authenticator.

1. Request Builder
2. Response Processor

Once the federation authentication is successfully completed, the federated authenticator will notify the authentication framework. The framework will now decide no more authentication is needed and hand over the control to the corresponding response builder of the inbound authenticator.

Both the request builder and the response processor are protocol aware while the authentication framework is not coupled to any protocol.

You can develop your own federated authenticators and plug them into the Identity Server.

#### **Multi-option authenticators**

The service provider can define how to authenticate users at the Identity Server, for authentication requests initiated by it. While doing that, each service provider can pick more than one authenticator to allow end users to get multiple login options. This can be a combination of local authenticators and federated authenticators.

#### **Multi-factor authenticators**

The service provider can define how to authenticate users at the Identity Server, for authentication requests initiated by it. While doing that, each service provider can define multiple steps and for each step it can pick more than one authenticator. The authentication framework tracks all the authenticators in each step and proceeds to the next step only if the user authenticates successfully in the current step. It is an AND between steps, while it is an OR between the authenticators in a given step.

### Provisioning framework

The following are the provisioning components available in the provisioning framework.

#### **Inbound provisioning**

Inbound provisioning focuses on how to provision users to the Identity Server. Out-of-the-box, the Identity Server supports inbound provisioning via a Simple Object Access Protocol ( SOAP) based API as well as the System for Cross-domain Identity Management (SCIM) 1.1 API. Both the APIs support HTTP Basic Authentication. If you invoke the provisioning API with Basic Authentication credentials, then where to provision the user (to which user store) will be decided based on the inbound provisioning configuration of the resident service provider.

The SCIM API also supports OAuth 2.0. If the user authenticates to the SCIM API with OAuth credentials, then the system will load the configuration corresponding to the service provider who owns the OAuth client id. If you plan to invoke the SCIM API via a web application or a mobile application, we would highly recommend you to use OAuth instead of Basic Authentication. You simply need to register your application as a service provider in Identity Server and then generate OAuth keys.

#### Just-in-time provisioning

Just-in-time (JIT) provisioning talks about how to provision users to the Identity Server at the time of federated authentication. A service provider initiates the authentication request, the user gets redirected to the Identity Server and then Identity Server redirects the user to an external identity provider for authentication. Just-in-time provisioning gets triggered in such a scenario when the Identity Server receives a positive authentication response from the external identity provider. The Identity Server will provision the user to its internal user store with the user claims from the authentication response.

You configure JIT provisioning against an identity provider - not against service providers. Whenever you associate an identity provider with a service provider for outbound authentication, if the JIT provisioning is enabled for that particular identity provider, then the users from the external identity provider will be provisioned into the Identity Server's internal user store. In the JIT provisioning configuration you can also pick the provisioning user store.

JIT provisioning happens while in the middle of an authentication flow. The provisioning can happen in a blocking mode or in a non-blocking mode. In the blocking mode, the authentication flow will be blocked till the provisioning finishes - while in the non-blocking mode, provisioning happens in a different thread.

#### **Outbound provisioning**

Outbound provisioning talks about provisioning users to external systems. This can be initiated by any of the following.

* Inbound provisioning request (initiated by a service provider or the resident service provider)
* JIT provisioning (initiated by a service provider)
* Adding a user via the management console (initiated by the resident service provider)
* Assigning a user to a provisioning role (initiated by the resident service provider)

MWARE Identity Server supports outbound provisioning with the following connectors. You need to configure one or more outbound provisioning connectors with a given identity provider, and associate the identity provider with a service provider. All the provisioning requests must be initiated by a service provider - and will be provisioned to all the identity providers configured in the outbound provisioning configuration of the corresponding service provider.

* SCIM
* SPML
* SOAP
* Google Apps provisioning API
* Salesforce provisioning API

\
