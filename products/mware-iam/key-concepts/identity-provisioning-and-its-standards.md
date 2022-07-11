# Identity Provisioning and its Standards

User and identity provisioning plays a key role in propagating users, user groups, and user identities across different systems and SaaS applications. Provisioning is the process of coordinating the creation of user accounts, e-mail authorizations in the form of rules and roles, and other tasks such as provisioning of resources associated with enabling new users.

This is particularly useful when adding new users into your organization. MWARE Identity Server can ensure that provisioning is made easy. A provisioning request can be sent to the Identity Server to add a new user and this user is provisioned to various applications that are configured with the Identity Server.&#x20;

### Inbound and outbound provisioning

Inbound provisioning focuses on how to provision users, user groups, and user identities to the Identity Server. This can be done using the REST API available for SCIM. The following figure is an example of this process.

![Inbound provisioning using MWARE Identity Server](https://is.docs.wso2.com/en/latest/assets/img/getting-started/inbound-provisioning.png)

Outbound provisioning involves provisioning users, user groups, and user identities to external systems or SaaS applications.

![Outbound provisioning using MWARE Identity Server](https://is.docs.wso2.com/en/latest/assets/img/getting-started/outbound-provisioning.png)

### Introducing SPML <a href="#introducing-spml" id="introducing-spml"></a>

Service Provisioning Markup Language (SPML) is an XML-based framework developed by OASIS for exchanging user, resource, and service provisioning information between cooperating organizations. The Service Provisioning Markup Language is the open standard for the integration and interoperation of service provisioning requests.

The goal of SPML is to allow organizations to securely and quickly set up user interfaces for Web services and applications, by letting enterprise platforms such as Web portals, application servers, and service centers generate provisioning requests within and across organizations. This can lead to automation of user or system access and entitlement rights to electronic services across diverse IT infrastructures, so that customers are not locked into proprietary solutions

### Introducing SCIM <a href="#introducing-scim" id="introducing-scim"></a>

The System for Cross-domain Identity Management (SCIM) specification is designed to make managing user identities in cloud based applications and services, easier. User and identity provisioning is a key aspect of any identity management solution. In simple terms, it is to create, maintain, and delete user accounts and related identities in one or more systems or applications in response to business processes that are initiated either by humans directly or by automated tasks.

Today the enterprise solutions adopt products and services from multiple cloud providers in order to accomplish various business requirements. Hence, it is no longer sufficient to maintain user identities only in a corporate LDAP.

In most cases, SaaS providers also need dedicated user accounts created for the cloud service users, which raises the need of proper identity provisioning mechanisms to be in place. Currently, different cloud vendors expose non-standard provisioning APIs that make it a nightmare for the enterprises to develop and maintain proprietary connectors to integrate with multiple SaaS providers. For example, Google exposes the Google Provisioning API for provisioning user accounts in Google Apps Domain.

When enterprise IT systems consist of distributed, heterogeneous components from multiple vendors, and from both in-house and cloud, it is key to have an open standard that all agree upon in order to achieve interoperability and simplicity while getting rid of multiple connectors to perform the same thing.

SCIM is an emerging open standard that defines a comprehensive REST API, along with a platform neutral schema and a SAML binding to facilitate the user management operations across SaaS applications; placing specific emphasis on simplicity and interoperability.
