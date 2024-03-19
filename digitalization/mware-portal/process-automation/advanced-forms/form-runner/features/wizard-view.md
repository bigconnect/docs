# Wizard view

By default, with Form Runner all the form sections appear in the same page, on top of each other. If your form is large that means that you have to scroll to fill out the entire form.

With the wizard view, top-level sections instead appear in a table of contents area to the left, and only a single top-level section is shown at any given time in a separate wizard "page":

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC5XV1UwSE5VwdY1q%252Fwizard.png%3Fgeneration=1528743371052540%26alt=media\&width=768\&dpr=4\&quality=100\&sign=de40c568a93874df70aaba812215bcae30cc8250d0bd3a073211e0335a910bd1)

Form Runner Wizard

You can navigate between pages by clicking on a title in the table of contents, or you can use the navigation arrows. You can also use "Prev" and "Next" buttons when configured.

Errors on your form appear at the bottom as usual, and the title of pages that contain errors are highlighted in red. If you click on an error you are taken directly to the page and control containing the error.

### Enabling the wizard view <a href="#enabling-the-wizard-view" id="enabling-the-wizard-view"></a>

#### Using a property <a href="#using-a-property" id="using-a-property"></a>

The wizard view is optional - you can use the regular view instead, and you can enable this view per form, per app, or globally with a property:

Copy

```
<property
  as="xs:string"
  name="oxf.fr.detail.view.appearance.*.*"
  value="wizard"/>
```

#### From Form Builder <a href="#from-form-builder" id="from-form-builder"></a>

\[SINCE Orbeon Forms 2016.2]

You can enable or disable the wizard view for a specific form definition directly from Form Builder's Form Settings dialog, under Form Options:

* **Use Default:** use the configuration specified with the `oxf.fr.detail.view.appearance` property
* **Always:** enable the wizard view no matter what the `oxf.fr.detail.view.appearance` property specifies
* **Never:** disable the wizard view no matter what the `oxf.fr.detail.view.appearance` property specifies

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-dcbcbf7848fd0928c4ae17ec8ba7f4eacc3cd610%252Fform-settings-view-options.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=f73cdbaffb7ac625802ac3b7fe5b0e72d63424e4ce6781a7cef6471a680d1ae7)

### Modes <a href="#modes" id="modes"></a>

#### Introduction <a href="#introduction-1" id="introduction-1"></a>

The wizard supports two mode:

* the _free_ mode (which is the default mode)
* the _validated_ mode \[SINCE Orbeon Forms 4.9]

#### Free mode <a href="#free-mode" id="free-mode"></a>

When using the free mode, you can freely:

* go back to the preceding page
* go forward to the next page
* change page from the table of contents
* leaving a page marks all fields on the given page as visited, ensuring that errors on that page, if any, show in the error summary

#### Validated mode <a href="#validated-mode" id="validated-mode"></a>

**Lax validated mode**

\[SINCE Orbeon Forms 4.9]

When using the validated mode:

* you can freely go back to the preceding page
* but you can only go forward to the next page if
  * there are no errors on all preceding pages as well as the current page
  * if you have already visited the next page
* the table of contents only allows you to navigate
  * to pages you have already visited
  * or to the next page if there are no errors on all preceding pages as well as the current page \[SINCE Orbeon Forms 2016.1]
* you should generally use the "Prev" or "Next" buttons for navigation
* any attempt to navigate to the next page marks all the fields of the preceding pages as well as the current page as visited, ensuring that errors on those pages, if any, show in the error summary

**NOTE: This only applies to navigation between top-level sections. When subsection navigation is enabled, the validated mode applies only to top-level sections, while navigation within a given top-level section is always free.**

\[FROM Orbeon Forms Forms 4.9 to 2016.2]

The following property enables the lax validated mode:

Copy

```
<property
  as="xs:boolean"
  name="oxf.xforms.xbl.fr.wizard.validate.*.*"
  value="true"/>
```

\[SINCE Orbeon Forms 2016.3]

The following property enables the lax validated mode:

Copy

```
<property
  as="xs:string"
  name="oxf.xforms.xbl.fr.wizard.validate.*.*"
  value="lax"/>
```

Setting the value to `true` is still supported for backward compatibility.

You can see in the following picture that the sections in the table of contents which have not yet been visited are grayed out:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC5XyyzncidTao1mS%252Fwizard-validated.png%3Fgeneration=1528743371004796%26alt=media\&width=768\&dpr=4\&quality=100\&sign=655f90d82c242be4317b0629026148f036d2d3aa90fc88d4c92084bdc144058a)

Wizard validated mode

**Strict validated mode**

\[SINCE Orbeon Forms 2016.3]

The following property enables the strict validated mode:

Copy

```
<property
  as="xs:string"
  name="oxf.xforms.xbl.fr.wizard.validate.*.*"
  value="strict"/>
```

This mode behaves the same as the lax validated mode, except that it doesn't matter if a forward section has been visited or not: if there are errors in the current or preceding wizard pages, it is not possible to navigate with the "Next" button or via the table of contents.

**NOTE: This only applies to navigation between top-level sections. When subsection navigation is enabled, the validated mode applies only to top-level sections, while navigation within a given top-level section is always free.**

### Buttons <a href="#buttons" id="buttons"></a>

#### Inner buttons <a href="#inner-buttons" id="inner-buttons"></a>

The following property allows specifying which buttons are presented _inside_ the wizard, as opposed to the bottom of the page. This example places the wizard's "Prev" and "Next" buttons just under the current wizard section:

Copy

```
<property
    as="xs:string"
    name="oxf.fr.detail.buttons.inner.*.*"
    value="wizard-prev wizard-next"/>
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC5YNb8n1y8GwW4S5%252Fwizard-buttons.png%3Fgeneration=1528743360932303%26alt=media\&width=768\&dpr=4\&quality=100\&sign=3414cbb03b484a4e789c238c40e28bb6efc16ede82d9feb9b8d462454e0611d3)

### Separate table of contents <a href="#separate-table-of-contents" id="separate-table-of-contents"></a>

\[SINCE Orbeon Forms 2016.2]

When set to `true`, the following property enables showing the table of contents separately, as if on a different page:

Copy

```
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.wizard.separate-toc.*.*"
    value="true"/>
```

When `false` (the default):

* the table of contents appears to the left (wide desktop layout) or the top (narrow mobile layout)
* the section content appears to the right (wide desktop layout) or under the table of contents (narrow mobile layout)

When `true`:

* the table of contents takes the entire width when landing on the page
* when selecting a section, the view toggles to the selected section (top-level or subsection depending on whether subsection navigation is enabled)

\[SINCE Orbeon Forms 2022.1]

The Form Settings in Form Builder allow overriding the default set by configuration properties at the form level.

### Section status <a href="#section-status" id="section-status"></a>

#### Status indication <a href="#status-indication" id="status-indication"></a>

\[SINCE Orbeon Forms 2016.2]

When enabling the separate table of contents of the wizard (see above), the wizard automatically indicates, the status of each section:

* **Not Started:** the user hasn't visited the section yet.
* **Incomplete:** the user has visited the section but some required fields are not filled.
* **Errors:** the user has visited the section and some fields have been filled but contain errors.
* **Complete:** the user has visited the section and all the required fields for the section have been filled.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LLk8nTblBXGwDLz\_2WS%252F-LEkC5YYaWTnaIS3BrVT%252Fwizard-status.png%3Fgeneration=1536258717509042%26alt=media\&width=768\&dpr=4\&quality=100\&sign=045f06adfa2dbf92a576e770d0d9e21f87015024bf1f0f7b2d203b7207b438c1)

Wizard validated mode

\[SINCE Orbeon Forms 2022.1]

By default, the status of each section is not shown in the regular (not separate) wizard table of contents. Setting the `section-status` property to `true` allows changing this default:

Copy

```
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.wizard.section-status.*.*"
    value="true"/>
```

The Form Settings in Form Builder allow overriding the default set by configuration properties at the form level.

#### CSS classes <a href="#css-classes" id="css-classes"></a>

CSS classes are available on table of content entries to reflect the status of each section:

* `disabled`
* `active`
* `started`
* `changed`
* `incomplete`
* `invalid`

\[SINCE Orbeon Forms 2018.1]

The following classes indicate the first and last sections:

* `first-page`: the first visible page
* `last-top-level-page`: the first visible top-level page
* `last-page`: the last visible page
  * this can be different from `last-top-level-page` when using subsection navigation

#### Wizard status persistence <a href="#wizard-status-persistence" id="wizard-status-persistence"></a>

\[SINCE Orbeon Forms 2017.2]

The wizard status is persisted when the data is saved to the database. This means that if the user saves incomplete data and comes back to it, information about visited or changed wizard pages is restored.

The wizard shows the last possible page upon loading data.

### Subsections <a href="#subsections" id="subsections"></a>

#### Subsections navigation <a href="#subsections-navigation" id="subsections-navigation"></a>

\[SINCE Orbeon Forms 2016.2]

Copy

```
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.wizard.subsections-nav.*.*"
    value="true"/>
```

When `false` (the default):

* the wizard allows you to navigate only between top-level sections

When `true`:

* the wizard navigates through the first level of subsections when present
* it shows one first level of subsections at a time, or one top-level section at a time for those top-level sections which don't have any subsections
* second-level and deeper levels of subsections appear on the same wizard page
* the "Next" and "Prev" buttons go to the next or previous first-level subsection if any, and then to the next or previous top-level section's first subsection if any
* the validated mode, if enabled, still applies only to top-level sections, which means that
  * it is possible to freely navigate through subsections within a given top-level section
  * however when attempting to navigate to the next top-level section, validation constraints apply

_NOTE: Grids directly nested within top-level sections which have any subsections are ignored in this mode. The recommendation is to avoid this situation when using subsections navigation and not place grids directly at the same level of first-level subsections. This restriction is lifted in Orbeon Forms 2016.3._

_NOTE: Top-level repeated sections are not supported. The recommendation is to avoid this situation when using subsections navigation and not use top-level repeated sections. This is improved in Orbeon Forms 2016.3 where top-level repeated sections disable subsection navigation for themselves._

\[SINCE Orbeon Forms 2016.3]

Grids directly nested within top-level sections which have any subsections are also part of the navigation. Each grid is navigated independently from the other grids.

The `subsections-nav` attribute on top-level `fr:section` elements can be used to explicitly disable subsection navigation. See [Wizard Component](https://doc.orbeon.com/form-runner/component/wizard#the-subsections-nav-attribute).

#### Visibility in the table of contents <a href="#visibility-in-the-table-of-contents" id="visibility-in-the-table-of-contents"></a>

\[SINCE Orbeon Forms 2016.2]

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.wizard.subsections-toc.*.*"
    value="active"/>
```

In all cases, the table of contents shows all visible top-level sections.

When `active` (the default and only behavior until 2016.1):

* the table of contents shows visible subsections only for the currently-visible top-level section

When `all`:

* the table of contents shows all visible subsections

When `none`:

* the table of contents doesn't show any subsection

### Paging large repeated sections <a href="#paging-large-repeated-sections" id="paging-large-repeated-sections"></a>

By default, top-level repeated sections appear as a single entry in the table of contents. The section's repeated content appears within the section, each repetition on top of the other, as is the case when the wizard is not used.

\[SINCE Orbeon Forms 2019.1]

It is now possible to make each iteration of a top-level repeated section that is the target of [synchronized repeated content](https://doc.orbeon.com/form-builder/advanced/services-and-actions/synchronize-repeated-content) appear as a separate entry in the table of contents. The benefit of this is that, if the repeated section content is large, then less content appears on screen at the same time. This is better for usability and for performance.

To enable this, go to the relevant [section repeat settings](https://doc.orbeon.com/form-builder/form-editor/repeat-settings), and:

* Uncheck "Allow the user to add, remove, or move repetitions"
* Check "Show one repetition at a time in the wizard"

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-b7d77ca501bc2c8f1534d2a1209b59575735692a%252Fcontainer-settings-repeated-content-one-repetition.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=92cf28179fa37aa707043b74eb067842c45d7a5faca05261b0e7b58e02ff1fb0)

Repeated Content Configuration

In addition, each repetition can have an individual [repetition label](https://doc.orbeon.com/form-builder/form-editor/section-settings#dynamic-iteration-label).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-afc9df258a1e7e3970894a38d43442a4fd992f06%252Fsection-settings-repetition-label.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=08b9e38593ac82b010d985fdeafddbc6cdbab0c0ff904ad716e69f94a684a44c)

### Disabling full update <a href="#disabling-full-update" id="disabling-full-update"></a>

\[SINCE Orbeon Forms 2022.1]

On the browser, by default, only the HTML for the page of the wizard currently being displayed is present in the DOM. When users switch from page A to page B, the HTML for page B is loaded and replaces in the DOM the HTML for page A. This makes the initial load of the form faster, as the HTML for just a single page is loaded, and the browser can then process the form more efficiently, having to deal with a much smaller DOM.

However, should you want to disable this behavior, and have the HTML for all the pages loaded at once, maybe because you have custom JavaScript for which it is convenient to access control values outside the current page, you can do so by setting the following property:

Copy

```
<property 
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.wizard.full-update.*.*"                   
    value="false"/>
```

### Page name URL parameter <a href="#page-name-url-parameter" id="page-name-url-parameter"></a>

[\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)

The URL of the form now contains a `fr-wizard-page` parameter, which indicates the page currently being displayed. For example:

Copy

```
/fr/orbeon/controls/new?fr-wizard-page=selection-controls
```

Form Runner also reads this URL parameter to attempt initializing the wizard to the requested page. This is useful when you want to bookmark a specific page of the wizard, or when you want to send a link to a specific page of the wizard to someone else.

Requesting a specific wizard page may fail in case the page is non-existent or not accessible to the current user. In particular, this is the case for non-available pages in validated modes. In that case, the wizard displays the first available page of the wizard as if the parameter had not been provided.

When using paged repeated sections, the URL parameter indicates the repetition number, and requesting that URL parameter value shows the given repetition if it exists and is available. If the page name doesn't correspond to a repeated section, or if the index requested is out of bounds, the wizard displays the first available page of the wizard as if the parameter had not been provided. The index is 1-based and encoded after an encoded `/` character (`%2F`), for example:

Copy

```
/fr/orbeon/travel/edit/28431b9fac9ba642a6bf8408a513538a97273483
    ?fr-wizard-page=detail-section%2F2
```

It is possible to disable this behavior by setting the following property to `false`:

Copy

```
<property 
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.wizard.allow-url-param.*.*"                   
    value="false"/>
```

It is possible to read the current wizard page name using the [`fr:wizard-current-page-name()`](https://doc.orbeon.com/xforms/xpath/extension-functions/extension-form-runner#fr-wizard-current-page-name) function. This can be used for example whe calling a process from a button to pass the current wizard page name as a parameter to the service URL.
