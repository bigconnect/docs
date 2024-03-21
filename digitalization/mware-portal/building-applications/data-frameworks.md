# Data Frameworks

## Asset Framework <a href="#asset-framework" id="asset-framework"></a>

The asset framework is behind many of the portal’s most powerful features. It provides tools for displaying and interacting with any type of content and data. For example, if you build an event management application that displays a list of upcoming events, you can use the asset framework to let users add tags, categories, or comments to make entries more descriptive. Using the asset framework is also the first step for integrating other important frameworks such as segmentation or workflow.

The term _asset_ refers to any type of content: text, a file, a URL, an image, a document, a blog entry, a bookmark, a wiki page, or anything you create in your application.

The asset framework tutorials assume that you are already familiar with using the portal’s Service Builder. Follow the links below to learn how to enable assets and to enhance the implementation of assets.

## Cache <a href="#cache" id="cache"></a>

Caching makes specified data readily available in memory. It costs memory but improves performance. You can experiment with cache to determine what’s good for your system. If your site serves lots of web content articles, for example, you may want to increase the limit on how many you can cache.

The portal’s cache configuration framework uses [Ehcache](https://www.ehcache.org/). It’s an independent framework used by the portal’s data access and template engine components. It manages two pools:

**Multi-VM:** Cache is replicated among cluster nodes. `EntityCache` and `FinderCache` (described next) are in this pool because they must synchronize with data on all nodes.

**Single-VM:** Cache is managed uniquely per VM and isn’t replicated among nodes. Single-VM cache is for objects and references that you don’t need/want replicated among nodes.

## Data Scopes <a href="#data-scopes" id="data-scopes"></a>

Apps can restrict their data to specific _scopes_. Scopes provide a context for the application’s data.

**Global:** One data set throughout a portal instance.

**Site:** One data set for each Site.

**Page:** One data set for each Page on a Site.

For example, a Site-scoped app has one set of data on one Site and a completely different set of data for another Site. For a detailed explanation of scopes, see the user guide article [Widget Scope](https://help.liferay.com/hc/en-us/articles/360028819992-Widget-Scope). To give your applications scope, you must manually add support for it. For instructions on this, see Enabling and Accessing Data Scopes.

#### ACCESSING THE SITE SCOPE ACROSS APPS <a href="#accessing-the-site-scope-across-apps" id="accessing-the-site-scope-across-apps"></a>

There may be times when you must access a different app’s Site-scoped data from your app that is scoped to a page or the portal. For example, web content articles can be created in the page, Site, or portal scope. Structures and Templates for such articles, however, exist only in the Site scope. Other techniques return your app’s scope, which might not be the Site scope. What a pickle! Never fear, the `ThemeDisplay` method `getSiteGroupId()` is here! This method always gets the Site scope, no matter your app’s current scope. For an example of using this method, see Enabling and Accessing Data Scopes.

## Expandos

When you need additional fields in your application, you can always add them in your service model definition and re-run Service Builder. This adds new columns to your database table. This works, but can be a pain: now you have to write an upgrade process to migrate your users to the new schema. But with the portal’s Expando feature, you can add additional fields without having to modify your database. Expandos work by adding additional properties to an object in Java.&#x20;

## Service Builder <a href="#service-builder" id="service-builder"></a>

An application without reliable business logic or persistence isn’t much of an application at all. Unfortunately, writing your own persistence code often takes a great deal of time. Service Builder is an object-relational mapping tool that can generate your model, persistence, and service layers from a single `xml` file. Once generated, the code is completely customizable: you can write your own persistence code along with custom SQL if necessary. Regardless of how you produce your persistence code, you can then use Service Builder to implement your app’s business logic.

Here, you’ll learn how to use Service Builder to

* Generate and customize your persistence framework
* Implement your business logic

When you configure your model and its relationships in your `service.xml` file and run Service Builder, it generates these layers of code:

* **Model layer:** defines objects to represent your project’s entities.
* **Persistence layer:** saves entities to and retrieves entities from the database.
* **Service layer:** a blank layer ready for you to create your API and business logic.

Here are some key features:

* Stubbed-out classes for implementing custom business logic
* Hibernate configurations
* Configurable caching support
* Flexibility and support for adding custom SQL queries and dynamic queries

NOTE

You don’t have to use Service Builder for your back-end services on the portal. It’s entirely possible to use your persistence framework of choice, such as JPA or Hibernate. Note that under the hood, Service Builder uses [Hibernate](http://hibernate.org/).

### Customization via Implementation Classes <a href="#customization-via-implementation-classes" id="customization-via-implementation-classes"></a>

Service Builder-generated entities contain these _implementation_ classes:

* **Entity implementation** (`*Impl.java`): Responsible for customizing the entity.
* **Local service implementation** (`*LocalServiceImpl.java`): Responsible for calling the persistence layer to retrieve and store data entities. Local services contain the business logic and access the persistence layer. They can be invoked by client code running in the same Java Virtual Machine.
* **Remote service implementation** (`*ServiceImpl.java`): Generated if the `service.xml` is configured for remote services. Remote services usually contain permission checking code and are meant to be accessible from outside the JVM. Service Builder automatically generates code that makes the remote services available via JSON or SOAP, and you can also create your own remote APIs through REST Builder or JAX-RS.

These classes are where you implement custom business logic. They’re the only classes generated by Service Builder intended for customization.

### Hibernate Configurations <a href="#hibernate-configurations" id="hibernate-configurations"></a>

Service Builder uses the Hibernate persistence framework for object-relational mapping. Service Builder hides the complexities of Hibernate, while still giving you access to technology like dynamic queries and custom SQL. You can take advantage of Object-Relational Mapping (ORM) in your projects without having to set up a Hibernate environment manually or make any configurations.

### Caching <a href="#caching" id="caching"></a>

Service Builder caches objects at three levels: _entity_, _finder_, and _Hibernate_. By default, the portal uses Ehcache as an underlying cache provider for each of these cache levels. However, this is configurable via portal properties. All you must do to enable entity and finder caching in your project is to set the `cache-enabled=true` attribute of your entity’s `<entity>` element in your `service.xml` file. Clustering describes the portal caching in a cluster.

### Dynamic Query and Custom SQL Query <a href="#dynamic-query-and-custom-sql-query" id="dynamic-query-and-custom-sql-query"></a>

Service Builder automates many of the common tasks associated with creating database persistence code, but it doesn’t prevent you from creating custom SQL queries. You can define custom SQL queries in an XML file and implement finder methods to run the queries. If you have some crazy join to do, Service Builder gets out of your way. You can also use dynamic query to access Hibernate’s criteria API.

\




