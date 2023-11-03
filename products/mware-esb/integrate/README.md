# Integrate

WSO2 API Manager 4.2.0 is shipped with an integration runtime (Micro Integrator) with comprehensive enterprise integration capabilities. Therefore, you can now use WSO2 API Manager to develop complex integration services and expose them as managed APIs in an API marketplace. This allows you to enable API-led connectivity across your business using a single platform.

### Integration Strategy <a href="#integration-strategy" id="integration-strategy"></a>

You can now leverage the integration capabilities as well as the API management capabilities of the product to implement any of the following integration strategies.

#### API-led Integration <a href="#api-led-integration" id="api-led-integration"></a>

WSO2 API Manager consists of an API management layer as well as an integration layer, which enables API-led integration through a single platform. The integration layer (Micro Integrator) is used for running the integration APIs, which are developed using WSO2 Integration Studio. The API management layer is used for converting the integration APIs into experience APIs and making them discoverable to developers.

#### Microservices Integration <a href="#microservices-integration" id="microservices-integration"></a>

The Micro Integrator is lightweight and container friendly. This allows you to leverage the comprehensive enterprise messaging capabilities of the Micro Integrator in your decentralized, cloud-native integrations.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/intro/mi-microservices-architecture.png)

If your organization is running on a decentralized, cloud-native, integration architecture where microservices are used for integrating the various APIs, events, and systems, the Micro Integrator can easily function as your Integration microservices and API microservices.

#### Centralized Integration (Enterprise Service Bus) <a href="#centralized-integration-enterprise-service-bus" id="centralized-integration-enterprise-service-bus"></a>

At the heart of the Micro Integrator server is an event-driven, standards-based messaging engine (the Bus). This ESB supports message routing, message transformations, and other types of messaging use cases. If your organization uses an API-driven, centralized, integration architecture, the Micro Integrator can be used as the central integration layer that implements the message mediation logic connecting all the systems, data, events, APIs, etc. in your integration ecosystem.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/intro/mi-esb-architecture.png)
