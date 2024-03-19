# Form Runner

### Default values <a href="#default-values" id="default-values"></a>

For the latest default values of Form Runner properties, see `properties-form-runner.xml`.

### Language <a href="#language" id="language"></a>

#### Default language <a href="#default-language" id="default-language"></a>

The following property determines Form Runner's default language:

Copy

```
<property
    as="xs:string"
    name="oxf.fr.default-language.*.*"
    value="en">
```

When wildcards are specified, this property can control the default language for a given app or form.

The property without wildcards can also be used to control the default language of pages which don't involve a specific form, such as the Form Runner Home Page

Copy

```
<property
    as="xs:string"
    name="oxf.fr.default-language"
    value="en">
```

For more details, see [Language selection at runtime](https://doc.orbeon.com/form-runner/features/localization#language-selection-at-runtime)

#### Available languages <a href="#available-languages" id="available-languages"></a>

For a given form, you can filter which languages are available in the language selector with a space-separated list of language codes:

Copy

```
<property
  as="xs:string"
  name="oxf.fr.available-languages.*.*"
  value="en fr"/>
```

The language selector by default shows all languages available in the form definition. When this property is specified, only the intersection of the languages is shown in the selector. For example:

* Example 1
  * form languages: `en fr jp`
  * property: `en fr`
  * resulting languages: `en fr`
* Example 2
  * form languages: `en fr jp`
  * property: `en jp kr`
  * resulting languages: `en jp`

If the property is blank or contains the wildcard `*`, all the form languages are available.

\[SINCE Orbeon Forms 4.3]

For pages which don't involve a specific form, such as the Form Runner Home Page, the following property controls the available languages:

Copy

```
<property
  as="xs:string"
  name="oxf.fr.available-languages"
  value="en fr"/>
```

For more details, see [Language selection at runtime](https://doc.orbeon.com/form-runner/features/localization#language-selection-at-runtime)

### Timezone <a href="#timezone" id="timezone"></a>

\[SINCE Orbeon Forms 2021.1]

The Summary and Home pages display timestamps showing the created and last modified dates of forms and data.

You can configure an explicit timezone to display these timestamps:

Copy

```
<property
      as="xs:string"
      name="oxf.fr.default-timezone"
      value="America/Los_Angeles"/>
```

Copy

```
<property
      as="xs:string"
      name="oxf.fr.default-timezone"
      value="Asia/Kolkata"/>
```

If the property is blank (the default), the Java environment's default timezone is used.

\[UNTIL Orbeon Forms 2020.1]

The Java environment's default timezone is used.

### Summary Page <a href="#summary-page" id="summary-page"></a>

See [Summary page configuration properties](https://doc.orbeon.com/configuration/properties/form-runner/form-runner-summary-page).

### Detail page <a href="#detail-page" id="detail-page"></a>

See [Detail page configuration properties](https://doc.orbeon.com/configuration/properties/form-runner/form-runner-detail-page).

### Show Orbeon Forms version <a href="#show-orbeon-forms-version" id="show-orbeon-forms-version"></a>

\[UNTIL Orbeon Forms 4.6, use `oxf.show-version` starting Orbeon Forms 4.6.1]

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.version.*.*"
    value="true"/>
```

Whether to show the Orbeon Forms version at the bottom.

### Default logo <a href="#default-logo" id="default-logo"></a>

With this property, you can set the default logo URI. This logo appears on the Summary, Detail and Home pages for a given form. You can omit (or comment out) this property or set its value to the empty string if you don't want a default logo at all.

This is the default value of the property:

Copy

```
<property as="xs:anyURI"  name="oxf.fr.default-logo.uri.*.*">
    /apps/fr/style/orbeon-navbar-logo.png
</property>
```

If you use two `*` wildcards, as in the example above, the property also sets the logo on the [Published Forms page](https://doc.orbeon.com/form-runner/pages/published-forms-page) and the [Forms Admin page](https://doc.orbeon.com/form-runner/pages/forms-admin-page)

1. Place your logo file(s) under one of the following recommended locations:
   * `WEB-INF/resources/forms/assets`: logo for all forms
   * `WEB-INF/resources/forms/APP/assets`: logo for app name APP
   * `WEB-INF/resources/forms/APP/FORM/assets`: logo for app name APP and form name FORM
2. Define the `oxf.fr.default-logo.uri.*.*` property to point to the file(s) you added. The path points to location under the `WEB-INF/resources` directory.

For example, to change the default logo for all forms and pages to an image called `my-logo.png`, place the image at the proper location and use the following property:

Copy

```
<property as="xs:anyURI"  name="oxf.fr.default-logo.uri.*.*">
    /forms/assets/my-logo.png
</property>
```

NOTE: Since Orbeon Forms 4.0, this property doesn't have an impact on the Form Builder logo. To override the Form Builder logo, you can use custom CSS (see the [`oxf.fr.css.custom.uri`](https://github.com/orbeon/orbeon-forms-doc/blob/master/form-runner/styling/css/README.md#adding-your-own-css-files) configuration).

### Adding your own CSS <a href="#adding-your-own-css" id="adding-your-own-css"></a>

See the [CSS page](https://doc.orbeon.com/form-runner/styling/css).

### Adding your own JavaScript <a href="#adding-your-own-javascript" id="adding-your-own-javascript"></a>

\[SINCE Orbeon Forms 4.4]

1. Place your JavaScript file(s) under one of the following recommended locations:
   * `WEB-INF/resources/forms/assets`: scripts for all forms
   * `WEB-INF/resources/forms/APP/assets`: scripts for app name APP
   * `WEB-INF/resources/forms/APP/FORM/assets`: scripts for app name APP and form name FORM
2. Define the [`oxf.fr.js.custom.uri`](https://doc.orbeon.com/configuration/properties/form-runner#adding-your-own-adding-your-own-javascript-files) property to point to the file(s) you added. The path points to location under the `WEB-INF/resources` directory.

Copy

```
<property as="xs:string" name="oxf.fr.js.custom.uri.*.*">
    /forms/acme/assets/acme.js
    /forms/acme/sales/assets/acme-sales.js
</property>
```

You can add more than one file, and just separate the paths by whitespace in the property.

\[SINCE Orbeon Forms 2017.1]

In addition to [`oxf.fr.js.custom.uri`](https://doc.orbeon.com/configuration/properties/form-runner#adding-your-own-adding-your-own-javascript-files), you can also use the following properties, which apply only to the Summary and Detail pages respectively:

* [`oxf.fr.summary.js.custom.uri`](https://doc.orbeon.com/configuration/properties/form-runner/form-runner-summary-page#adding-your-own-javascript-files)
* [`oxf.fr.detail.js.custom.uri`](https://doc.orbeon.com/configuration/properties/form-runner/form-runner-detail-page#adding-your-own-javascript-files)

### Overriding resources <a href="#overriding-resources" id="overriding-resources"></a>

In some cases, it might make sense to change some of the resources provided out of the box by Form Runner. For instance, the Detail Page can have a submit button, which in English has a label "Submit". For your application, another label might make more sense, for instance "Send". To override Form Runner resources, you define properties with a name that has the following structure:

1. The name start with `oxf.fr.resource`.
2. Followed by the name of the application and form name for which you want to redefine the resource. You can use `*` for either if you want the redefinition to apply to all the applications or all the forms. For instance: `*.*`, or `my-app.my-form`.
3. The 2-letter code for the language for which you want to override the resource. For instance: `en`.
4. A dot-separated path corresponding to the path of the resource you want to override as defined by Form Runner [`resources.xml`](https://github.com/orbeon/orbeon-forms/blob/master/form-runner/jvm/src/main/resources/apps/fr/i18n/resources.xml).
5. Resources are aggressively caches, so you need to restart your application server (or redeploy the web app) after changing a property that overrides resources.

For instance, to change the label of the submit button to be "Send" in English for all applications and forms, write:

Copy

```
<property
    as="xs:string"
    name="oxf.fr.resource.*.*.en.detail.buttons.send"
    value='&lt;i class="icon-arrow-right"/&gt; Send'/>
```

This also works for resources that don't exist yet. For your own resources, start with a prefix specific to your company or project. For example:

Copy

```
<property
  as="xs:string"
  name="oxf.fr.resource.*.*.en.acme.my-resource-1"
  value="Resource 1 in English"/>

<property
  as="xs:string"
  name="oxf.fr.resource.*.*.fr.acme.my-resource-1"
  value="Resource 1 en français"/>
```

_NOTE: You can add new resources as shown above, but you cannot create new languages._

### Email settings <a href="#email-settings" id="email-settings"></a>

See [Email](https://doc.orbeon.com/configuration/properties/form-runner/form-runner-detail-page/form-runner-email).

### Sections and grids <a href="#sections-and-grids" id="sections-and-grids"></a>

#### Appearance of repeated sections <a href="#appearance-of-repeated-sections" id="appearance-of-repeated-sections"></a>

\[SINCE Orbeon Forms 2016.1]

The following property allows you to set the appearance of repeated sections to `full` (the default) or `minimal` for all forms or for a subset of forms:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.section.appearance.*.*"
    value="minimal"/>
```

See also the [`appearance`](https://doc.orbeon.com/form-runner/component/section#repeated-mode) attribute of the [section component](https://doc.orbeon.com/form-runner/component/section).

#### Appearance of repeated grids <a href="#appearance-of-repeated-grids" id="appearance-of-repeated-grids"></a>

\[SINCE Orbeon Forms 2016.1]

The following property allows you to set the appearance of repeated grids to `full` (the default) or `minimal` for all forms or for a subset of forms:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.grid.appearance.*.*"
    value="minimal"/>
```

See also the [`appearance`](https://doc.orbeon.com/form-runner/component/grid#repeated-mode) attribute of the [grid component](https://doc.orbeon.com/form-runner/component/grid).

#### Insert position of repeated sections <a href="#insert-position-of-repeated-sections" id="insert-position-of-repeated-sections"></a>

\[SINCE Orbeon Forms 2016.2]

The following property allows you to select where new repetitions are added when using the "Add Another" or "+" button. Allowed values are `index` (default for the `full` appearance) and `bottom` (default for the `minimal` appearance):

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.section.insert.*.*"
    value="index"/>
```

See also the [`insert`](https://doc.orbeon.com/form-runner/component/section#repeated-mode) attribute of the [section component](https://doc.orbeon.com/form-runner/component/section).

#### Insert position of repeated grids <a href="#insert-position-of-repeated-grids" id="insert-position-of-repeated-grids"></a>

\[SINCE Orbeon Forms 2016.2]

The following property allows you to select where new repetitions are added when using the "Add Another" or "+" button. Allowed values are `index` (default for the `full` appearance) and `bottom` (default for the `minimal` appearance):

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.grid.insert.*.*"
    value="index"/>
```

See also the [`insert`](https://doc.orbeon.com/form-runner/component/grid#repeated-mode) attribute of the [grid component](https://doc.orbeon.com/form-runner/component/grid).

#### Section collapsing <a href="#section-collapsing" id="section-collapsing"></a>

\[SINCE Orbeon Forms 2016.1]

The following property allows you to set whether a section content can be collapsed by clicking on its title for all forms or for a subset of forms:

Copy

```
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.section.collapsible.*.*"
    value="false"/>
```

By default, sections are allowed to collapse.

The following property controls the same behavior in noscript mode:

\[UNTIL Orbeon Forms 2018.1]

Copy

```
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.section.noscript.collapsible.*.*"
    value="false"/>
```

A value of `false` may make sections more accessible and less confusing to screen reader users.

The following property controls the whether collapsing/opening of sections uses an animation. The default is `true`:

Copy

```
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.section.animate.*.*"
    value="false"/>
```

A value of `false` can be more efficient with slower browsers or large forms.

#### Grid markup <a href="#grid-markup" id="grid-markup"></a>

At runtime, grids in your forms are rendered using HTML tables, with elements such as `table`, `tr`, and `td`, this instead of using the more modern [CSS grid layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS\_Grid\_Layout) (with `div` elements). This is because older browsers, in particular IE11, only provides limited support for CSS grids.

\[SINCE Orbeon Forms 2020.1.7 and 2021.1.1] As support for IE11 will be dropped in future versions of Orbeon Forms, we anticipate that Orbeon Forms will by default use the CSS grids layout instead of HTML tables. You can already make this choice today by adding the following property:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.grid.markup.*.*"
    value="css-grid"/>
```

#### Deprecated properties <a href="#deprecated-properties" id="deprecated-properties"></a>

Before Orbeon Forms 2016.1, you could use the following properties, deprecated since Orbeon Forms 2016.1. Section collapsing:

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.ajax.section.collapse.*.*"
    value="false"/>
```

Section collapsing in noscript mode:

\[UNTIL Orbeon Forms 2018.1]

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.noscript.section.collapse.*.*"
    value="false"/>
```

Section collapsing animation:

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.ajax.section.animate.*.*"
    value="true"/>
```

### Noscript properties <a href="#noscript-properties" id="noscript-properties"></a>

\[DEPRECATED SINCE Orbeon Forms 2016.3]

\[UNTIL Orbeon Forms 2018.1]

#### Show noscript link <a href="#show-noscript-link" id="show-noscript-link"></a>

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.noscript-link.*.*"
    value="true"/>
```

Whether to show the link to the noscript/full version.

#### Noscript: use table layout <a href="#noscript-use-table-layout" id="noscript-use-table-layout"></a>

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.noscript.table.*.*"
    value="true"/>
```

Whether forms in noscript mode are allowed to use a layout based on tables. If `false`, no tables are used. WYSIWYG is lost, but the form may be more accessible. The default is `true`.
