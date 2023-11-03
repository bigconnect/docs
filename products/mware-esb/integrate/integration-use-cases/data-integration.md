# Data Integration

Data integration is an important part of an integration process. For example, consider a typical integration process that is managed using the Micro Integrator: Data stored in various, disparate datasources are required in order to complete the integration use case.

The data services functionality that is embedded in the Micro Integrator can decouple the data from the datasource layer and exposing them as data services. The main integration flow defined in the Integrator will then have the capability of managing the data through the data service. Once the data service is defined, you can manipulate the data stored in the datasources by invoking the relevant operation defined in the data service. For example, you can perform the basic CRUD operations as well as other advanced operations.
