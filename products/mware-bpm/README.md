# MWARE BPM

MWARE BPM is a light-weight business process engine written in Java. The BPM process engine allows you to deploy BPMN 2.0 process definitions (an industry XML standard for defining processes), creating process instances of those process definitions, running queries, accessing active or historical process instances and related data, plus much more. This section will gradually introduce various concepts and APIs to do that through examples that you can follow on your own development machine.

MWARE BPM is extremely flexible when it comes to adding it to your application/services/architecture. You can _embed_ the engine in your application or service by including the library, which is available as a JAR. Since it’s a JAR, you can add it easily to any Java environment: Java SE; servlet containers, such as Tomcat or Jetty, Spring; Java EE servers, such as JBoss or WebSphere, and so on. Alternatively, you can use the REST API to communicate over HTTP. There are also several Applications (Modeler, Admin,  IDM and  Task) that offer out-of-the-box example UIs for working with processes and tasks.

Common to all the ways of setting up MWARE BPM is the core engine, which can be seen as a collection of services that expose APIs to manage and execute business processes. The various tutorials below start by introducing how to set up and use this core engine. The sections afterwards build upon the knowledge acquired in the previous sections.

* The first section shows how to run MWARE BPM in the simplest way possible: a regular Java main using only Java SE. Many core concepts and APIs will be explained here.
* The section on the REST API shows how to run and use the same API through REST.
* The section on the REST App, will guide you through the basics of using the out-of-the-box example MWARE BPM REST application.

