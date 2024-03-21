# Configuration Files and Factories

## Using Configuration Files <a href="#using-configuration-files" id="using-configuration-files"></a>

System Settings provides a user interface for making system-scoped configuration changes and setting default configurations for other scopes. The same configurations in the UI can be made via configuration file.

You can use configuration files to transfer configurations from pre-production systems to production systems or between any other portal systems, as long as the version is identical.

Configuration files use the `.config` property value format defined by the Apache Felix Configuration Admin framework.

NOTE

The `.cfg` file format is common in OSGi environments, and it’s a supported format, but `.config` files are preferred because you can both specify a property value’s type, and allow multi-valued properties. For this reason, `.config` files are used, recommended, and documented as a best practice.

### Creating Configuration Files <a href="#creating-configuration-files" id="creating-configuration-files"></a>

System Settings provides an _Export_ option that becomes available once you modify a configuration entry. Exporting is the best way to create `.config` files: you download a `.config` file containing the entry’s settings in a `key=value` format, then modify or distribute as you have need. All available configuration keys and values are exported, even if only one value was changed.

You can export a single configuration entry or the entire set of modified configurations.

Configuration files are named after the backing Java class. For example, the Journal Service entry, which backs Web Content functionality, has this file name:

```bash
com.liferay.journal.configuration.JournalServiceConfiguration.config
```

![The Web Content System Settings entry has the back-end ID com.liferay.journal.configuration.JournalServiceConfiguration.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/configuration-files-and-factories/using-configuration-files/images/01.png)

The system enforces the configuration file’s name. If you edit the name, the link to the configuration entry is broken and further customization never takes effect. The only exception is for Factory Configuration, where a unique subname can be provided in some cases.

#### Enabling Password Export <a href="#enabling-password-export" id="enabling-password-export"></a>

By default, passwords are excluded when exporting configuration files (e.g. Elasticsearch password, Salesforce password, etc.). To include them in exports, add the following property to your `portal-ext.properties` file:

```properties
module.framework.export.password.attributes=true
```

Restart your portal instance for the change to take place.

### Key/Value Syntax <a href="#keyvalue-syntax" id="keyvalue-syntax"></a>

The syntax for all keys and values in a `.config` file is the same:

```properties
configurationName="value"
```

For single value configurations without special characters, that’s all there is to know. Settings with multiple values and certain characters are slightly different.

#### Multi-Value Settings <a href="#multi-value-settings" id="multi-value-settings"></a>

Configuration entries can have properties that accept multiple values. For example, a configuration property for specifying supported file extensions needs more than one value. Here’s how to write a multi-value setting in a `.config` file:

```properties
multiValueSetting=["Value 1","Value 2", ...]
```

If you use a space character between values (after the comma), the property is ignored.

Open the Web Content category in System Settings (under the Content section), and select _Web Content_ for the virtual instance scope. You’ll see multiple single value entries for _Characters Blacklist_:

![The Web Content System Settings entry has many Characters Blacklist fields.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/configuration-files-and-factories/using-configuration-files/images/02.png)

In the configuration file, this is represented by single key with an array of comma-separated values:

```properties
charactersblacklist=["&","'","@","\\","]","}",":","\=",">","/","<","[","{","%","+","#","`","?","\"",";","*","~"]
```

#### Escaping Characters <a href="#escaping-characters" id="escaping-characters"></a>

Double quotes (`"`) and equals signs (`=`) must be _escaped_ in `.config` files. Escaping is using another character to denote that a character shouldn’t be used in its normal way. Since double quotes and equals signs are already used in `.config` files, escaping them tells the system not to read them the normal way, but to pass them through as part of the value. Use a backslash to escape characters in the `.config` file:

```properties
charactersblacklist=["&","\"","\="]
```

This setting illustrates a multi-value setting with a regular, unescaped character (`&`), and two escaped ones (`\"` and `\=`).

Along with the mandatory escaping of double quotes and equals characters, it’s beneficial to escape spaces inside values to avoid problems. In this example, a backslash is used before each space character to ensure it’s read and processed properly:

```properties
blacklistBundleSymbolicNames=["Liferay\ Marketplace","Liferay\ Sharepoint\ Connector"]
```

If you don’t escape spaces yourself, the system adds the backslash for you after deployment.

### Typed Values <a href="#typed-values" id="typed-values"></a>

The `.config` file format supports specifying the type of a configuration value by inserting a special type marker character. Because the system already knows the correct type for each configuration property, the type characters are only useful for informational purposes. For example, a configuration with a boolean type has _B_ just before the value to mark it as a boolean type:

```properties
addDefaultStructures=B"true"
```

If you see type markers in `.config` files, you can safely ignore them. The example included above functions identically without the type marker:

```properties
addDefaultStructures="true"
```

### Deploying a Configuration File <a href="#deploying-a-configuration-file" id="deploying-a-configuration-file"></a>

Once you have a configuration file, deploy it. It’s registered and the targeted configuration values are updated automatically.

To deploy the `.config` file, place it in your Home’s `osgi/configs` folder. To change the configuration further, you can edit the `.config` file directly or use System Settings.

### Configuration Files and Clustering <a href="#configuration-files-and-clustering" id="configuration-files-and-clustering"></a>

In a clustered environment, each node needs the same configuration values for each entry. For example, all nodes should use the same Blogs configuration settings. To accomplish this, deploy a `.config` file. For transparency and maintainability, deploy the `.config` file to all nodes in the cluster. However, an internal system applies the change to all nodes in the cluster even if the configuration file is only deployed to a single node.

IMPORTANT

If storing your portal configuration (e.g., Home) in a source control system, make sure to include the OSGi configuration files (.config files).

### Setting Read-Only Configurations for a Schema <a href="#setting-read-only-configurations-for-a-schema" id="setting-read-only-configurations-for-a-schema"></a>

Available 7.4+

When provisioning specific sites or instances, it may be useful to set certain configurations to be read-only. To do this,

1. Navigate to the specific configuration you wish to set as read-only. For example, navigate to _Control Panel_ → _Instance Settings_ → _Blogs_.
2. Change and save your configuration settings. Click the ![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) icon. Click Export.
3.  On the command line, set the exported configuration file to be read-only for that file. For example,

    ```bash
    chmod -w com.liferay.blogs.configuration.BlogsGroupServiceConfiguration.scoped_0efd1a18-105e-445d-8b42-0ea2d6117690.config
    ```
4.  Place the config file into the `/osgi/configs` folder of your installation. Refresh the settings page. The configuration is now read-only.

    ![Place the config file into /osgi/configs and the configuration is now read-only.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/configuration-files-and-factories/using-configuration-files/images/03.png)

### Specifying a Single Read-Only Value <a href="#specifying-a-single-read-only-value" id="specifying-a-single-read-only-value"></a>

To specify a single read-only configuration value, use a [portal property](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties).

Available 7.4+, 7.3, 7.2

Here’s the property syntax:

```properties
configuration.override.${pid}_${key}=${value}{code}
```

For example,

```properties
configuration.override.com.liferay.journal.configuration.JournalServiceConfiguration_indexAllArticleVersionsEnabled=B"false"
configuration.override.com.liferay.journal.configuration.JournalServiceConfiguration_journalArticleMaxVersionCount=I"5"{code}
```

IMPORTANT

Configuration properties must specify type markers.

Property configurations are applied on server restart. Property configurations take highest priority; they’re prioritized over configuration files and database-persisted configurations. The UI distinguishes all read-only configuration settings. If you must change a read-only configuration value, use a portal property.

NOTE

Read-only factory configuration values are not supported.

## Using Factory Configuration <a href="#using-factory-configuration" id="using-factory-configuration"></a>

Most portal System Settings allow just one set of configurations for an entry. To change the configuration, you update the single configuration form, and it applies throughout the affected configuration scope. Less commonly, a configuration supports multiple entries that simultaneously coexist: these are called _factory configurations_. For factory configurations, each configuration set provides a unique configuration. A common usage is for configuring system-scoped connections to third party servers (e.g., LDAP and Elasticsearch). Since each connection to these servers requires unique configuration values, a factory configuration is used to make it possible.

TIP

Factory Configuration Example: Adding Organization types is supported, and is useful if you need to model real-life hierarchies or enforce hierarchical rules. In the portal, each Organization type is created via a factory configuration entry in System Settings.

### Identifying Factory Configurations <a href="#identifying-factory-configurations" id="identifying-factory-configurations"></a>

If a service supports factory configurations, its System Settings entry has an Add button.

![If a System Settings entry has an ADD button, it supports factory configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/configuration-files-and-factories/using-configuration-files/images/01.png)

Click the Add button ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) to add a new set of configuration values.

### Factory Configuration Files <a href="#factory-configuration-files" id="factory-configuration-files"></a>

As with single-instance configurations, you can set factory configurations in the System Settings interface (using the Add button described above) or via configuration files. A standard single-instance configuration file uses the fully qualified class name of the configuration object, appended by `.config`:

```bash
my.service.ServiceConfiguration.config
```

If your service supports factory configurations, use the convention of calling the configuration’s first instance `~default.config`. The default Organization type is named in this way:

```bash
com.liferay.organizations.internal.configuration.OrganizationTypeConfiguration~default.config
```

NOTE

Instead of using a tilde (`~`) to separate a Factory Configuration file’s name and subname, the portal versions 7.0-7.3 use a dash (`-`) or an underscore (`_`), depending on your browser and OS. For example,

Dash:

`com.liferay.organizations.internal.configuration.OrganizationTypeConfiguration-default.config`

Underscore:

`com.liferay.organizations.internal.configuration.OrganizationTypeConfiguration_default.config`

Although tilde is the preferred separator character, dash and underscore are still supported.

The next instance contains a unique _subname_ (something other than _default_). It’s good practice to use a descriptive name that sheds light on when this instance should be used. Following the example from Adding a New Organization Type, you could add the _League_ type with a configuration file named

```bash
com.liferay.organizations.internal.configuration.OrganizationTypeConfiguration~league.config
```

WARNING

Providing a configuration file with a subname forces a factory configuration scenario, even if the service isn’t designed to accept multiple configuration entries. Use the System Settings UI as described above to determine if using factory configurations is supported for a configuration entry.

Some System Settings entries that support factory configuration don’t ship with a configuration file for the default instance (e.g., the Anonymous User entry). If you export a factory configuration file to obtain the `.config` file, it doesn’t use the `~default.config` naming convention. Instead, whether it’s the first occurrence or an additional one, it’s given a guaranteed unique identifier for its subname:

```bash
com.liferay.user.associated.data.web.internal.configuration.AnonymousUserConfiguration~6befcd73-7c8b-4597-b396-a18f64f8c308.config
```

If you’re exporting the configuration file for deployment in a separate system, you can rename part of the exported file name after the first `~` to use a more descriptive subname. Be careful: if you rename the file and deploy it to the same system it was exported from, the new subname marks it as an entirely new configuration. You’ll end up with an additional configuration instance in this case, not just a renamed one.

WARNING

For configuration entries supporting factory configurations, omitting the subname from a `.config` file’s name causes System Settings to disallow adding new entries for the configuration entry targeted by this `.config` file. This is caused by a known bug. See LPS-76352 for more information. Once an improperly named configuration file is deployed, you can’t add any entries for the configuration in question from its System Settings entry.

Deploying an erroneous (lacking a subname) `.config` file doesn’t disable anything permanently. Rename the file using the proper convention described above or remove it entirely and start over.

Capability:[\
](https://learn.liferay.com/search?category=19239469)
