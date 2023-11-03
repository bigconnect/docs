# Message Routing and Transformation

### Message routing[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-use-case/message-routing-overview/#message-routing) <a href="#message-routing" id="message-routing"></a>

Message routing is one of the most fundamental requirements when integrating systems/services. It considers content-based routing, header-based routing, rules-based routing, and policy-based routing as ways of routing a message. WSO2 Micro Integrator enables these routing capabilities using the concepts of mediators and endpoints.

The following image depicts a form of message routing where a message is routed through the Micro Integrator to the appropriate service. In this case, the Switch/Send mediator can be used.

![Message Routing](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/use-cases-overview/message-routing.png)

### Message transformation[¶](https://apim.docs.wso2.com/en/latest/integrate/integration-use-case/message-routing-overview/#message-transformation) <a href="#message-transformation" id="message-transformation"></a>

The integration of systems that communicate in various message formats is a common business case in enterprise integration. WSO2 Micro Integrator facilitates this use case as the intermediary system bridging the communication gap among the systems.

The following image depicts a typical message transformation scenario using the Transform mediator.

![Message Transformation](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/use-cases-overview/message-transformation.png)

For example, consider a service that returns data in XML format and a mobile client that accepts messages only in JSON format. To allow these two systems to communicate, the intermediary system needs to convert message formats during the communication. This allows the systems to communicate with each other without depending on the message formats supported by each system.
