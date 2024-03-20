# Form Runner navigation bar

The Form Runner navigation bar shows at the top of the page when Advanced Forms is not embedded within another application.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-0c0b7972ec9d4cfcca0556a1f715783c4a2ad658%252Fnavbar.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=c8dc7d22aeaafebc99529e1e436afe9922f57aebe0a35c4c93b654705a158291)

Form Runner navigation bar

It features the following elements:

* The logo, which can be customized
* The form title
  * This always shows
* The language selector
* The login menu
  * this is disabled by default
* The link to the Landing page
  * this is enabled by default
* The Revision History button
  * this is disabled by default
  * available only on the Detail page in Edit mode
* The Share button
  * this is disabled by default
  * available only on the Detail page in Edit/View modes

### Configuration <a href="#configuration" id="configuration"></a>

#### Logo <a href="#logo" id="logo"></a>

See Default logo for details.

#### Language selector <a href="#language-selector" id="language-selector"></a>

The language selector is enabled by default. You can disable it globally by setting the following property to `false`:

```
<property
    as="xs:boolean"
    name="oxf.fr.navbar.language-selector.enable"
    value="false"/>
```

#### Landing page link <a href="#landing-page-link" id="landing-page-link"></a>

This link points to the Landing page. It is enabled by default, but you can disable it globally by setting the following property to `false`:

```
<property
    as="xs:boolean"
    name="oxf.fr.navbar.home-link.enable"
    value="false"/>
```

#### Revision History button <a href="#revision-history-button" id="revision-history-button"></a>

This button opens the Revision History pane. It is disabled by default, but you can enable it or disable it by app/form by setting the following property to `true`:

```
<property
    as="xs:boolean"
    name="oxf.fr.navbar.data-history.enable.*.*"
    value="true"/>
```

Form Builder explicitly disables it by default, so if you'd like to enable the Revision History button for Form Builder, you must explicitly enable it:

```
<property
    as="xs:boolean"
    name="oxf.fr.navbar.data-history.enable.forms.builder"
    value="true"/>
```
