# Protocol Switching

The Micro Integrator offers a wide range of integration capabilities from simple message routing to complicated systems that use integrated solutions. Different applications typically use different protocols for communication. Therefore, for two systems to successfully communicate, it is necessary to switch the protocol (that passes from one system) to the protocol compatible with the receiving application.

For example, messages that are received via HTTP may need to be sent to a JMS queue. Further, you can couple the protocol switching feature with the message transformation feature to handle use cases where the content of messages received via one protocol (such as HTTP) are first processed, and then sent out in a completely different message format and protocol.
