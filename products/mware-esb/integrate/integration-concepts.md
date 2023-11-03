# Integration Concepts

Listed below are the key concepts of WSO2 Micro Integrator.

![Key Concepts](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/key-concepts/key-concepts.png)

### Message entry points[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#message-entry-points) <a href="#message-entry-points" id="message-entry-points"></a>

Message entry points are the entities that a message can enter into the Micro Integrator mediation flow.

#### REST APIs[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#rest-apis) <a href="#rest-apis" id="rest-apis"></a>

A REST API in WSO2 Micro Integrator is analogous to a web application deployed in the web container. The REST API will receive messages over the HTTP/S protocol, performs the necessary transformations and processing, and then forwards the messages to a given endpoint. Each API is anchored at a user-defined URL context, much like how a web application deployed in a servlet container is anchored at a fixed URL context. An API will only process requests that fall under its URL context. An API is made of one or more **Resources**, which are logical components of an API that can be accessed by making a particular type of HTTP call.

#### Proxy Services[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#proxy-services) <a href="#proxy-services" id="proxy-services"></a>

A Proxy service is a virtual service that receives messages and optionally processes them before forwarding them to a service at a given endpoint. This approach allows you to perform the necessary message transformations and introduce additional functionality to your services without changing your actual services. Unlike in [REST APIs](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#rest-apis), here the protocol does not always need to be HTTP/S. Proxy Services do support any well-known protocols including HTTP/S, JMS, FTP, FIX, and HL7.

#### Inbound Endpoints[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#inbound-endpoints) <a href="#inbound-endpoints" id="inbound-endpoints"></a>

In [Proxy services](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#proxy-services) and [REST APIs](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#rest-apis) some part of the configuration is global to a particular instance. For example, HTTP port needs to be common for all the REST APIs. The Inbound Endpoints do not contain such global configurations. That gives extra flexibility in configuring the Inbound Endpoints compared to the other two message entry points.

***

### Message processing units[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#message-processing-units) <a href="#message-processing-units" id="message-processing-units"></a>

#### Mediators[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#mediators) <a href="#mediators" id="mediators"></a>

Mediators are individual processing units that perform a specific function on messages that pass through the Micro Integrator. The mediator takes the message received by the message entry point (Proxy service, REST API, or Inbound Endpoint), carries out some predefined actions on it (such as transforming, enriching, filtering), and outputs the modified message.

#### Mediation Sequences[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#mediation-sequences) <a href="#mediation-sequences" id="mediation-sequences"></a>

A mediation sequence is a set of [mediators](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#mediators) organized into a logical flow, allowing you to implement pipes and filter patterns. The mediators in the sequence will perform the necessary message processing and route the message to the required destination.

#### Message Stores and Processors[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#message-stores-and-processors) <a href="#message-stores-and-processors" id="message-stores-and-processors"></a>

A **Message Store** is used by a [mediation sequence](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#mediation-sequences) to temporarily store messages before they are delivered to their destination. This approach is useful for several scenarios. 1. Serving traffic to back-end services that can only accept messages at a given rate, whereas incoming traffic arrives at different rates. This use case is called **request rate matching**. 2. If the back-end service is not available at a particular moment, the message can be kept safely inside the message store until the back-end service becomes available. This use case is called **Guaranteed delivery**

The task of the **Message Processor** is to pick the messages stored in the Message Store and deliver it to the destination.

#### Templates[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#templates) <a href="#templates" id="templates"></a>

A large number of configuration files in the form of [sequences](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#mediation-sequences) and [endpoints](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#endpoints), and transformations can be required to satisfy all the mediation requirements of your system. To keep your configurations manageable, it is important to avoid scattering configuration files across different locations and to avoid duplicating redundant configurations. Templates help minimize this redundancy by creating prototypes that users can use and reuse when needed. WSO2 Micro Integrator can template [sequences](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#mediation-sequences) and [endpoints](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#endpoints).

***

### Message exit points[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#message-exit-points) <a href="#message-exit-points" id="message-exit-points"></a>

#### Endpoints[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#endpoints) <a href="#endpoints" id="endpoints"></a>

A message exit point or an endpoint defines an external destination for a message. An endpoint could represent a URL, a mailbox, a JMS queue, a TCP socket, etc. along with the settings needed for the connection.

#### Connectors[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#connectors) <a href="#connectors" id="connectors"></a>

Connectors allow your mediation flows to connect and interact with external services such as Twitter and Salesforce. Typically, connectors are used to wrap the API of an external service. It is also a collection of [mediation templates](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#templates) that define specific operations that should be performed on the service. Each connector provides operations that perform different actions in that service. For example, the Twitter connector has operations for creating a tweet, getting a user's followers, and more.

To download a required connector, go to the [WSO2 Connector Store](https://store.wso2.com/store).

***

### Data Services[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#data-services) <a href="#data-services" id="data-services"></a>

The data in your organization can be a complex pool of information that is stored in heterogeneous systems. Data services are created for the purpose of decoupling the data from its infrastructure. In other words, when you create a data service in WSO2 Micro Integrator, the data that is stored in a storage system (such as an RDBMS) can be exposed in the form of a service. This allows users (that may be any application or system) to access the data without interacting with the original source of the data. Data services are, thereby, a convenient interface for interacting with the database layer in your organization.

A data service in WSO2 Micro Integrator is a SOAP-based web service by default. However, you also have the option of creating REST resources, which allows applications and systems to consume the data service to have both SOAP-based, and RESTful access to your data.

***

### Other concepts[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#other-concepts) <a href="#other-concepts" id="other-concepts"></a>

#### Scheduled Tasks[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#scheduled-tasks) <a href="#scheduled-tasks" id="scheduled-tasks"></a>

Executing an integration process at a specified time is a common requirement in enterprise integration. For example, in an organization, there can be a need for running an integration process to synchronize two systems every day at the day end.\
In the Micro Integrator, the execution of a message mediation process can be automated to run periodically by using a **Scheduled task**. You can schedule a task to run in the time interval of 't' for 'n' number of times or to run once the Micro Integrator starts. Furthermore, you can use cron expressions for more advanced executing time configuration.

#### Transports[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#transports) <a href="#transports" id="transports"></a>

A transport protocol is responsible for carrying messages that are in a specific format. WSO2 Micro Integrator supports all the widely used transports including HTTP/S, JMS, VFS, as well as domain-specific transports like FIX. Each transport provides a receiver implementation for receiving messages and a sender implementation for sending messages.

#### Service Catalog[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#service-catalog) <a href="#service-catalog" id="service-catalog"></a>

Service Catalog is one of the main attributes that enable the API-first Integration in WSO2 API Manager. Through the Service Catalog, integration services are made discoverable to the API Management layer so that API proxies can directly be created using them.

These integration services can be created using WSO2 Integration Studio and a variety of other platforms. For an Integration Studio user, the service registration happens automatically when exporting the project as a composite application (CApp).

#### Registry[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#registry) <a href="#registry" id="registry"></a>

WSO2 Micro Integrator uses a registry to store various configurations and resources such as [endpoints](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#endpoints). A registry is simply a content store and a metadata repository. Various resources such as XSLT scripts, WSDLs, and configuration files can be stored in a registry and referred to by a key, which is a path similar to a UNIX file path. The WSO2 Micro Integrator uses a [file-based registry](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/mi-setup/deployment/file\_based\_registry) that is configured by default. When you develop your integration artifacts, you can also define and use a [local registry](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/creating-artifacts/registry/creating-local-registry-entries).

#### Message Builders and Formatters[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-key-concepts/#message-builders-and-formatters) <a href="#message-builders-and-formatters" id="message-builders-and-formatters"></a>

When a message comes into WSO2 Micro Integrator, the receiving transport selects a **message builder** based on the message's content type. It uses that builder to process the message's raw payload data and converts it to common XML, which the mediation engine of WSO2 Micro Integrator can then read and understand. WSO2 Micro Integrator includes message builders for text-based and binary content.

Conversely, before a transport sends a message out from WSO2 Micro Integrator, a **message formatter** is used to build the outgoing stream from the message back into its original format. As with message builders, the message formatter is selected based on the message's content type. You can implement new message builders and formatters for custom requirements.
