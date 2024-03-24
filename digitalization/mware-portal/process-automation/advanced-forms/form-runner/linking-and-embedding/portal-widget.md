# Portal Widget

### **Add the Advanced Forms portlet**

Log into the portal, go to the Add menu and create a new page. Then go to the Applications tab and click or drag "Advanced Forms Portlet" to the page.&#x20;

The Advanced Forms home page will show:

<figure><img src="../../../../../../.gitbook/assets/image (1) (1).jpg" alt=""><figcaption></figcaption></figure>

### Accessing portal users and roles <a href="#accessing-liferay-users-and-roles" id="accessing-liferay-users-and-roles"></a>

**Using the xxf:get-request-header() function**

You can access some specific user and roles information from XForms.

Getting the current user's email:

```
xxf:get-request-header('forms-liferay-user-email')
```

Example:

`'test@portal.com'`

Getting the current user's full name:

```
xxf:get-request-header('forms-liferay-user-full-name')
```

Example:

`'Joe Bloggs'`

Getting the current user's role names:

```
xxf:get-request-header('forms-liferay-user-roles')
```

This returns a sequence of strings, with one string per role.

Example:

`('Administrator', 'Power user', 'User')`

Alternatively, you can use the standard `Forms-Username` and `Forms-Roles` headers.&#x20;

**Portlet filters**

The portlet is configured to use two portlet filters in `portlet.xml`.

The first filter, `AddLiferayUserHeadersFilter`, extracts Liferay user information and sets it as headers ("properties" in portlet API parlance). This sets the following properties/headers:

* `Forms-Liferay-User-Id`
* `Forms-Liferay-User-Screen-Name`
* `Forms-Liferay-User-Full-Name`
* `Forms-Liferay-User-First-Name`&#x20;
* `Forms-Liferay-User-Middle-Name`&#x20;
* `Forms-Liferay-User-Last-Name`&#x20;
* `Forms-Liferay-User-Email`
* `Forms-Liferay-User-Group-Id`
* `Forms-Liferay-User-Group-Name`
* `Forms-Liferay-User-Roles`

The second filter, `AddFormsAuthHeadersFilter`, handles either container or header-based auth, and then sets the following headers/properties:

* `Forms-Username`
* `Forms-Group`
* `Forms-Roles`

These in turn are used internally by Advanced Forms to handle other aspects of authentication/authorization.

#### Performance tuning <a href="#performance-tuning" id="performance-tuning"></a>

For large pages, we have found that the portal strip filter can take an extremely long time to process a response. You can disable that filter in your `portal-ext.properties` with:

```
com.liferay.portal.servlet.filters.strip.StripFilter=false
```

_NOTE: `portal-ext.properties` is a standard Liferay configuration file you place under the Liferay Home directory._

### Creating a new landing page for the portlet <a href="#creating-a-new-landing-page-for-the-portlet" id="creating-a-new-landing-page-for-the-portlet"></a>

By default, the portlet shows a list of Advanced Forms sample forms and apps.

To change this, you need to:

1. Modify `WEB-INF/resources/page-flow-portlet.xml.`
2. Create a new landing page in XHTML format.
3. Change the default Advanced Forms theme so that no custom Advanced Forms CSS is added.

A simple way do implement this is as follows:

First, replace this line in `page-flow-portlet.xml`:

```
<page path="/" model="apps/home/page-flow.xml"/>
```

with:

```
<page path="/" view="home.xhtml"/>
```

Second, create a new file, `WEB-INF/resources/home.xhtml`, with content such as:

```
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>Form Runner Home</title>
    </head>
    <body>
        <a href="/fr/">Link to the Form Runner home page</a>
        <a href="/fr/acme/form1/summary">Link to summary page of form acme/form1</a>
        <a href="/fr/acme/form2/new">Link to new page of form acme/form2</a>
    </body>
</html>
```

This is the landing page itself, and it can contain any XHTML you like. Typically would include links to specific Form Runner paths as shown in the example above.

Finally, to change the Advanced Forms portlet theme to the plain theme, set this property:

```
<property
  as="xs:anyURI"
  name="oxf.epilogue.theme.embeddable"
  value="oxf:/config/theme-embeddable.xsl"/>
```
