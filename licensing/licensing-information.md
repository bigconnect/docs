# Licensing Information

## MWARE Portal

* Latest version: 7.4.3
* Deployment options: On-premise, Cloud
* Licensing: commercial, perpetual, on-premise or cloud

MWARE Portal is an enhanced commercial version of Liferay, meaning that all Liferay features are available in MWARE Portal. However, there are some notable changes that MWARE Portal has over the vanilla Liferay:

* A powerful new BPM engine with support for BPMN 2.0, CMMN, Forms, Decision Tables and Workflow Applications, Task List and much more
* Professional, pixel-perfect forms with avanced features like digial signatures, export/import PDF forms, 1D/2D barcodes, analytics and much more
* Privacy-preserving traffic monitoring and analytics
* Native Single Sign-On integration with MWARE Identity & Access Management
* Multi-factor authentication
* Seamless deployment and clustering with MWARE Application Server
* Performance tuning and optimizations that enable high concurrency and low latency in handling user requests
* Production binaries released with ahead-of-time (AoT) compilation. These binaries are smaller, start up to 100x faster, provide peak performance with no warmup, and use less memory and CPU than applications running on a Java Virtual Machine (JVM).
* G1 (Garbage-First) garbage collector as the default JVM garbage collector
* Generative AI integration with our state-of-the-art Romanian LLM
* Professional support

**Restricted Use Licenses**

| Feature                  | Licensing Information                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MWARE BPM                | <p>A restricted use license* for MWARE BPM is included with MWARE Portal, free of charge, for the purpose of creating custom workflows for your organization using the BPMN, CMMN and DMN standards.<br><br>MWARE BPM may be used only together with MWARE Portal or by applications that are integrated in the portal. Using the restricted use product outside the fully licensed product is prohibited.</p> |
| MWARE Application Server | A restricted use license\* for MWARE Application Server is included with MWARE Portal, free of charge, for the purpose of deploying and running MWARE Portal or applications that are integrated in the portal.                                                                                                                                                                                                |

**Open Source Licensed Software included in MWARE Portal:**

| Provider | Component      | License |
| -------- | -------------- | ------- |
| Liferay  | Liferay Portal | LGPLv2  |

\*) A _restricted use license_ limits the usage of the restricted use product only in conjuction with the fully licensed product.&#x20;

## MWARE BPM

* Latest version: 6.8.1&#x20;
* Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

MWARE BPM is a commercial-grade, light-weight business process engine written in Java. The BPM process engine allows you to deploy BPMN 2.0 process definitions (an industry XML standard for defining processes), creating process instances of those process definitions, running queries, accessing active or historical process instances and related data, plus much more.

MWARE BPM started as a fork of Activiti, an open source BPM engine. Since then, it has diverged significantly from Activiti and now includes additional features and enhancements:

* Support for Case Management Model and Notation (CMMN) implementation
* Seamless integration with event streaming platforms
* Direct execution of BPMN models without intermediary steps
* Capability to inject processes dynamically at runtime
* Asynchronous processing of historical data
* Utilization of abstract data source interfaces
* Implementation of genuine parallel gateway functionality

**Open Source Licensed Software included in MWARE BPM:**

| Provider | Component | License    |
| -------- | --------- | ---------- |
| Alfresco | Activiti  | Apache 2.0 |

## MWARE IAM

* Latest version: 7.0&#x20;
* Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

MWARE IAM is a fork of WSO2 Identity Server with the following additional features:

* Native Single Sign-On (SSO) integration with MWARE Portal, MWARE BPM, MWARE Application Server and the BigConnect platform
* Performance tuning and optimizations that enable high concurrency and low latency in handling user requests
* Professional support

**Open Source Licensed Software included in MWARE Portal:**

| Provider | Component            | License    |
| -------- | -------------------- | ---------- |
| WSO2     | WSO2 Identity Server | Apache 2.0 |

## MWARE Application Server

* Latest version: 6.8
* &#x20;Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

MWARE Application Server an application platform for mission-critical Jakarta EE production systems. It started as a fork of Eclipse GlassFish, but has diverged significantly from the original GlassFish, by adding significant improvements in the developer experience and performance:

* Enhanced reporting and admin functions
* Optimised for use in production
* Security, monitoring and DevOps features like slow SQL Logging, healthchecks, asadmin recorder etc.
* Support for enterprise-grade cluster topologies through the use of Hazelcast
* Support for container-based deployments
* Up-to-date Java and Jakarta EE standards
* The included MWARE Web Server component offers native Apache HTTP Server integration with support for HTTP/2, HTTP/3, WebSockets, load-balancing, virtual servers and more.
* Professional support

**Open Source Licensed Software included in MWARE Application Server:**

| Provider | Component          | License                        |
| -------- | ------------------ | ------------------------------ |
| Eclipse  | Glassfish          | Eclipse Public License - v 2.0 |
| Apache   | Apache HTTP Server | Apache 2.0                     |

## MWARE ESB

* Latest version: 5.3
* &#x20;Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

MWARE ESB is an enterprise API management solution built on the WSO2 API Manager platform. It supports API designing, API publishing, lifecycle management, application development, API security, rate limiting, viewing statistics of APIs, as well as connecting APIs, API Products, and endpoints.

Compares to the WSO2 API Manager, MWARE ESB includes additional features and enhancements:

* A new user interface with a modern and responsive design
* Self-service portal for developers to register applications and APIs
* API Marketplace for publishing APIs to a wider audience
* Visual Studio Code plugin for rapid API development with Generative AI capabilities
* Professional support
* Native integration with MWARE IAM

**Open Source Licensed Software included in MWARE ESB:**

| Provider | Component   | License    |
| -------- | ----------- | ---------- |
| WSO2     | API Manager | Apache 2.0 |

## BigConnect Core

* Latest version: 4.2.2
* &#x20;Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

BigConnect Core is a massively scalable Data Lake designed for storing and querying vast amounts of raw data. It can store raw data, objects and links with multi-valued properties, while providing fast queries, aggregations, distributed data processing pipelines and super-strong security (row and property-level).

Built from scratch by MWARE to serve mission-critical use cases, BigConnect Core is the key component of the BigConnect platform.

**Open Source Licensed Software included in BigConnect Core:**

| Provider   | Component  | License    |
| ---------- | ---------- | ---------- |
| Apache     | HDFS       | Apache 2.0 |
| Apache     | Accumulo   | Apache 2.0 |
| Apache     | Ignite     | Apache 2.0 |
| Apache     | Kafka      | Apache 2.0 |
| Apache     | Spark      | Apache 2.0 |
| Facebook   | RocksDB    | GPL 2.0    |
| Trino      | Trino      | Apache 2.0 |
| OpenSearch | OpenSearch | Apache 2.0 |
| Neo4j      | Browser    | GPL-3.0    |

## BigConnect Answers

* Latest version: 5.24&#x20;
* Deployment options: On-premise, Cloud&#x20;
* Licensing: commercial, perpetual, on-premise or cloud

BigConnect Answers is a data analytics platform built on top of Metabase. It allows you to connect to BigConnect Core and other data sources, query the data, and visualize the results.

Built onn top of Metabase, BigConnect Answers includes additional features and enhancements:

* Integration with BigConnect Core for querying lake data
* Integration with MWARE IAM for authentication using SSO
* Support for embedding charts and dashboards in MWARE Portal
* Support for more data sources: Trino, Cube.js, Dremio, DB2, Impala, Spark SQL, Teradata

**Open Source Licensed Software included in BigConnect Core:**

| Provider | Component | License  |
| -------- | --------- | -------- |
| Metabase | Metabase  | AGPL 2.0 |

## BigConnect Explorer

* Latest version: 5.24
* &#x20;Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

BigConnect Explorer is a complete visual environment best suited for exploring and making sense of **unstructured data** store in BigConnect Core It can do a lot: data ingestion, data enrichment, discovery and analysis.

It is completely extensible and can be extended with plugins to suit any use case, using common development languages like Java, JavaScript, React, CSS/LESS etc.

**Open Source Licensed Software included in BigConnect Core:**

NONE

## BigConnect Sponge

* Latest version: 2.21
* &#x20;Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

Sponge helps you transform the limitless amount of unstructured content into valuable information that can be used everywhere. Whether is about web sites, blogs or forums, Sponge is a full-featured, flexible and extensible web crawler that runs on any platform and will help you crawl what you want, how you want.

**Open Source Licensed Software included in BigConnect Core:**

NONE

## BigConnect VM

* Latest version: 2.21
* &#x20;Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

BigConnect VM is a fork of the popular Portainer platform with the following additions:

* Better support for Docker Swarm clusters
* Native integration with BigConnect Core for status monitoring and logging
* Professional support

**Open Source Licensed Software included in BigConnect Core:**

| Provider  | Component | License      |
| --------- | --------- | ------------ |
| Portainer | Portainer | zlib license |

## BigConnect Pipelines

* Latest version: 4.22
* &#x20;Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

BigConnect Pipelines started as an early fork of the Streamsets Data Collector platform, but has since diverged significantly from the original project:

* A new user interface with a modern and responsive design
* Support for BigConnect Core services and data sources
* Pipeline Controller for orchestrating pipelines across multiple machines
* Support for clustered deployment topologies

**Open Source Licensed Software included in BigConnect Core:**

| Provider   | Component      | License    |
| ---------- | -------------- | ---------- |
| Streamsets | Data Collector | Apache 2.0 |

## BigConnect TetraML

* Latest version: 4.3
* &#x20;Deployment options: On-premise, Cloud
* &#x20;Licensing: commercial, perpetual, on-premise or cloud

BigConnect TetraML is an integrated development environment for AI and Machine Learning. It allows you to ;label, build, train, and deploy models using the most popular frameworks and libraries:

* Development environment build on Jupyter Lab
* Integration with Label Studio for data labeling
* Support for popular frameworks and libraries like H2O, Spark MLib, XGBoost, PyTorch, TensorFlow, Scikit-Learn, MLflow, and more
* Native integration with BigConnect Core for storing and querying data
* A collection of pre-built AI models for common use-cases: Named Entity Recognition (NER), Sentiment Analysis, Text Classification, Face detection, License Plate Recognition (LPR), Optical Character Recognition (OCR), and more
* Support for Generative AI frameworks and models like Llama, Mistral, GPT4, Claude, Gemini, and more

**Open Source Licensed Software included in BigConnect Core:**

| Provider    | Component    | License    |
| ----------- | ------------ | ---------- |
| Jupyter     | Jupyter Lab  | BSD        |
| H2O         | H2O          | Apache 2.0 |
| Apache      | Spark Mlib   | Apache 2.0 |
| Apache      | MLFlow       | Apache 2.0 |
| HumanSignal | Label Studio | Apache 2.0 |

## Written Offer for Source Code

For third party technology that you receive from MWARE SOLUTIONS SA in binary form which is licensed under an open source license that gives you the right to receive the source code for that binary, you can obtain a copy of the applicable source code by sending a request to us.

If the source code for the technology was not provided to you with the binary, you can also receive a copy of the source code on physical media by submitting a written request to:

MWARE SOLUTIONS SA Attn: Licensing Floreasca Park, sos. Pipera 43, building B 014254, Sector 1, Bucharest, Romania
