# Persistence

### Levels of configuration <a href="#levels-of-configuration" id="levels-of-configuration"></a>

Storage for the persistence hierarchy can be configured at multiple levels:

* Globally
* For each _application_
  * The application is referred to with an _app name._
* For each _form definition_ within an application
  * The form definition is referred to with a _form name._
* For each _resource type_ (form definition vs. form data)
  * The resource type is referred to with an identifier:
    * _form_: form definition as XHTML, with optional associated resources (images, PDF template, and other attachments)
    * _data_: form data as XML, with optional associated attachments

This allows you for example to store certain form definition on disk, while storing the associated data, as filled-out by users, in one or more databases.

### Persistence layer configuration <a href="#persistence-layer-configuration" id="persistence-layer-configuration"></a>

Advanced Forms introduces a level of indirection in the persistence layer configuration: you map the parameters _app_, _form_, and _form definition or data_ to a _provider_, and then map that provider to its REST persistence API. This is configured via properties starting with `oxf.fr.persistence.provider`.

By default, eXist is configured for all apps and forms, including form definitions and form data. This is done by associating the `exist` provider:

```
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.*.*.*"
    value="exist"/>
```

In each such provider mapping, there are 3 configurable pieces of information. They represent, in this order:

1. A string to specify the Form Runner application name, like `forms` or `acme`.
2. A Form Runner form name, like `registration` or `address`.
3. Whether the configuration regards form data (`data`), or the form configuration files (`form`).

Each of those can be a wildcard (`*`). Wildcards allow you to setup Advanced Forms to use different persistence providers for different app, form, and resource type combinations.

The properties are interpreted hierarchically and you may specify the configuration more or less specifically:

| Property                                         | Explanation                                           |
| ------------------------------------------------ | ----------------------------------------------------- |
| `oxf.fr.persistence.provider.*.*.*`              | form definitions and form data for all applications   |
| `oxf.fr.persistence.provider.*.*.data`           | form data for all applications                        |
| `oxf.fr.persistence.provider.*.*.form`           | form definitions for all applications                 |
| `oxf.fr.persistence.provider.forms.*.form`       | form definitions for all forms in application "forms" |
| `oxf.fr.persistence.provider.forms.*.data`       | form data for all forms in application "forms"        |
| `oxf.fr.persistence.provider.forms.contact.*`    | form definitions and data for "forms/contact"         |
| `oxf.fr.persistence.provider.forms.contact.form` | form definitions for "forms/contact"                  |
| `oxf.fr.persistence.provider.forms.contact.data` | form data for "forms/contact"                         |

_NOTE: This means that you can't name a persistence provider `provider`._

For example some built-in demo forms as well as Form Builder load their form definitions directly from the Advanced Forms web application. This is done by using more specific properties (without wildcards):

```
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.forms.builder.form"
    value="resource"/>
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.forms.dmv-14.form"
    value="resource"/>
```

One thing you might notice is that, as far as configuring persistence, you treat Form Builder itself just like another form, with app name `forms` and form name `builder`.

If you want to change the default provider to Oracle, and since a provider named `oracle` is predefined, the following configuration will do just that:

```
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.*.*.*"
    value="oracle"/>
```

Because wildcards are used, this property does not override the configuration for the built-in demo forms as well as Form Builder! This is desirable, because the Form Builder implementation itself is not usually something you want to store somewhere else.

In the case of Oracle, the wildcards also allow you to setup Advanced Forms to use different schemas for different app, form, and resource type combinations. For instance you could store everything related to an `hr` app in one Oracle database schema and everything related to another `finance` app in another Oracle database schema.&#x20;

Each provider supports standard properties, as follows:

| Property                                                    | Explanation                                             |
| ----------------------------------------------------------- | ------------------------------------------------------- |
| `oxf.fr.persistence.[provider].uri`                         | The location, via HTTP, of the provider implementation. |
| `oxf.fr.persistence.[provider].active`                      | Whether the provider is active                          |
| `oxf.fr.persistence.[provider].autosave`                    | Whether autosave is supported.                          |
| `oxf.fr.persistence.[provider].permissions`                 | Whether user/group permissions are supported            |
| `oxf.fr.persistence.[provider].versioning`                  | Whether versioning is supported                         |
| `oxf.fr.persistence.[provider].data-format-version`         | The data format version used in the database            |
| `oxf.fr.persistence.[provider].escape-non-ascii-characters` | Whether non-ASCII characters should be escaped          |

#### `uri` property <a href="#property_uri" id="property_uri"></a>

The `uri` property specifies the location, via HTTP, of the provider implementation.

```
<property
    as="xs:anyURI"
    name="oxf.fr.persistence.[provider].uri"
    value="[URI to reach the provider]"/>
```

#### `active` property <a href="#property_active" id="property_active"></a>

The `active` property specifies whether the provider is active.

This is used as follows:

* If active, the Form Runner Home Page queries the persistence implementation to obtain the list of published forms and enable administrative operations.

```
<property
    as="xs:boolean"
    name="oxf.fr.persistence.[provider].active"
    value="[true|false]"/>
```

#### `autosave` property <a href="#property_autosave" id="property_autosave"></a>

The `autosave` property specifies whether autosave is supported.

This is used as follows:

* If `true`, Form Runner enables autosave if the other conditions are met. Otherwise, Form Runner will not attempt to enable autosave.

```
<property
    as="xs:boolean"
    name="oxf.fr.persistence.[provider].autosave"
    value="[true|false]"/>
```

#### `permissions` property <a href="#property_permissions" id="property_permissions"></a>

The `permissions` property specifies whether user/group permissions are supported.

If `true`, Form Runner assumes that permissions are supported by the provider implementation.

This is used as follows:

* The Summary Page sends a 403 if the user doesn't have access based on role, and the persistence provider is known not to support permissions.

```
<property
    as="xs:boolean"
    name="oxf.fr.persistence.[provider].permissions"
    value="[true|false]"/>
```

#### `versioning` property <a href="#property_versioning" id="property_versioning"></a>

The `versioning` property specifies whether versioning is supported.

If `true`, Form Runner assumes that versioning is supported by the provider implementation.

This is used as follows:

* At form publishing time, Form Builder proposes the option to overwrite the existing published form definition, or to create a new version.

```
<property
    as="xs:boolean"
    name="oxf.fr.persistence.[provider].versioning"
    value="[true|false]"/>
```

#### `escape-non-ascii-characters` property <a href="#property_escape-non-ascii-characters" id="property_escape-non-ascii-characters"></a>

We recommend you set up your database to store text as Unicode, to avoid potential problems when users enter non-ASCII characters, like accents, characters from non-latin languages, or even emojis. In cases when this isn't possible, you can set the following property to `true`, to instruct Advanced Forms to escape all non-ASCII characters in form data before they are sent to the database.

This escaping only applies to form data, and not to indices created based on this data, which means that if you are using the Form Runner summary page, or the search API, and that a field value shown in the summary page or returned by the search API contains a character that your database is unable to store, while your data is still safe, the summary page or the result of the search API might contain an incorrect value.

For instance, say a user enters the character `é` in a field, and that you have this property enabled, then the form data will contain `&#233;`, which is the escaped version of `é`, however, if that field is indexed (because you've enabled _Show on Summary page_ or _Allow bulk edit_ in the _Control Settings_ for that field), then the character `é` will be stored in the index table, which might be a problem if your database encoding cannot accommodate the storage of this character.

Amongst the built-in implementations of the persistence API, this property is supported for all relational databases, but not for eXist.

```
<property
    as="xs:boolean"
    name="oxf.fr.persistence.*.escape-non-ascii-characters"
    value="false"/>
```

### Multiple databases of the same type <a href="#multiple-databases-of-the-same-type" id="multiple-databases-of-the-same-type"></a>

Say you have two MySQL databases (or a single MySQL database with different schemas and users configured) and you would like to configure Advanced Forms to store form definitions and form data to one or the other of the databases. Assume the following JDBC datasources:

* `mysql_foo`
* `mysql_bar`

You associate each datasource with a Form Runner _persistence provider_ with the following properties:

```
<property as="xs:anyURI"  name="oxf.fr.persistence.mysql_foo.uri"         value="/fr/service/mysql"/>
<property as="xs:string"  name="oxf.fr.persistence.mysql_foo.datasource"  value="mysql_foo"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.autosave"    value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.permissions" value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.versioning"  value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.lease"       value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.reindex"     value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.reencrypt"   value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.sort"        value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_foo.active"      value="true"/>

<property as="xs:anyURI"  name="oxf.fr.persistence.mysql_bar.uri"         value="/fr/service/mysql"/>
<property as="xs:string"  name="oxf.fr.persistence.mysql_bar.datasource"  value="mysql_bar"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.autosave"    value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.permissions" value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.versioning"  value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.lease"       value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.reindex"     value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.reencrypt"   value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.sort"        value="true"/>
<property as="xs:boolean" name="oxf.fr.persistence.mysql_bar.active"      value="true"/>
```

Then if you would like:

* form definitions and form data for the Form Runner app `foo` to go to `mysql_foo`
* form definitions and form data for the Form Runner app `bar` to go to `mysql_bar`

Add:

```
<property as="xs:string"  name="oxf.fr.persistence.provider.foo.*.*"      value="mysql_foo"/>
<property as="xs:string"  name="oxf.fr.persistence.provider.bar.*.*"      value="mysql_bar"/>
```

### Configuration properties for specific persistence providers <a href="#configuration-properties-for-specific-persistence-providers" id="configuration-properties-for-specific-persistence-providers"></a>

Each provider may have specific configuration properties. For the latest settings, see `properties-form-runner.xml`.

### Attachments <a href="#attachments" id="attachments"></a>

By default, attachments are stored in the database. You can configure Form Runner to store attachments in the file system instead. This is useful for larger attachments, for example. You can do so globally by setting the following property:

```
<property 
    as="xs:string"
    name="oxf.fr.persistence.provider.*.*.*.attachments"
    value="filesystem"/>
```

The base directory where attachments are stored is configured with:

```
<property
    as="xs:string"
    name="oxf.fr.persistence.filesystem.directory"
    value="/path/to/attachments"/>
```

#### Multiple attachment directories <a href="#multiple-attachment-directories" id="multiple-attachment-directories"></a>

Multiple attachment directories can be configured, following the same principles as described [above](https://doc.orbeon.com/configuration/properties/form-runner/persistence#multiple-databases-of-the-same-type)

For example, if you need two different base directories for the Form Runner apps `foo` and `bar`, you can do so by using the following properties:

```
<property 
    as="xs:string"
    name="oxf.fr.persistence.provider.foo.*.*.attachments"
    value="filesystem_foo"/>
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.bar.*.*.attachments"
    value="filesystem_bar"/>

<property as="xs:anyURI" name="oxf.fr.persistence.filesystem_foo.uri"       value="/fr/service/filesystem"/>
<property as="xs:string" name="oxf.fr.persistence.filesystem_foo.directory" value="/path/to/foo_attachments"/>

<property as="xs:anyURI" name="oxf.fr.persistence.filesystem_bar.uri"       value="/fr/service/filesystem"/>
<property as="xs:string" name="oxf.fr.persistence.filesystem_bar.directory" value="/path/to/bar_attachments"/>
```

#### Dynamic base directory configuration <a href="#dynamic-base-directory-configuration" id="dynamic-base-directory-configuration"></a>

In addition to static paths, you can also use an AVT to dynamically configure the base directory. For instance, the following would use a base directory specified by an environment variable:

```
<property
    as="xs:string"
    name="oxf.fr.persistence.filesystem.directory"
    value="{environment-variable('ATTACHMENTS_BASE_DIRECTORY')}"/>
```

Note that, specifically in the context of the `oxf.fr.persistence.*.directory` property, it is not necessary to set `oxf.xpath.environment-variable.enabled` to true in order to use the `environment-variable()` function.
