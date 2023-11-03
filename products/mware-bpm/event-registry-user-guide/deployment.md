# Deployment

### Event Registry Definitions

For the Event Registry two types of definitions are supported, Event and Channel definitions. Event definitions have an .event extension and Channel definitions have a .channel extension.

When the Event Registry engine is used with the Process engine, the Event and Channel definitions can be packed into a business archive (BAR) together with other process related resources. The Process engine deployment service will take care of deploying the Event and Channel resources to the Event Registry engine.

> **Note**
>
> Java classes used for custom expression functions present in the business archive **will not be added to the classpath**. All custom classes used in expressions in event and channel definitions in the business archive should be present on the Flowable (Event Registry) Engine classpath.

#### Event Definitions <a href="#event-definitions" id="event-definitions"></a>

An Event definition configures the event payload structure and the correlation and tenant detection. When deploying an Event definition a new definition is inserted into the FLW\_EVENT\_DEFINITION table.

#### Deploying Event definitions programmatically <a href="#deploying-event-definitions-programmatically" id="deploying-event-definitions-programmatically"></a>

Deploying an Event definition can be done like this:

```
String eventDefinition = "path/to/definition-one.event"; //Don't forget the .event extension!

repositoryService.createDeployment()
    .name("Deployment of Event definition-one")
    .addClasspathResource(eventDefinition)
    .deploy();
```

You can use other methods to add the Event definitions to the deployment like `addInputStream`. This is an example to deploy an Event definition from an external file:

```
File eventFile = new File("/path/to/definition-two.event"); //Don't forget the .event extension!

repositoryService.createDeployment()
    .name("Deployment of Event definition-two")
    .addInputStream(eventFile.getName(), new FileInputStream(eventFile))
    .deploy();
```

The name of the deployment can be any text but the resource name must always contain a valid Event definition resource name suffix (".event").

#### Channel Definitions <a href="#channel-definitions" id="channel-definitions"></a>

A Channel definition configures the source or target destination for an incoming or outgoing event. By default the Flowable Event Registry supports JMS, Kafka and RabbitMQ source and target destinations, but this can be extended with other adapter types as well. When deploying a Channel definition a new definition is inserted into the FLW\_CHANNEL\_DEFINITION table.

#### Deploying Channel definitions programmatically <a href="#deploying-channel-definitions-programmatically" id="deploying-channel-definitions-programmatically"></a>

Deploying a Channel definition can be done like this:

```
String channelDefinition = "path/to/definition-one.channel"; //Don't forget the .channel extension!

repositoryService.createDeployment()
    .name("Deployment of Channel definition-one")
    .addClasspathResource(channelDefinition)
    .deploy();
```

You can use other methods to add the Event definitions to the deployment like `addInputStream`. This is an example to deploy an Event definition from an external file:

```
File channelFile = new File("/path/to/definition-two.channel"); //Don't forget the .channel extension!

repositoryService.createDeployment()
    .name("Deployment of Channel definition-two")
    .addInputStream(channelFile.getName(), new FileInputStream(channelFile))
    .deploy();
```

The name of the deployment can be any text but the resource name must always contain a valid Channel definition resource name suffix (".channel").

#### Java classes <a href="#java-classes" id="java-classes"></a>

All classes that contain the custom expression functions that are used in your Event and Channel should be present on the engine’s classpath when a decision is executed.

During deployment of an Event or Channel definition, however, these classes don’t have to be present on the classpath.

When you are using the demo setup and you want to add your custom classes, you should add a JAR containing your classes to the flowable-app-rest webapp lib. Don’t forget to include the dependencies of your custom classes (if any) as well. Alternatively, you can include your dependencies in the libraries directory of your Tomcat installation, ${tomcat.home}/lib.

#### Creating a single app <a href="#creating-a-single-app" id="creating-a-single-app"></a>

Instead of making sure that all Event Registry engines have all the delegation classes on their classpath and use the right Spring configuration, you may consider including the flowable-rest webapp inside your own webapp so that there is only a single EventRegistryEngine.

### Versioning of Event and Channel definitions <a href="#versioning-of-event-and-channel-definitions" id="versioning-of-event-and-channel-definitions"></a>

During deployment, Flowable will assign a version to the Event and Channel definitions before storing them in the Flowable DB.

For each Event and Channel definition, the following steps are performed to initialize the properties key, version, name and id:

* The event or channel key attribute in the definition JSON file is used as the definition key property.
* The name property in the JSON file is used as the definition name property.
* The first time an Event or Channel definition with a particular key is deployed, version 1 is assigned. For all subsequent deployments of Event or Channel definitions with the same key, the version will be set 1 higher than the maximum currently deployed version. The key property is used to distinguish Event and Channel definitions.
* The id property is a unique number to guarantee uniqueness of the Event and Channel definition identifier for the caches in a clustered environment.

Take for example the following process:

```
{
    "key": "myEvent",
    "name": "My event",
    ...
```

When deploying this Event definition, it will look like this:

| id                                   | key     | name     | version |
| ------------------------------------ | ------- | -------- | ------- |
| e29d4126-ed4d-11e6-9e00-7282cbd6ce64 | myEvent | My event | 1       |

Suppose we now deploy an updated version of the same Event definition, but the key of the Event definition remains the same. The Event definition table will now contain the following entries:

| id                                   | key     | name     | version |
| ------------------------------------ | ------- | -------- | ------- |
| e29d4126-ed4d-11e6-9e00-7282cbd6ce64 | myEvent | My event | 1       |
| e9c2a6c0-c085-11e6-9096-6ab56fad108a | myEvent | My event | 2       |

When the eventRegistry.getEventModelByKey("myEvent") is called, it will now use the Event definition with version 2, as this is the latest version of the Event definition.

If we create a second decision, as defined below and deploy this to Flowable DMN, a third row will be added to the table.

```
{
    "key": "myNewEvent",
    "name": "My new event",
    ...
```

The table will look like this:

| id                                   | key        | name         | version |
| ------------------------------------ | ---------- | ------------ | ------- |
| e29d4126-ed4d-11e6-9e00-7282cbd6ce64 | myEvent    | My event     | 1       |
| e9c2a6c0-c085-11e6-9096-6ab56fad108a | myEvent    | My event     | 2       |
| d317d3f7-e948-11e6-9ce6-b28c070b517d | myNewEvent | My new event | 1       |

Note how the key for the new Event definition is different from our first Event definition. Even if the name is the same, the Flowable Event Registry only considers the key attribute when distinguishing Event and Channel. The new Event definition is therefore deployed with version 1.
