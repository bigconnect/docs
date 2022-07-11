# Access Control and Entitlement Management

Today, enterprise applications seek ways of enforcing security that allows the right people to access the right resources with restrictions enforced by enterprise policies. Identity and entitlement management systems, as a whole, attempt to address these security considerations. These systems help enterprise architects and developers in their application requirement implementations and try to significantly ease the burden of security-related implementations. These identity and entitlement management systems are governed by industry standard specifications, hence supporting security requirements for heterogeneous applications becomes easy.

### Types of access control <a href="#types-of-access-control" id="types-of-access-control"></a>

![](<../../../.gitbook/assets/image (755).png>)

The following table describes the various types of access control.

| Access Control Type	               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |   |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| **Access Control Lists**           | Access control lists are the oldest and most basic form of access control. This type was primarily adopted for use in operating systems. This maintains a set of users and operations that can be performed on a resource as a mapping. This is easy to implement due to the use of maps. However, this is not scalable for larger user bases and can become difficult to manage.                                                                                                                                                                                                                                                                                                                              |   |
| **Role-based Access Control**      | <p>Role-based Access Control (RBAC) is an approach used to restrict access to authorized users based on their role. This is a static permission model which provides access control. It is used by the majority of enterprises with more than 500 users.</p><p>This access control type is used when all users are categorized into different roles. The roles define the resources that can be accessed by users assigned to that role. This type of access control reduces management overhead. These users and roles can also be externalized using user stores. These roles need to be managed carefully. There can be instances where a user is assigned to multiple roles. This is a subset of ABAC.</p> |   |
| **Attribute-based Access Control** | <p>Attribute-based access control (ABAC) defines a new access control paradigm whereby access rights are granted to users through the use of policies that combine attributes together.</p><p>Here, authorization happens based on attributes. This access control type addresses the limitations of role-based access control to provide a more fine-grained approach. This can be based on the attributes of the user, the environment, or even the resource itself. This is more flexible when compared with the role-based approach. There is no need to know the user prior to granting access.</p>                                                                                                       |   |
| **Policy-based Access Control**    | <p>This type of access control addresses the requirement to have a more uniform access control mechanism. This helps larger enterprises to have uniform access control for the large amount of organizational units. This is helpful when carrying out security audits.</p><p>This is the most complex form of access control. This involves specifying policies unambiguously using XACML and using authorized attribute sources in the enterprise. Here Policy Based Access Control(PBAC) (also called Rule-based access control) is an extension of ABAC where attributes are used in writing policies.</p>                                                                                                 |   |

### Introducing XACML

XACML (eXtensible Access Control Markup Language) is an XML-based language for access control that has been standardized by the Technical Committee of the OASIS consortium. XACML is popular as a fine grain authorization method among the community. However, there are aspects of XACML that surpasses other policy standards by being a fine-grained authorization mechanism.

Although XACML was introduced as a standard by OASIS in 2003, not many organizations that have adopted it yet. This is because most organization's lack of interest in moving towards a XACML based solution for authorization. The following can be some of the reasons for this:

* Many software designer and developers lack a clear understanding of features, importance, and advantages of XACML.
* It is comparatively difficult to implement a XACML solution when compared with a typical JDBC or hard-coded authorization system.
* Performance of XACML-based authorization system may be less than adequate.
* The complexity of defining and managing XACML policies.

However, current market trends indicate that there is some motivation for XACML-based authorization systems. This section includes some architectural and implementation details on XACML with an existing XACML engine.

To summarize, XACML describes both an access control policy language, request/response language, and reference architecture. The policy language is used to express access control policies (who can do what, when). The request/response language expresses queries about whether a particular access should be allowed (requests) and describes answers to those queries(responses). The reference architecture proposes a standard for deployment of necessary software modules within an infrastructure to allow efficient enforcement of policies.

MWARE Identity Server uses XACML as a tool for controlling access to applications.

{% hint style="info" %}
For more information on XACML specifications and other related information, see [the OASIS website](http://oasis-open.org/committees/xacml).
{% endhint %}

### Why XACML

Most of the organizations still use legacy systems with inbuilt authorization logic. Sometimes, one organization contains a large number of information systems and applications that each system or application uses for their own process for authorization. Today, authorization has become more complex because users within organizations and outside need access to shared data and have the need to collaborate efficiently. Therefore, this has been challenging task to manage these legacy systems, custom authorization systems. However, XACML offers a solution to this problem.

Most traditional authorization systems mostly have the following features:

* Authorization logic is hard-coded into the source code.
* Authorization logic is stored in databases that could only be readable and understandable by the underlying application

The following are the some goals that current organizations are looking at from an authorization system.

* Can business managers (who should determine how access controls would be implemented) define and modify different authorization logic?
* Can we find new IT technicians to manage these legacy systems? Especially when those who developed the system have left the organization?
* Can different authorization logic be modified without any source code changes in a dynamic way?
* Is authorization system capable of evaluating following rule? “X resource can be accessed by the Users who are from [example.com](http://example.com/) domain and whose age is greater than 21 years old”
* If we are going to implement a new information system with the organization, can we re-use the authorization logic of a legacy system?
* Can we achieve a fine-grained authorization level without defining a large number of static combinations?
* Are the authorization systems capable of answering the questions such as: “Can a user, Alex, transfer X amount from Y current account at 1.00pm?“

Clearly, the above goals cannot be achieved from a traditional authorization system. However, a XACML-based authorization solution can help to achieve all of these goals, because XACML:

* is a standard which is ratified by OASIS standards organization.
* is a policy language implemented using XML which is an industry standard.
* supports Attribute-based Access Control (ABAC) and evaluation can be done with the additional data retrieved from Policy Information Point (PIP) which is defined by the XACML reference architecture.

{% hint style="info" %}
**ATTRIBUTE-BASED ACCESS CONTROL(ABAC)**

ABAC is an access control paradigm whereby access rights are granted to users through the use of policies which combine attributes together. The policies can use any type of attributes (user attributes, resource attributes, object, environment attributes etc.). This model supports boolean logic, in which rules contain "IF, THEN" statements about who is making the request, the resource, and the action. For example: IF the requestor is a manager, THEN allow read/write access to sensitive data.
{% endhint %}

* contains reference architecture which is provided to externalize the authorization system. The Policy Decision Point (PDP) offers authorization as a service in your infrastructure. Authorization algorithms can be removed from the application logic and applications can query the PDP via their own Policy Enforcement Points (PEP).
* provides fine-grained authorization with higher level of abstraction by means of policies and rules.
* supports dynamic evaluation of policies by using the Policy Information Point (PIP).
* can publish multiple policies into external PDPs.

### XACML system architecture <a href="#xacml-system-architecture" id="xacml-system-architecture"></a>

Policy Decision Point (PDP) evaluates policies against access requests provided by Policy Enforcement Points (PEP). To provide the decisions, PDP may also need to query a Policy Information Point (PIP) to gather descriptive attributes about the user or any other missing attribute in the request. Policy Administration Point (PAP) is used to manage the PDP and PIP functionality.

![XACML system architecture](https://is.docs.wso2.com/en/latest/assets/img/getting-started/xacml-system-architecture.png)

### XACML terminology <a href="#xacml-terminology" id="xacml-terminology"></a>

The XACML reference architecture illustrated above is comprised of the following four 'building blocks'.

* **Policy Enforcement Point (PEP)** : The system entity that performs access control by making decision requests and enforcing authorization decisions. This is the entity that sends the XACML request to the Policy Decision Point (PDP) and receives an authorization decision.
* **Policy Decision Point (PDP)** : The system entity that evaluates an applicable policy and returns an authorization decision.
* **Policy Information Point (PIP)** : The system entity that acts as a source of attribute values. If there are missing attributes in the XACML request that is sent by PEP, PIP would find them for the PDP to evaluate the policy.
* **Policy Administration Point (PAP)** : The system entity that creates a policy or policy set and manages them.

### XACML engine architecture

The MWARE Identity Server is a major player in the XACML and open source world. The Identity Server supports XACML 3.0, which is based on [Balana XACML implementation](https://github.com/wso2/balana). As the source code, distribution and documentation are available for free, it is possible to analyze and understand the architecture behind it. You can find source code from [here](https://svn.wso2.org/repos/wso2/trunk/commons/balana/).

This section provides some information regarding the architecture of the XACML engine (or the entitlement engine) of the MWARE Identity Server.

The XACML engine of the MWARE Identity Server has two major components, i.e., PAP and PDP. The section below focuses on Policy Decision Point (PDP).

The following diagram represents the component architecture of the PDP.

![](<../../../.gitbook/assets/image (725).png>)

The following provides a more detailed information on the various components available in the PDP architecture.

#### Entitlement Admin Service

All PDP configurations are exposed via this API as a Web service which you need to have certain privileges to access. The following functions can be done by calling this API.

* Invalidating caches
* Refreshing policy, attribute, resource finder modules
* Retrieving PDP configurations
* Includes the function for testing the Test PDP with a given sample request or a sample request with given policies

#### **Entitlement Service**

Actual XACML authorization API is exposed via this API as a Web service. This service supports the following three communication methods with PEP.

* SOAP-based Web service
* Thrift binary protocol (this is much faster than SOAP)
* WS-XACML (This is the standard way but not popular)

#### **Balana PDP**

This is the core of the engine. Balana PDP has been initialized with all the attribute finders, resource finders and policy finder modules that are plugged with MWARE Identity Server.

#### **Balana Test PDP**

This is same as Balana PDP. However, this instance has been initialized with the PAP policy store. It means, policies that are defined by PAP can be evaluated by using this PDP instance. There is no policy caching or decision caching with this. Therefore, this can only be used for testing your policies in the PAP store. Web service API for Balana Test PDP is exposed via the Entitlement Admin Service with method name “doTestRequest()”.

#### **Carbon Policy Finder**

This module finds from different policy stores to evaluate a XACML request. The following is a high-level diagram of how this works.

![](<../../../.gitbook/assets/image (733).png>)

Any number of policy finder modules can be plugged with the Carbon policy finder.

By default, MWARE Identity Server is shipped with a Carbon registry-based policy finder module that can retrieve policies from a registry collection.

All modules are initialized and policies are loaded into memory when the Carbon policy finder is used. Loaded policies are kept as a policy collection where you can use your own implementation to access them. The default implementation uses an in-memory cache and creates an effective policy for a given XACML request.

Therefore, the Carbon policy finder always finds effective policies from the policy collection where it is recommended to be kept in-memory. However, if any update in the policy store happens, the Carbon policy finder can be re-initialized automatically by the module that you have written, or you can re-initialize it by using the Web service API (Entitlement Admin Service). Alternatively, you can do it manually using the Identity Server Management Console.

When you are writing a new module, you need to consider the following in addition to policy retrieving and re-initializing.

* Policy ordering - The order which policies are evaluated.
* Policy combining algorithm
* How to retrieve referenced policies

#### **Carbon Attribute Finder**

This module helps to find the missing attribute for a given XACML request. The following is the high-level diagram for both the Carbon attribute and resource finders.

![](<../../../.gitbook/assets/image (764).png>)

Any number of PIP attribute finder modules can be plugged with the Carbon attribute finder. You need to implement the PIPAttributeFinder interface to write a new module and register it using the entitlement.properties configuration file. By default, MWARE Identity Server is shipped with a DefaultAttributeFinder that communicates with the underlying user store of the Identity Server. The default user store of the Identity Server is ApacheDS where it can be changed.&#x20;

All modules would be initialized and supported attribute Ids are retrieved for each module. During runtime, the Carbon attribute finder checks for the support attribute Id and hands it over to the proper module to handle. A caching machine is used by the Carbon attribute finder for caching the findings or the attribute finder module itself can handle it.

#### **Carbon Resource Finder**

This is used to retrieve children or descendant resources of a given root level resource value. Basically, it is used to fulfill the requirement of a multiple decision profile. The implementation is same as the PIP attribute finder module. By default, MWARE  Identity Server is shipped with DefaultResourceFinder (which is the implementation of PIPResourceFinder) that communicates with Carbon registry resources.

### XACML policy language structure and syntax <a href="#xacml-policy-language-structure-and-syntax" id="xacml-policy-language-structure-and-syntax"></a>

In order to render an authorization decision, it is possible to combine the two separate policies to form the single policy applicable to the request.

XACML defines three top-level policy elements:

* Element that contains a _boolean_ expression that can be evaluated in isolation, but that is not intended to be accessed in isolation by a PDP. So, it is not intended to form the basis of an authorization decision by itself. It is intended to exist in isolation only within a XACML PAP, where it may form the basic unit of management.
* Element that contains a set of elements and a specified procedure for combining the results of their evaluation. It is the basic unit of policy used by the PDP, and so it is intended to form the basis of an authorization decision.
* Element that contains a set of or other elements and a specified procedure for combining the results of their evaluation. It is the standard means for combining separate policies into a single combined policy.

As XACML is used in Attribute-based Access Controlling, in XACML all the attributes are categorized into the following four main categories: But from XACML 3.0, custom categories are also supported.

* Subject
* Resource
* Action
* Environment

A Rule is the most elementary unit of policy. It may exist in isolation only within one of the major actors of the XACML domain. The main components of a Rule are as follows:

* \<Target>- This defines the set of requests to which the rule is intended to apply in the form of a logical expression on attributes in the request.
* \<Effect>- The effect of the rule indicates the rule-writer's intended consequence of a "True" evaluation of the rule. Two values are allowed: "Permit" and "Deny".
* \<Condition>- A Boolean expression that refines the applicability of the rule beyond the predicates implied by its target. Therefore, it may be absent.
* \<ObligationExpressions>- Obligation expressions may be added by the writer of the policy. When a PDP evaluates a policy containing obligation expressions, it evaluates the obligation expressions into obligations and returns certain of those obligations to the PEP in the response context.
* \<AttributeSelector>- This allows the policy to specify an attribute with given identifier, category and data type. The AttributeSelector on the other hand, provides a mean to lookup the value of attributes using a XPath query by specifying the data type and XPath expression. Attribute selectors are then executed against the XML content that may have been sent along in the initial XACML request.

A sample XACML 3.0 policy is as follows:

```xml
<Policy xmlns="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17" PolicyId="samplePolicy" RuleCombiningAlgId="urn:oasis:names:tc:xacml:3.0:rule-combining-algorithm:deny-overrides" Version="1.0">
    <Target>
        <AnyOf>
            <AllOf>
                <Match MatchId="urn:oasis:names:tc:xacml:1.0:function:string-equal">
                    <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">read</AttributeValue>
                    <AttributeDesignator AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" Category="urn:oasis:names:tc:xacml:3.0:attribute-category:action" DataType="http://www.w3.org/2001/XMLSchema#string" MustBePresent="true"/>
                </Match>
            </AllOf>
        </AnyOf>
    </Target>
    <Rule Effect="Permit" RuleId="permit"/>
</Policy>
```

### XACML 2.0 and XACML 3.0 samples <a href="#xacml-20-and-xacml-30-samples" id="xacml-20-and-xacml-30-samples"></a>

The biggest difference between XACML 2.0 and XACML 3.0 for your client app is that the structure of the attributes in the authentication request has changed significantly in XACML 3.0.

In XACML 2.0, attributes were organized into subject, resource, environment, or action categories using XML element tags:

```xml
<Request  xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os  access_control-xacml-2.0-context-schema-os.xsd">
        <Subject>
            <Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                <AttributeValue>Julius Hibbert</AttributeValue>
            </Attribute>
        </Subject>
        <Resource>
            <Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
                  DataType="http://www.w3.org/2001/XMLSchema#anyURI">
                <AttributeValue>http://medico.com/record/patient/BartSimpson</AttributeValue>
            </Attribute>
        </Resource>
        <Action>
            <Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                <AttributeValue>read</AttributeValue>
            </Attribute>
        </Action>
        <Environment/>
</Request>
```

In XACML 3.0, these categories are indicated using XML attributes instead of XML element tags:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Request xsi:schemaLocation="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17 http://docs.oasis-open.org/xacml/3.0/xacml-core-v3-schema-wd-17.xsd" ReturnPolicyIdList="false" CombinedDecision="false" xmlns="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Attributes Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject">
    <Attribute IncludeInResult="false" AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id">
      <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">Julius Hibbert</AttributeValue>
    </Attribute>
  </Attributes>
  <Attributes Category="urn:oasis:names:tc:xacml:3.0:attribute-category:resource">
    <Attribute IncludeInResult="false" AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
      <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#anyURI">http://medico.com/record/patient/BartSimpson</AttributeValue>
    </Attribute>
  </Attributes>
  <Attributes Category="urn:oasis:names:tc:xacml:3.0:attribute-category:action">
    <Attribute IncludeInResult="false" AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id">
      <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">read</AttributeValue>
    </Attribute>
  </Attributes>
  <Attributes Category="urn:oasis:names:tc:xacml:3.0:attribute-category:environment" />
</Request>
```

The `<Subject>` element in XACML 2.0 becomes `<Attributes Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject">` in XACML 3.0, for example. This is the same for the resource, environment, and action categories.
