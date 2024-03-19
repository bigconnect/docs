# Detail page

### HTML page layout <a href="#html-page-layout" id="html-page-layout"></a>

Available modes:

* `fixed` (default)
* `fluid`

When in `fluid` mode, the form sections and grids take the entire web browser's viewport size. This also applies when using the wizard view.

This can also be configured for a particular form in Form Builder's Form Settings dialog.

Copy

```
<property
    as="xs:string"
    name="oxf.fr.detail.html-page-layout.*.*"
    value="fixed"/>
```

### Adding your own CSS files <a href="#adding-your-own-css-files" id="adding-your-own-css-files"></a>

In addition to `oxf.fr.css.custom.uri`, you can also use the following property, which apply only to the Detail page:

```
<property as="xs:string" name="oxf.fr.detail.css.custom.uri.*.*">
    /forms/acme/assets/acme-detail.css"
</property>
```

### Adding your own JavaScript files <a href="#adding-your-own-javascript-files" id="adding-your-own-javascript-files"></a>

In addition to `oxf.fr.js.custom.uri`, you can also use the following property, which apply only to the Detail page:

```
<property as="xs:string" name="oxf.fr.detail.js.custom.uri.*.*">
    /forms/acme/assets/acme-detail.js"
</property>
```

### Position of error summary <a href="#position-of-error-summary" id="position-of-error-summary"></a>

```
<property
    as="xs:string"
    name="oxf.fr.detail.error-summary.*.*"
    value="bottom"/>
```

Where to place the error summary: `top`, `bottom`, `both`, or `none`.

### Buttons on the Detail Page <a href="#buttons-on-the-detail-page" id="buttons-on-the-detail-page"></a>

#### Choosing which buttons are shown <a href="#choosing-which-buttons-are-shown" id="choosing-which-buttons-are-shown"></a>

```
<property
    as="xs:string"
    name="oxf.fr.detail.buttons.*.*"
    value="close clear print pdf save submit"/>
```

The property configures which buttons are included on the Detail Page, and in what order they are shown.&#x20;

#### Hiding and disabling buttons <a href="#hiding-and-disabling-buttons" id="hiding-and-disabling-buttons"></a>

The following properties, where you replace `BUTTON` by a specific button name, control whether a particular button is visible (button visibility) or disabled (or "readonly"):

```
oxf.fr.detail.button.BUTTON.visible.*.*
oxf.fr.detail.button.BUTTON.enabled.*.*
```

The value of these properties is an XPath expression. For example the following properties hide, show, and disable buttons depending on whether the wizard shows its table of contents or its body:

```
  <property as="xs:string"  name="oxf.fr.detail.button.wizard-prev.visible.*.*"               >
      fr:owns-lease-or-none-required() and fr:is-wizard-body-shown()
  </property>
  <property as="xs:string"  name="oxf.fr.detail.button.wizard-prev.enabled.*.*">
      not(fr:is-wizard-first-page())
  </property>
  <property as="xs:string"  name="oxf.fr.detail.button.wizard-next.visible.*.*">
      fr:owns-lease-or-none-required() and fr:is-wizard-body-shown()
  </property>
  <property as="xs:string"  name="oxf.fr.detail.button.wizard-next.enabled.*.*">
      not(fr:is-wizard-last-page())
  </property>
  <property as="xs:string"  name="oxf.fr.detail.button.wizard-toc.visible.*.*">
      fr:owns-lease-or-none-required() and fr:is-wizard-separate-toc() and fr:is-wizard-body-shown()
  </property>
```

You can access control values in the data in one of two ways, where `foo` is the name of the control:

* Use an expression of the type `//foo`. Note that control elements might not be unique in case of repeats or section templates, and so this returns as many XML elements as there are values in the data, including within repeats and within section templates.
* Use `fr:control-string-value('foo')`. This only works for controls that are not in a section template and returns zero or one value. If the control is repeated, only the first value is returned.
* _NOTE: It's not yet possible to use the variable notation `$foo`._

Example searching data elements:

```
<property as="xs:string"  name="oxf.fr.detail.button.save-final.visible.*.*">
    xxf:non-blank(//foo)
</property>
```

Example with `fr:control-string-value()`:

```
<property as="xs:string"  name="oxf.fr.detail.button.save-final.visible.*.*">
    xxf:non-blank(fr:control-string-value('foo'))
</property>
```

#### Loading indicator for buttons <a href="#loading-indicator-for-buttons" id="loading-indicator-for-buttons"></a>

The property `oxf.fr.detail.loading-indicator.BUTTON.*.*`, where you replace `BUTTON` by a specific button name, allows you to configure which loading indicator, if any, is to be used for that button. The value of the property can be either:

* Empty, which is the default, and means "no loading indicator".
* `modal`, greys out the background, shows a spinner in the center of the screen, and prevents any user input as long as the action triggered by the button is being processed.
* `inline`, shows a spinner inside the button itself.

In the following example, the `send` button is made modal:

```
<property 
    as="xs:string"  
    name="oxf.fr.detail.loading-indicator.send.*.*"                 
    value="modal"/>
```

In general, we would expect this property to be used as follows:

* `modal` for buttons performing actions for which allowing users to change the value of fields after the button is pressed wouldn't make any sense, would be confusing, or outright dangerous. This would for instance be the case for _submit_ or _publish_ buttons.
* `inline` for buttons performing actions that are expected to take a little bit of time, like a _save_ operation.
* Empty for any other button.

In all cases, should an action take any noticeable amount of time, Advanced Forms will always show a loading bar at the top of the page, so users know one of their actions is being processed.

By default, as shown in the below video:

* The `modal` loading indicator used for the `submit` button.
* The `inline` loading indicator for the _save_ buttons (`save-draft` (up to 2020.1.x), `save-progress` (2021.1 and newer), and `save-final`).

![Loading indicators](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwGjf5eBo6n7Dci4%252Floading-indicators.gif%3Fgeneration=1528743356431132%26alt=media\&width=768\&dpr=4\&quality=100\&sign=89dd8592df62e661b43ce0425af329349f25331c033df19abb5a6c0d71f9db3e)

### Controlling the appearance of control labels <a href="#controlling-the-appearance-of-control-labels" id="controlling-the-appearance-of-control-labels"></a>

By default, with Form Runner, control labels appear _inline_ above the control. The following property allows overriding this behavior:

```
<property
    as="xs:string"
    name="oxf.xforms.label.appearance.*.*"
    value="full"/>
```

Allowed values:

* `full`: labels show inline above the control (the default)
* `full minimal`: labels show inline above the control, but for text, date, and time input fields only, labels show as an HTML _placeholder_ within the field when the field is empty

_LIMITATION: The `minimal` appearance is not supported on combined "Date and Time" fields and on text fields with "Character Counter" appearance._

_NOTE: Only one `minimal` appearance can be used between `oxf.xforms.label.appearance` and `oxf.xforms.hint.appearance`. If both include `minimal`, the label wins._

### Controlling the appearance of control hints <a href="#controlling-the-appearance-of-control-hints" id="controlling-the-appearance-of-control-hints"></a>

By default, with Form Runner, control hints appear _inline_ under the control. The following property allows overriding this behavior:

```
<property
    as="xs:string"
    name="oxf.xforms.hint.appearance.*.*"
    value="full"/>
```

Allowed values:

* `full`: hints show inline below the control (the default)
* `full minimal`: hints show inline below the control, but for text, date, and time input fields only, hints show as an HTML _placeholder_ within the field when the field is empty
* `tooltip`: hints show as tooltips upon mouseover
* `tooltip minimal`: hints show as tooltips upon mouseover, but for input fields only, hints show as an HTML _placeholder_ within the field when the field is empty

Here is how hints appear depending on the type of control they are associated with:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBtUVPhNrIEbjNXU9%252Fplaceholder-and-inline-hints.png%3Fgeneration=1528743309737953%26alt=media\&width=768\&dpr=4\&quality=100\&sign=ff680e57205d708523f5460405b7874f3e5bc249f861c6abdfcbb77fcc35cbac)

_LIMITATION: The `minimal` appearance is not supported on combined "Date and Time" fields and on text fields with "Character Counter" appearance._

_NOTE: Only one `minimal` appearance can be used between `oxf.xforms.label.appearance` and `oxf.xforms.hint.appearance`. If both include `minimal`, the label wins._

### Display hints inline <a href="#display-hints-inline" id="display-hints-inline"></a>

This property set whether the control hints are shown inline, rather than as tool-tips. The default is `true`.

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.hints.inline.*.*"
    value="true"/>
```

This property is deprecated. Use `oxf.fr.detail.hint.appearance` instead. For backward compatibility, when this property is present, it overrides `oxf.xforms.hint.appearance` and sets it to:

* `full` if set to `true`
* `tooltip` if set to `false`

### Order of LHHA elements <a href="#order-of-lhha-elements" id="order-of-lhha-elements"></a>

This property sets the respective order, in the generated HTML markup, of label/help/hint/alert and the control element.

_NOTE: It is not recommended to change the default value of this property, which was introduced in the days where CSS couldn't do all it can do now. We recommend styling using CSS instead._

```
<property 
    as="xs:string" 
    name="oxf.fr.detail.lhha-order.*.*"                               
    value="help label control alert hint"/>
```

### Initial keyboard focus <a href="#initial-keyboard-focus" id="initial-keyboard-focus"></a>

This property controls whether Form Runner attempts to set focus on the first control upon form load. The default is `true`.

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.initial-focus.*.*"
    value="true"/>
```

In some cases, such as embedding, it can be desirable to disable this by setting the property to `false`.

### Focusable controls <a href="#focusable-controls" id="focusable-controls"></a>

The following properties determine which control types are focusable in in the following scenarios:

* initial focus (if enabled by `oxf.fr.detail.initial-focus`)
* switching sections in the table of contents
* switching sections in the wizard table of contents or navigation
* clearing the form data with the "Clear" button
* moving, inserting, or deleting repetitions in repeated grids and sections

```
<property
    as="xs:string"
    name="oxf.fr.detail.focus.includes.*.*"
    value=""/>
    
<property
    as="xs:string"
    name="oxf.fr.detail.focus.excludes.*.*"
    value="xf:trigger"/>
```

The default is to allow focus on any input control, including text fields, text areas, dropdown menus, and more. However, buttons are explicitly excluded.

The values of these properties follow the `include` and `exclude` attributes on the `<xf:setfocus>` action.

### Validation of static lists of choices <a href="#validation-of-static-lists-of-choices" id="validation-of-static-lists-of-choices"></a>

This property allows you to automatically add a validation error when a static list of choices contains invalid values. The default is `false`.

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.validate-selection-controls-choices.*.*"
    value="true"/>
```

This can be useful in the following cases:

* to catch errors where selection control values are set using calculations
* to validate work in progress data added with the Persistence API

This property might be enabled by default in the future.

This is also automatically enabled when importing form data through the Import page.

### Validation mode <a href="#validation-mode" id="validation-mode"></a>

The following property controls whether validation happens as the user types or explicitly when activating a button:

```
<property
    as="xs:string"
    name="oxf.fr.detail.validation-mode.*.*"
    value="explicit"/>
```

Values:

* `incremental`: validate as the user types (default)
* `explicit`: validate upon explicit activation of a button

The main purpose of the `explicit` mode is to mimic old-style forms, where validation traditionally happened upon pressing a "Submit" button.

By default, in `explicit` mode, validation occurs:

* when the `validate` Form Runner action runs
* with the Wizard view, in validated mode, when the user attempts to navigate to the next page or select a page in the wizard's table of contents

### Captcha <a href="#captcha" id="captcha"></a>

#### Enabling and choosing a component <a href="#enabling-and-choosing-a-component" id="enabling-and-choosing-a-component"></a>

If you are creating a public form, you might want to add a captcha to avoid spam. You can do so by enabling the _captcha_ feature, which you do by adding the following property to your `properties-local.xml`:

```
<property
    as="xs:string"
    name="oxf.fr.detail.captcha.component.*.*"
    value="reCAPTCHA"/>
```

You can set this property to either:

* Blank (empty string), to not have captcha added to your form. This is the default.
* `reCAPTCHA` or `SimpleCaptcha`, to use one the two captcha implementations built in Advanced Forms.&#x20;
* The qualified name of an XBL component you created and that implements a captcha, say `acme:custom-captcha`. When doing so, you need to have a namespace defined in your property file for the component prefix you're using, say `xmlns:acme="http://acme.org/"`.

If using the reCAPTCHA, you'll also need to add properties to specify your reCAPTCHA public and private keys.

#### Showing inside the wizard <a href="#showing-inside-the-wizard" id="showing-inside-the-wizard"></a>

The following property allows you to configure where the captcha is shown (when enabled). The two possible values are:

* `form-bottom` (the default value), to show the captcha at the bottom of the form.
* `inside-wizard` to show the captcha inside the wizard. Only use this if your form is using the wizard view, otherwise the captcha won't show.

```
<property 
    as="xs:string"  
    name="oxf.fr.detail.captcha.location.*.*"                         
    value="inside-wizard"/>
```

**Limitations**

When all the following conditions are met:

* The captcha is shown inside the wizard.
* You have set the `oxf.fr.detail.captcha.visible.*.*` property (see below) to have the captcha show only on certain pages.
* Users attempt to save or submit the form, or otherwise perform an action that requires data to be valid.
* The captcha hasn't been solved yet.

Then:

* While an error will show in the error summary to inform users that the captcha needs to be solved, when users click on the error message, Form Runner will not switch to the page where the captcha appears. (The situation here is somewhat different relative to what happens with normal fields, as depending on how you set the `oxf.fr.detail.captcha.visible.*.*` property, the captcha could appear on multiple pages.)
* The page or pages in which the captcha appear won't be highlighted, as is the case for other invalid fields.

#### Captcha visibility <a href="#captcha-visibility" id="captcha-visibility"></a>

When the captcha is enabled, you can control its visibility with the following property. The default value is `true` (shown below), and you can use a [value template](https://doc.orbeon.com/xforms/core/attribute-value-templates), to make it dynamic. For instance, you can have the captcha only show on the last page of the wizard by setting the above location property to `inside-wizard`, and this `visible` property to `{fr:is-wizard-last-page()}`.

Even when not visible according this property (i.e. your value template returns `false`), as long as the captcha is enabled, solving it is required; so this property isn't intended to be used to dynamically decide whether to have a captcha on a page or not, but it is to decide at what point during the form filling process you want the captcha to show.

```
<property 
    as="xs:string"  
    name="oxf.fr.detail.captcha.visible.*.*"                          
    value="true"/>
```

### Running processes upon page load <a href="#running-processes-upon-page-load" id="running-processes-upon-page-load"></a>

The following property controls what process(es) to run when the page loads in "new" or "edit" mode:

```
oxf.fr.detail.process.
  after-controls|after-data|before-data.
  background|foreground.
  new|edit|view|pdf|email.
  $app.
  $form
```

where `$app` and `$form` represent a Form Runner application name and/or form name or `*` wildcards, as is usual with Form Runner configuration properties.

The following process names apply:

* `after-controls`: run after the controls are ready:
  * The process runs when controls are "live", which means you can set their values and list of choices.
* `after-data`: run when the data is ready:
  * It has been loaded from the database if needed ("edit" mode).
  * Its initial values and calculations are up to date.
* `before-data`: run before the data's initial values are calculated:
  * The process runs before the data is ready.
  * You cannot set control values and list of choices as a result.

_WARNING: When running the process upon `after-data`, controls are not yet initialized. Because data validation depends on controls being present, data validation does not function in this case. If you need to validate data, for example before saving it or sending it, you must use the `after-controls` process name._

Background options:

* `background`: run only in the background, that is within a service such as the "run form in the background" service
* `foreground`: run only in the foreground, that is when the user is interacting with the page
* `*`: run in both cases

Mode options:

* `new`: run in "new" mode only
* `edit`: run in "edit" mode only
* etc.
* `*`: run in all modes

_NOTE: When running in the background, only the `new` and `edit` modes are supported._

### Warning the user when data is unsafe <a href="#warning-the-user-when-data-is-unsafe" id="warning-the-user-when-data-is-unsafe"></a>

When data is _unsafe_, meaning that is has been modified but not saved yet, Form Runner by default shows a warning when attempting to navigate away from the current page or to close the current browser tab or window.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LPcE34SsQKZNyuHx1sM%252F-LPcEBRtt44POZiqUWHf%252Fchrome-leave-site.png%3Fgeneration=1540420853900178%26alt=media\&width=300\&dpr=4\&quality=100\&sign=0e097cac348155fa4d0b85e86410c7b11309c2779dd751e8b72a46a31aff51fa)

In some cases, in particular when embedding a form, this can be an inconvenience. The following property allows disabling this behavior.

In addition to `true` or `false`, since the value of the property is an AVT, you can also, when needed, dynamically disable the warning by providing an expression between curly braces. For instance the following would only warn users if the content of the field named `description` has more than 50 characters and if users have made changes to the form since it was loaded:

```
<property 
    as="xs:string" 
    name="oxf.fr.detail.warn-when-data-unsafe.*.*"
    value="{string-length(fr:control-string-value('description')) > 50}"/>
```

See also the `set-data-status` action.

### Initial data <a href="#initial-data" id="initial-data"></a>

When creating a new form (for instance going to the URL `http://localhost:8080/forms/fr/forms/bookshelf/new`), the initial form data (also known as "form instance" or "form instance data") can come from 3 different places:

1. The initial instance provided in the form can be used.
2. The Base64-encoded XML documented `POST`ed to the "new form" URI can be used.
3. A service can be called to get the initial instance.

#### Initial data posted to the New Form page <a href="#initial-data-posted-to-the-new-form-page" id="initial-data-posted-to-the-new-form-page"></a>

The instance provided in the form is used by default and the `POST`ed XML document is used if there actually is an XML document being `POST`ed.

The document can be `POST`ed in two ways:

1. As a direct `POST` of the XML document
2. As an HTML form `POST` parameter called `fr-form-data`

For #2, this behaves as if a browser was submitting an HTML form that looks like the following, with the value of the `fr-form-data` request parameter being the Base64-encoded XML document.:

Copy

```
<form method="post" action="/path/to/new">
    <input type="hidden" name="fr-form-data" value="Base64-encoded XML"/>
</form>
```

You can change this behavior to `POST` data in the latest internal format by specifying the `data-format-version=edge` request parameter. This is useful if you obtained the data from, for example, a `send()` action using `data-format-version = "edge"`.

Use the authorization mechanism for services (see Authorization of pages and services) to enable submitting initial instances to the new page:

* Your external application must provide credentials (e.g. BASIC authorization, a secret token, etc.) when `POST`ing to Form Runner.
* Your authorizer service must validate those credentials.

If `data-format-version=edge` is _not_ specified, then the data `POST`ed is assumed to be in 4.0.0 format.

Whe `POST`ing data as described above, the data can now be incomplete. Say the 4.0.0 format of your form data is:

```
<form>
  <contact>
    <first-name/>
    <last-name/>
    <email/>
    <phone/>
  </contact>
  <message>
    <order-number/>
    <topic/>
    <comments/>
  </message>
</form>
```

Let's say that you just want to pass the `<last-name>` and `<order-number>` comments. You can now just `POST`:

```
<form>
  <contact>
    <last-name>Washington</last-name>
  </contact>
  <message>
    <order-number>3141592</order-number>
  </message>
</form>
```

All other elements are automatically added.

_NOTE: If the `POST`ed data contains extra XML elements in no namespace that are not supported by the form, an error is returned. However, extra XML elements in a custom namespace are allowed._

#### Initial data from service <a href="#initial-data-from-service" id="initial-data-from-service"></a>

With the following properties, you can configure Form Runner to call an HTTP service instead of using the default instance provided as part of the form:

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.new.service.enable.*.*"
    value="false"/>

<property
    as="xs:string"
    name="oxf.fr.detail.new.service.uri.*.*"
    value="/fr/service/custom/my-app/new"/>
```

Set the first property above to `true` to enable this behavior and have the second property point to your service.

The service is called with a `GET` HTTP method.

The service must either:

* return a successful HTTP response containing XML data in the `4.0.0` format for the given form
* return an empty body, in which case no error is produced
* return an error HTTP response or malformed XML response, in which case an error is produced and the form doesn't initialize

The following property defines a space-separated list of request parameters to be passed to the service. Say the new page was invoke with request parameters `foo=42` and `bar=84`, if you set the value of this property to `foo bar`, these two request parameters will be passed along as request parameters to the service. The request parameters can either get to the new page in a `POST` or `GET` request. The service is always called with a `GET`, consequently request parameters will be passed on the URI.

```
<property
    as="xs:string"
    name="oxf.fr.detail.new.service.passing-request-parameters.*.*"
    value="foo bar"/>
```

The `oxf.fr.persistence.*.data-format-version` property does not affect `oxf.fr.detail.new.service.enable` and the data returned by the service must still be in `4.0.0` format in all cases.

Enabling `oxf.fr.detail.new.service.enable` doesn't change the behavior with regard to `POST`ed instance: even if you are calling a service to get the initial instance, the `POST`ed instance will be used when a document is `POST`ed to the corresponding "new form" page.

### View mode <a href="#view-mode" id="view-mode"></a>

#### Buttons on the view page <a href="#buttons-on-the-view-page" id="buttons-on-the-view-page"></a>

You configure which buttons are shown on the view page with the following property:

```
<property
    as="xs:string"
    name="oxf.fr.detail.buttons.view.*.*"
    value="back workflow-edit pdf"/>
```

You can use all the buttons available on the Detail Page. In addition, the following buttons apply:

* `workflow-edit`
  * Label: "Edit"
  * Action: Navigate back to the Detail Page in "edit" mode.

#### Showing alerts in view and PDF modes <a href="#showing-alerts-in-view-and-pdf-modes" id="showing-alerts-in-view-and-pdf-modes"></a>

The following property allows you to show alerts in the `view` and `pdf` modes. By default, the value is `false` and the alerts do not show.

```
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.static-readonly-alert.*.*"
    value="true"/>
```

_NOTE: Alerts show under the fields as usual. Setting this property to `true` doesn't cause the Error Summary to show._

#### Showing hints in view and PDF modes <a href="#showing-hints-in-view-and-pdf-modes" id="showing-hints-in-view-and-pdf-modes"></a>

The following property allows you to show hints in the `view` and `pdf` modes. By default, the value is `false` and the hints do not show.

```
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.static-readonly-hint.*.*"
    value="true"/>
```

#### Calculations in readonly modes <a href="#calculations-in-readonly-modes" id="calculations-in-readonly-modes"></a>

The following property allows disabling Calculated Value formulas in readonly modes (Review, PDF). By default, the value is `false` and the calculations take place.

```
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.readonly.disable-calculate.*.*"
    value="true"/>
```

#### Grid tab order <a href="#grid-tab-order" id="grid-tab-order"></a>

You can configure the tab order in grids with the following property:

```
<property 
    as="xs:string" 
    name="oxf.xforms.xbl.fr.grid.tab-order.*.*"
    value="columns"/>
```

Valid values are `rows` and `columns`.
