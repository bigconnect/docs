# Portal Widget

### **Add the Advanced Forms portlet**

Log into the portal, go to the Add menu and create a new page. Then go to the Applications tab and click or drag "Advanced Forms Portlet" to the page.&#x20;

The Advanced Forms home page will show:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBz31966NDzS0eKU7%252Fliferay-full-home.png%3Fgeneration=1528743339462964%26alt=media\&width=768\&dpr=4\&quality=100\&sign=96130122e513959229749f10884d4378d2872c9a0966043d84387da34cd6af79)

### Accessing Liferay users and roles <a href="#accessing-liferay-users-and-roles" id="accessing-liferay-users-and-roles"></a>

**Using the xxf:get-request-header() function**

When running in Liferay, you can access some specific user and roles information from XForms.

Getting the current user's email:

Copy

```
xxf:get-request-header('orbeon-liferay-user-email')
```

Example:

`'test@liferay.com'`

Getting the current user's full name:

Copy

```
xxf:get-request-header('orbeon-liferay-user-full-name')
```

Example:

`'Joe Bloggs'`

Getting the current user's role names:

Copy

```
xxf:get-request-header('orbeon-liferay-user-roles')
```

This returns a sequence of strings, with one string per role.

Example:

`('Administrator', 'Power user', 'User')`

Alternatively, you can use the standard `Orbeon-Username` and `Orbeon-Roles` headers. See also [Access Control](https://doc.orbeon.com/form-runner/access-control).

**Portlet filters**

\[SINCE Orbeon Forms 4.10]

The portlet is configured to use two portlet filters in `portlet.xml`.

The first filter, `AddLiferayUserHeadersFilter`, extracts Liferay user information and sets it as headers ("properties" in portlet API parlance). This sets the following properties/headers:

* `Orbeon-Liferay-User-Id`
* `Orbeon-Liferay-User-Screen-Name`
* `Orbeon-Liferay-User-Full-Name`
* `Orbeon-Liferay-User-First-Name` \[SINCE Orbeon Forms 2018.1]
* `Orbeon-Liferay-User-Middle-Name` \[SINCE Orbeon Forms 2018.1]
* `Orbeon-Liferay-User-Last-Name` \[SINCE Orbeon Forms 2018.1]
* `Orbeon-Liferay-User-Email`
* `Orbeon-Liferay-User-Group-Id`
* `Orbeon-Liferay-User-Group-Name`
* `Orbeon-Liferay-User-Roles`

The second filter, `AddOrbeonAuthHeadersFilter`, handles either container or header-based auth, and then sets the following headers/properties:

* `Orbeon-Username`
* `Orbeon-Group`
* `Orbeon-Roles`

These in turn are used internally by Orbeon to handle other aspects of authentication/authorization.

#### Performance tuning <a href="#performance-tuning" id="performance-tuning"></a>

For large pages, we have found that the Liferay strip filter can take an extremely long time to process a response. You can disable that filter in your `portal-ext.properties` with:

Copy

```
com.liferay.portal.servlet.filters.strip.StripFilter=false
```

_NOTE: `portal-ext.properties` is a standard Liferay configuration file you place under the_ [_Liferay Home directory_](https://dev.liferay.com/discover/deployment/-/knowledge\_base/6-2/liferay-home)_._

### Creating a new landing page for the portlet <a href="#creating-a-new-landing-page-for-the-portlet" id="creating-a-new-landing-page-for-the-portlet"></a>

By default, the portlet shows a list of Orbeon Forms sample forms and apps.

To change this, you need to:

1. Modify `WEB-INF/resources/page-flow-portlet.xml.`
2. Create a new landing page in XHTML format.
3. Change the default Orbeon theme so that no custom Orbeon CSS is added.

A simple way do implement this is as follows:

First, replace this line in `page-flow-portlet.xml`:

Copy

```
<page path="/" model="apps/home/page-flow.xml"/>
```

with:

Copy

```
<page path="/" view="home.xhtml"/>
```

Second, create a new file, `WEB-INF/resources/home.xhtml`, with content such as:

Copy

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

Finally, to change the Orbeon portlet theme to the plain theme, set this property:

Copy

```
<property
  as="xs:anyURI"
  name="oxf.epilogue.theme.embeddable"
  value="oxf:/config/theme-embeddable.xsl"/>
```
