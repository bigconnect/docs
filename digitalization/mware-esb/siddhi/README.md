# Siddhi

This section provides information on using and running Siddhi.

Checkout the Siddhi features to get and idea on what it can do in brief.

### Writing Siddhi Applications <a href="#writing-siddhi-applications" id="writing-siddhi-applications"></a>

Writing steam processing logic in Siddhi is all about building Siddhi Applications. A Siddhi Application is a script with `.siddhi` file extension having self-contained stream processing logic. The stream processing constructs, such as streams and queries, defined within a Siddhi App is not visible even to the other Siddhi Apps running in the same JVM.

It is recommended to have different business usecase in separate Siddhi Applications, where it allow users to selectively deploy the applications based on business needs. It is also recommended to move the repeated steam processing logics that exist in multiple Siddhi Applications, such as message retrieval and preprocessing, to a common Siddhi Application, whereby reducing code duplication and improving maintainability. In this case, to pass the events from one Siddhi App to another, users can configure common topic using In-Memory Sink and In-Memory Source in order to communicate between them.

To write Siddhi Applications using Siddhi Streaming SQL refer Siddhi Query Guide for details.

For specific API information on Siddhi functions and features refer Siddhi API Guide.

### Executing Siddhi Applications <a href="#executing-siddhi-applications" id="executing-siddhi-applications"></a>

Siddhi can run in multiple environments as follows.

* As a Java Library
* As a Local Microservice
* As a Docker Microservice
* As a Kubernetes Microservice
* As a Python Library _(WIP)_

### Siddhi Configurations <a href="#siddhi-configurations" id="siddhi-configurations"></a>

Refer the Siddhi Config Guide for information on advance Siddhi execution configurations.

### System Requirements <a href="#system-requirements" id="system-requirements"></a>

For all Siddhi execution modes following are the general system requirements.

1. **Memory** - 128 MB (minimum), 500 MB (recommended), higher memory might be needed based on in-memory data stored for processing
2. **Cores** - 2 cores (recommended), use lower number of cores after testing Siddhi Apps for performance
3. **JDK** - 8 or 11
4. To build Siddhi from the Source distribution, it is necessary that you have JDK version 8 or 11 and Maven 3.0.4 or later
