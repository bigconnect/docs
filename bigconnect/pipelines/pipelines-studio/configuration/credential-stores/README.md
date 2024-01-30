# Credential Stores

BigConnect Pipelines pipeline stages communicate with external systems to read and write data. Many of these external systems require sensitive information, such as user names or passwords, to access the data. When you configure pipeline stages for these external systems, you must specify the details that the stages need to connect to the system.

If you enter sensitive information directly in stage properties, you expose those details to any user with access to the pipeline. To access external systems without exposing the sensitive information, add them as secrets in a credential store and then use BigConnect Pipelines credential functions in the stage properties to retrieve those values.

Defining secrets in a credential store can make it easier to migrate pipelines to another environment. For example, if you migrate multiple pipelines from a development to a production environment, you do not need to edit each pipeline with details for the production environment. You can simply replace the development credential store with the production version.

You can configure BigConnect Pipelines to use multiple credential stores at the same time. Each credential store is identified by a unique credential store ID.

You can use the following credential stores with BigConnect Pipelines:

* AWS Secrets Manager
* CyberArk
* Hashicorp Vault
* Java keystore
* Microsoft Azure Key Vault

**Important**: Use the Java keystore credential store system in a development environment only. In a production environment, use a centralized keystore such as the other supported credential stores, to better secure sensitive information.
