# Summary page

### Adding your own CSS files <a href="#adding-your-own-css-files" id="adding-your-own-css-files"></a>

\[SINCE Orbeon Forms 2017.1]

In addition to `oxf.fr.css.custom.uri`, you can also use the following property, which apply only to the Summary page:

Copy

```
<property
    as="xs:string"
    name="oxf.fr.summary.css.custom.uri.*.*"
    value="/forms/acme/assets/acme-summary.css"/>
```

See also [Adding your own CSS](https://doc.orbeon.com/configuration/properties/form-runner#adding-your-own-css).

### Adding your own JavaScript files <a href="#adding-your-own-javascript-files" id="adding-your-own-javascript-files"></a>

\[SINCE Orbeon Forms 2017.1]

In addition to `oxf.fr.js.custom.uri`, you can also use the following property, which apply only to the Summary page:

Copy

```
<property
    as="xs:string"
    name="oxf.fr.summary.js.custom.uri.*.*"
    value="/forms/acme/assets/acme-summary.js"/>
```

See also [Adding your own JavaScript](https://doc.orbeon.com/configuration/properties/form-runner#adding-your-own-javascript).

### Summary page size <a href="#summary-page-size" id="summary-page-size"></a>

Copy

```
<property
    as="xs:integer"
    name="oxf.fr.summary.page-size.*.*"
    value="10"/>
```

Number of rows shown in the Summary page.

### Created and Last Modified columns <a href="#created-and-last-modified-columns" id="created-and-last-modified-columns"></a>

By default, the Summary page shows a Created and Modified columns:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-7fb63f99d87dafe9e8ea9c24fe20077b98f9fd5a%252Fsummary-created-last-modified.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=f11d232046dc62d024e36e509a61a368e08792e82059287ae4c8fe044fe3fb9f)

Created and Last Modified

You can remove either one of those columns by setting the appropriate property value to `false`:

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.summary.show-created.*.*"
    value="true"/>

<property
    as="xs:boolean"
    name="oxf.fr.summary.show-last-modified.*.*"
    value="true"/>
```

### Show the workflow stage <a href="#show-the-workflow-stage" id="show-the-workflow-stage"></a>

\[SINCE Orbeon Forms 2020.1]

You can add this column by setting the following property value to `true`:

Copy

```
<property 
    as="xs:boolean" 
    name="oxf.fr.summary.show-workflow-stage.*.*"                           
    value="true"/>
```

### Show created by and last modified by users <a href="#show-created-by-and-last-modified-by-users" id="show-created-by-and-last-modified-by-users"></a>

\[SINCE Orbeon Forms 2021.1]

You can add either one of those columns by setting the appropriate property value to `true`:

Copy

```
<property 
    as="xs:boolean" 
    name="oxf.fr.summary.show-created-by.*.*"                           
    value="true"/>
<property 
    as="xs:boolean" 
    name="oxf.fr.summary.show-last-modified-by.*.*"                   
    value="true"/>
```

### Buttons on the Summary page <a href="#buttons-on-the-summary-page" id="buttons-on-the-summary-page"></a>

See [Configuring Summary page buttons](https://doc.orbeon.com/form-runner/advanced/buttons-and-processes/summary-page-buttons-and-processes#configuring-summary-page-buttons).

### Versioning <a href="#versioning" id="versioning"></a>

The following property allows you to configure whether the summary page shows data:

* if set to `true`: for one version at a time, which is the default;
* if set to `false`: for all data across form definition versions, as it used to be the case up until Orbeon Forms 2018.1.

Even when set to `false`, if the `form-version` request parameter is passed to the summary page, it will not ignore the request parameter and will only show data created with that version of the form definition.

```
<property 
    as="xs:boolean"
    name="oxf.fr.summary.show-version-selector.*.*"
    value="false"/>
```

When the property is set to `false`, links to the summary page from the Published Forms and Landing pages will not include the `form-version` request parameter.
