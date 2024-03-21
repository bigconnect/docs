# Configuring Logging

In the Server Administration panel’s Log Levels tab, you can set logging levels for classes and packages in the portal’s class hierarchy and add custom objects to be similarly managed using the Server Administration logging UI. Changes to the log level near the top of the class hierarchy (such as at `com.liferay`) also change log levels for all the classes under that hierarchy. Modifications unnecessarily high in the hierarchy generate too many messages to be useful.

TIP

If your changes produce more log messages, make them as specific as possible.

Each logging configuration is referred to as a Category.

### Adding a Class or Package to the Log Levels User Interface <a href="#adding-a-class-or-package-to-the-log-levels-user-interface" id="adding-a-class-or-package-to-the-log-levels-user-interface"></a>

To add a category,

1. Navigate to Control Panel → Configuration → Server Administration, and click the _Log Levels_ tab.
2. Open the Add Category form by clicking the Add button ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png).
3. Fill out the form and click _Save_.
4. Click _Save_.

_Logger Name_: A class deployed on the server, such as `com.liferay.portal.workflow.kaleo.runtime.internal.notification.TemplateNotificationMessageGenerator`

_Log Level_: Choose the logging level you’d like to see: OFF, FATAL, ERROR, WARN, INFO, DEBUG, TRACE, or ALL.

![You can add your own deployed classes to the logging level categories.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/using-the-server-administration-panel/configuring-logging/images/01.png)

For a description of the logging levels, refer to [Apache’s Log4j `Level` class Javadoc](https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/Level.html).

### Changing the Log Level of an Existing Category <a href="#changing-the-log-level-of-an-existing-category" id="changing-the-log-level-of-an-existing-category"></a>

To change the log level of an existing logging configuration,

1. Navigate to Control Panel → Configuration → Server Administration, and click the _Log Levels_ tab.
2. Browse to or search for the category to configure.
3. Use the selector to change the level.
4. Click _Save_.

![To debug possible issues, you can change the logging level of any class that can be logged.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/using-the-server-administration-panel/configuring-logging/images/02.png)

### Adjusting Levels with XML Files <a href="#adjusting-levels-with-xml-files" id="adjusting-levels-with-xml-files"></a>

To adjust the log levels with an XML file,

1. Note the symbolic bundle name of the module (e.g. `com.liferay.saml.opensaml.integration`).
2. In your `[LIFERAY_HOME]/osgi/log4j` folder, create an XML file with the following format `[symbolicBundleName]-log4j-ext.xml`. For example `com.liferay.saml.opensaml.integration-log4j-ext.xml`.
3. Define the logger level in the XML file. For example,

```xml
   <?xml version="1.0"?>
   <Configuration strict="true">
    <Loggers>
        <Logger level="DEBUG" name="com.liferay.saml.opensaml.integration" />
    </Loggers>
   </Configuration>
```

Note, the configuration is in the XML Schema Definition (XSD) format and not in Document Type Definition (DTD) format.
