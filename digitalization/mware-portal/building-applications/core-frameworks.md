# Core Frameworks

## Configuration Framework

You can use Liferay’s configuration framework to add a settings UI for a MVC Portlet.

### Creating the Configuration Interface <a href="#creating-the-configuration-interface" id="creating-the-configuration-interface"></a>

Defining configurable attributes in a configuration interface is enough to generate a configuration UI in System Settings.

In the sample project, the `N2F3WebConfiguration.java` file is the configuration interface.

```java
@ExtendedObjectClassDefinition(
	category = "n2f3", scope = ExtendedObjectClassDefinition.Scope.COMPANY
)
@Meta.OCD(
	id = "com.acme.n2f3.web.internal.configuration.N2F3WebConfiguration",
	localization = "content/Language", name = "n2f3-configuration-name"
)
public interface N2F3WebConfiguration {

	@Meta.AD(deflt = "blue", required = false)
	public String fontColor();

	@Meta.AD(deflt = "serif", required = false)
	public String fontFamily();

	@Meta.AD(deflt = "16", required = false)
	public int fontSize();

}
```

Note, for this example interface the scope is set to `Scope.COMPANY`. See Scoping Configurations for more information.

The interface has three configurable attributes: font color, font family, and font size. Note that color and family are type `string` and size is type `int`.

`Meta.OCD` registers this class as a configuration with a specific ID.

IMPORTANT

Note that the ID must be the fully qualified class name (FQCN) of the configuration interface.

`Meta.AD` specifies [optional metadata](http://bnd.bndtools.org/chapters/210-metatype.html) about the attribute such as a default value or whether the attribute is a required field. Note that if an attribute value is required but a default is not set, an administrator must set a value in settings for the application to work properly.

Next, see how the configuration is read by the MVC Portlet.

### Reading the Configuration from the Application <a href="#reading-the-configuration-from-the-application" id="reading-the-configuration-from-the-application"></a>

1.  In the `@Component` annotation, the configuration interface class is specified with the `configurationPid`:

    ```java
    configurationPid = "com.acme.n2f3.web.internal.configuration.N2F3WebConfiguration"
    ```
2. To access the configuration, the `render()` method utilizes a `ConfigurationProvider`. The Configuration Provider API provides methods to retrieve a configuration at different levels of scope. The sample project’s configuration is instance scoped and uses the `getCompanyConfiguration()` method to retrieve the configuration.

```java
try {
	renderRequest.setAttribute(
		N2F3WebConfiguration.class.getName(),
		_configurationProvider.getCompanyConfiguration(
			N2F3WebConfiguration.class,
			_portal.getCompanyId(renderRequest)));
}
```

```log
The configuration object is added to the request object and can now be read from the request of the application's JSP.
```

### Accessing the Configuration from a JSP <a href="#accessing-the-configuration-from-a-jsp" id="accessing-the-configuration-from-a-jsp"></a>

1.  The following import statement adds the configuration interface to the JSP:

    ```markup
    <%@ page import="com.acme.n2f3.web.internal.configuration.N2F3WebConfiguration" %>
    ```
2.  The configuration object from the request object is obtained and the configuration values are read.

    ```markup
    <%
    N2F3WebConfiguration n2f3WebConfiguration = (N2F3WebConfiguration)request.getAttribute(N2F3WebConfiguration.class.getName());
    %>
    ```
3. The attributes `fontColor()`, `fontFamily()`, `fontSize()` can now be used in the JSP.

### Implementing a Dropdown Selection UI <a href="#implementing-a-dropdown-selection-ui" id="implementing-a-dropdown-selection-ui"></a>

The sample project has three attributes that can be configured. Currently the attributes must be manually entered into a text input field, but this can be further customized.

For example, you can use a dropdown list for the font family attribute instead of an input field. In the project’s configuration interface, replace the `@Meta.AD` annotation with this:

```java
@Meta.AD(
	optionLabels = {"Arial", "Georgia", "Helvetica", "Tahoma", "Verdana"},
	optionValues = {"arial", "georgia", "helvetica", "tahoma", "verdana"},
required = false)
```

Redeploy the example module.

![The font family is now a dropdown selection.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/core-frameworks/configuration-framework/setting-and-accessing-configurations/images/03.png)

Now the font family attribute is a dropdown selection.

### ConfigurationBeanDeclaration with Previous Versions of Liferay <a href="#configurationbeandeclaration-with-previous-versions-of-liferay" id="configurationbeandeclaration-with-previous-versions-of-liferay"></a>

IMPORTANT

For version 7.4 +, a `ConfigurationBeanDeclaration` class is not required. The configration interface is registered with the Configuration Provider API automatically.

On Liferay versions before 7.4 Update/GA 51, the configuration class must be registered with a `ConfigurationBeanDeclaration` to use it with the Configuration Provider API. The `ConfigurationBeanDeclaration` class has one method that returns the configuration interface class. This helps the system keep track of configuration changes as they happen. For example, for the N2F3 portlet, create a class like this:

```java
@Component(service = ConfigurationBeanDeclaration.class)
public class N2F3WebConfigurationBeanDeclaration
	implements ConfigurationBeanDeclaration {

	@Override
	public Class<?> getConfigurationBeanClass() {
		return N2F3WebConfiguration.class;
	}

}
```

In this example, place the class in the `com.acme.n2f3.web.internal.settings.definition` package.

## Dependency Injection

When you’re using a object based on its interface, you don’t have to concern yourself with the implementation because it’s abstracted from you. At runtime, the implementation used depends on your environment and your app’s configuration. The portal offers several standard ways to register implementations and inject them into your applications.

**Contexts and Dependency Injection (CDI):** Is the Java EE standard dependency injection mechanism. The portal’s CDI bean container makes an application’s concrete classes available as beans. Bean classes can user other beans by way of injecting them into their fields that have the `@Inject` annotation.

**OSGi Declarative Services:** The portal’s OSGi runtime framework allows components to register as service provides and other components can call on the registry to bind the services to their fields that have the `@Reference` annotation. The portal’s services and services you create using Service Builder are available as OSGi Declarative Services.

**Spring DI:** The Spring framework includes inversion of control (IoC) and dependency injection. It’s available to applications that configure the Spring framework.

As an added bonus, the portal provides OSGi CDI integration. It lets you publish CDI beans as OSGi services and consume OSGi services in your CDI beans. Which dependency injection mechanism will you use? Read on to learn more about them.

## Localization <a href="#localization" id="localization"></a>

Liferay currently supports about 50 languages out-of-the-box. Translation is complete for many of these languages and some are still in the translation process. Each language has its own language properties file containing keys for its language. But there are more than 6000 languages in the world. If you must add an additional language, here’s how.

### Adding A New Language <a href="#adding-a-new-language" id="adding-a-new-language"></a>

By default, the `portal.properties` file specifies all languages available in Liferay. To add a new language that is not specified,

1. Take note of the [ISO 639-1 standard code](http://www.loc.gov/standards/iso639-2/php/code\_list.php) for your language (e.g. the ISO code for Amharic would be `am`).
2. Take note of the [ISO 3166-1 alpha-2 two-letter country code](https://www.iso.org/iso-3166-country-codes.html) for your language. (e.g. the ISO code for Ethiopia would be `ET`).
3.  Modify and replace the `web.xml` file found in your Liferay installation: `[HOME]/tomcat<version>/webapps/ROOT/WEB-INF/`. Add your new language under the `<web-resource-name`> section. For example,

    ```xml
    	<url-pattern>/am/c/portal/protected</url-pattern>
    	<url-pattern>/am-ET/c/portal/protected</url-pattern>
    	<url-pattern>/am_ET/c/portal/protected</url-pattern>
    ```
4.  Modify and replace the `shielded-container-web.xml` file found in your Liferay installation: `[HOME]/tomcat<version>/webapps/ROOT/WEB-INF/`. Add your new language under the `<servlet-mapping`> section. For example,

    ```xml
    	<servlet-mapping>
    		<servlet-name>I18n Servlet</servlet-name>
    		<url-pattern>/am/*</url-pattern>
    	</servlet-mapping>
    	<servlet-mapping>
    		<servlet-name>I18n Servlet</servlet-name>
    		<url-pattern>/am-ET/*</url-pattern>
    	</servlet-mapping>
    	<servlet-mapping>
    		<servlet-name>I18n Servlet</servlet-name>
    		<url-pattern>/am_ET/*</url-pattern>
    	</servlet-mapping>
    ```
5.  Create your `language.properties` file for your language and use the ISO code in your file name. For example `Language_am.properties` for Amharic:

    ```properties
    create-account=መለያ መፍጠር
    email-address=የ ኢሜል አድራሻ
    forgot-password=መክፈቻ ቁልፉን ረሳኽው
    home=መነሻ ገጽ
    password=ፕስወርድ
    powered-by-x=በ {0} የተጎላበተ
    remember-me=አስታወስከኝ
    search=የፍለጋ አሞሌ
    sign-in=ስግን እን
    ```
6. Create a language module with the `language.properties` file you created and deploy the module to your portal installation. See Overriding Global Language Translations to see a sample project and specific instructions on creating this module.
7.  Create or modify your `portal-ext.properties` file in your installation’s Home folder. Note, this file overrides the enabled default locales defined in the `portal.properties` file. Therefore list all the languages you plan to use in the `portal-ext.properties` file. Define both `locales` and `locales.enabled`. For example:

    ```properties
    locales=am_ET,ar_SA,fa_IR,en_US,zh_CN,ja_JP
    locales.enabled=am_ET,ar_SA,fa_IR,en_US,zh_CN,ja_JP
    ```
8.  Once the module is deployed, navigate to _Control Panel_ → _Instance Settings_ → _Localization_. Verify and move the new language to the current languages. Click _Save_.

    ![Save your new language to the list of current languages.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/core-frameworks/localization/adding-a-language/images/01.png)
9.  Change your default language to the new language and click _Save_. The new language is now used by the portal instance.

    ![Select and use the new language for your Liferay instance.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/core-frameworks/localization/adding-a-language/images/02.png)



## Message Bus <a href="#message-bus" id="message-bus"></a>

The Message Bus provides a loosely coupled way to exchange messages. A class sending a message invokes the Message Bus to send the message to a destination, while other classes (_listeners_) registered at that destination receive the message. The listeners are transparent to senders and vice-versa.

Here are the main messaging components:

**Destinations:** Logical (not physical), named locations where messages are sent.

**Listeners:** Classes that _listen_ for messages sent to specific destinations.

**Message Bus:** The framework that handles destination registration and sends messages to listeners.

**Messages:** Objects that can contain a payload and metadata, including an optional response destination.

**Senders:** Arbitrary classes that invoke Message Bus to send messages to a destination’s listeners.

The figure below demonstrates component interaction.

![Example Messaging Component Interaction](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/core-frameworks/message-bus/images/01.png)

Here is an example interaction sequence:

1. An arbitrary class sends a `Message` to a registered `Destination`.
2. Message Bus dispatches the `Message` to the destionation’s registered `MessageListener`s.

The message sender is only concerned with using Message Bus to send the message to the destination; it is not concerned with the message recipients. The message listeners are only concerned with receiving messages at the destination; they are not concerned with the message sender.

### Synchronous and Asynchronous Messaging <a href="#synchronous-and-asynchronous-messaging" id="synchronous-and-asynchronous-messaging"></a>

Message Bus sends messages synchronously and asynchronously.

IMPORTANT

Synchronous messaging was removed and is no longer supported for version 7.4 and above.

**Synchronous Messaging:** The sender blocks after sending a message. When the sender’s condition (determined by the sender type) is met, the sender unblocks and continues processing.

**Asynchronous Messaging:** The sender immediately continues processing after sending a message.

The following topics cover both ways of messaging:

* Using Asynchronous Messaging shows how to configure a destination and send messages asynchronously to it. This is the simplest way to send messages.
* Using Default Synchronous Messaging in Previous Versions demonstrates a message sender blocking until one listener responds to the message _or_, if no listener responds, until the message times out.
* Using Direct Synchronous Messaging in Previous Versions shows a message sender blocking on sending a message until _all_ listeners receive the message.

## Portlet Providers <a href="#portlet-providers" id="portlet-providers"></a>

Some apps perform the same operations on different entity types. For example, the Asset Publisher lets users browse, add, preview, and view various entities as assets including documents, web content, blogs, and more. The entities vary, but the operations and surrounding business logic stay the same. Apps like the Asset Publisher rely on the Portlet Providers framework to fetch portlets to operate on the entities. In this way, the framework lets you focus on entity operations and frees you from concern about portlets that carry out those operations.

#### PORTLET PROVIDER CLASSES <a href="#portlet-provider-classes" id="portlet-provider-classes"></a>

Portlet Provider classes are components that implement the `PortletProvider` interface, and are associated with an entity type. Once you’ve registered a Portlet Provider, you can invoke the `PortletProviderUtil` class to retrieve the portlet ID or portlet URL from that Portlet Provider.

As an example, examine the `WikiEditPortletProvider` class:

```java
@Component(
    immediate = true,
    property = {
        "model.class.name=com.liferay.wiki.model.WikiPage",
        "service.ranking:Integer=100"
    },
    service = EditPortletProvider.class
)
public class WikiEditPortletProvider
    extends BasePortletProvider implements EditPortletProvider {

    @Override
    public String getPortletName() {
        return WikiPortletKeys.WIKI;
    }

}
```

`WikiEditPortletProvider` extends `BasePortletProvider`, inheriting its `getPortletURL` methods. `WikiEditPortletProvider` must, however, implement `PortletProvider`’s `getPortletName` method, which returns the portlet’s name `WikiPortletKeys.WIKI`.

**Note:** If you’re creating a Portlet Provider for one of Liferay’s portlets, your `getPortletName` method should return the portlet name from that portlet’s `*PortletKeys` class, if such a class exists.

The `@Component` annotation for `WikiEditPortletProvider` specifies these elements and properties:

* `immediate = true` activates the component immediately upon installation.
* `"model.class.name=com.liferay.wiki.model.WikiPage"` specifies the entity type the portlet operates on.
* `"service.ranking:Integer=100"` sets the component’s rank to `100`, prioritizing it above all Portlet Providers that specify the same `model.class.name` value but have a lower rank.
* `service = EditPortletProvider.class` reflects the subinterface `PortletProvider` class this class implements (`EditPortletProvider`).

## Servlets <a href="#servlets" id="servlets"></a>

Java Servlets are foundational to Java EE. You can use servlets and servlet filters to provide applications in your portal context and to process requests and responses to specific URLs in your sites.&#x20;

You can use servlets or JAX-RS to provide a lightweight web integration or a web endpoint to a browser client. Servlets, rather than REST endpoints or portlets, let you control an application’s entire UI experience. the portal supports servlet based applications and embeds [HTTP Whiteboard](https://osgi.org/specification/osgi.cmpn/7.0.0/service.http.whiteboard.html) for servlets.

Here’s how to create your own servlet:

1. Create a module project.
2.  Add the necessary dependencies. Here they are for Gradle:

    ```
    compileOnly group: "javax.servlet", name: "javax.servlet-api", version: "3.0.1"
    compileOnly group: "org.osgi", name: "org.osgi.service.component.annotations", version: "1.4.0"
    compileOnly group: "org.osgi", name: "org.osgi.service.log", version: "1.4.0"
    ```
3. Create a servlet class that extends `javax.servlet.http.HttpServlet`.
4.  Add the following `@Component` annotation.

    ```
    @Component(
        property = {
            "osgi.http.whiteboard.context.path=/",
            "osgi.http.whiteboard.servlet.pattern=/blade/servlet/*"
        },
        service = Servlet.class
    ) 
    ```

    `service = Servlet.class`: Makes the component an OSGi service of type `Servlet`.
5.  Set the following `@Component` `property` values to specify a context path and servlet URL pattern:

    `"osgi.http.whiteboard.context.path=/"`: Sets the servlet’s context. Replace the value with your servlet’s context path.

    `"osgi.http.whiteboard.servlet.pattern=/blade/servlet/*"`: Sets the servlet’s mapping pattern. Replace the value with your servlet’s pattern.
6. Override `HttpServlet` methods to implement your servlet’s behavior.
7. Deploy your module.

Your servlet is up and running. You’re well on your way to delivering custom user experiences using servlets.

\
