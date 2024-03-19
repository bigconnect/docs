# Published Forms page

The Form Runner Published Forms page allows you to access your deployed forms.

You access the Form Runner Published Forms page by adding `fr/forms` to the path on which you deployed Orbeon Forms. If you deployed Orbeon Forms on `http://www.example.com/orbeon/`, then you can access the Published Forms page at `http://www.example.com/orbeon/fr/forms`.

This page is also accessible directly from the [Landing page](https://github.com/orbeon/orbeon-forms-doc/blob/master/form-runner/feature/landing-page.md).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-7582e267255409c8f4924ac89d8ad8cb919d9d99%252Fform-runner-forms.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=85d6ff23810009f628374b987c74371d62090f2093467145c5794d4b39d89aa6)

The Published Forms page

### Availability and compatibility <a href="#availability-and-compatibility" id="availability-and-compatibility"></a>

\[SINCE Orbeon Forms 2022.1]

Until Orbeon Forms 2021.1, this page was combined with the [Forms Admin page](https://doc.orbeon.com/form-runner/pages/forms-admin-page) under the name [Home page](https://github.com/orbeon/orbeon-forms-doc/blob/master/form-runner/feature/home-page.md) and accessible at the path `/fr/`. Starting with Orbeon Forms 2022.1, the Home page is replaced with separate Published Forms and Forms Admin pages at paths `/fr/forms` and `/fr/admin`. The `/fr/` path now reaches the [Landing page](https://github.com/orbeon/orbeon-forms-doc/blob/master/form-runner/feature/landing-page.md).

### Permissions <a href="#permissions" id="permissions"></a>

Depending on your [permissions](https://doc.orbeon.com/form-runner/access-control/deployed-forms), only the subset of the forms that you have access to is shown.

### Published forms <a href="#published-forms" id="published-forms"></a>

The page shows a table with the following data:

* Each line shows only forms that are _published and available_.
* For each form, the app name, form name and title in the current language are shown.
* The form version is also shown. \[SINCE Orbeon Forms 2020.1]
* You can navigate to the form's Summary, New or View page, depending on permissions, by clicking on a line.
* Forms are sorted by last modification time.
* Libraries are never shown in this view.

_NOTE: Only deployed forms are visible. Forms that have been created with Form Builder and which have been just saved but never deployed are not visible._

### Configuration properties <a href="#configuration-properties" id="configuration-properties"></a>

#### Page size <a href="#page-size" id="page-size"></a>

The number of forms shown on a given page can be set with the following property:

Copy

```
<property
    as="xs:integer"
    name="oxf.fr.home.page-size"
    value="20"/>
```

#### Link to the summary or new page <a href="#link-to-the-summary-or-new-page" id="link-to-the-summary-or-new-page"></a>

The table listing the forms links, for each form, either to the `summary` or the `new` page, based on which page the user has access to. If the user has access to both, then it links to the `summary` page, in essence giving the priority to the `summary` page. \[SINCE Orbeon Forms 2017.2] You can also change this priority by setting the value of the following property, of which the default is shown below.

Copy

```
<property
    as="xs:string"
    name="oxf.fr.home.table.link-to"
    value="summary new"/>
```

For instance, if you set it to `new summary`, the priority will be given to the `new` instead of the `summary` page. If you list only one page, say `new`, then entries in the table will only link to the `new` page, of course if the user has access to it. If you leave the value blank, the forms will only be listed, with no link.

### Orbeon Forms 4.0 to 4.2 <a href="#orbeon-forms-4.0-to-4.2" id="orbeon-forms-4.0-to-4.2"></a>

For each form definition the current user has access to, the following links are shown if allowed:

* Link to the summary page: shown if the current user can perform either one of the read, update, or delete operations on the form.
* Link to the new page: shown if the current user can perform the create operation on the form.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yUZV11htl\_U0ws%252Fhome.png%3Fgeneration=1528743371039105%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1b7fa1e5193e3cd71ef3ba8ebd489d2543706c745b9c56fb5127f3647c53f9b3)

Published Forms page
