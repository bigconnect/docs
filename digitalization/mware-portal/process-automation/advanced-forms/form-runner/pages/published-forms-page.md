# Published Forms page

The Form Runner Published Forms page allows you to access your deployed forms.

You access the Form Runner Published Forms page by adding `fr/forms` to the path on which you deployed Advanced Forms. If you deployed Advanced Forms on `http://www.example.com/forms/`, then you can access the Published Forms page at `http://www.example.com/forms/fr/forms`.

This page is also accessible directly from the Landing page.

<figure><img src="../../../../../../.gitbook/assets/image (6).jpg" alt=""><figcaption><p>The Published Forms page</p></figcaption></figure>

### Availability and compatibility <a href="#availability-and-compatibility" id="availability-and-compatibility"></a>

The Home page is replaced with separate Published Forms and Forms Admin pages at paths `/fr/forms` and `/fr/admin`. The `/fr/` path now reaches the Landing page.

### Permissions <a href="#permissions" id="permissions"></a>

Depending on your permissions, only the subset of the forms that you have access to is shown.

### Published forms <a href="#published-forms" id="published-forms"></a>

The page shows a table with the following data:

* Each line shows only forms that are _published and available_.
* For each form, the app name, form name and title in the current language are shown.
* The form version is also shown.
* You can navigate to the form's Summary, New or View page, depending on permissions, by clicking on a line.
* Forms are sorted by last modification time.
* Libraries are never shown in this view.

_NOTE: Only deployed forms are visible. Forms that have been created with Form Builder and which have been just saved but never deployed are not visible._

### Configuration properties <a href="#configuration-properties" id="configuration-properties"></a>

#### Page size <a href="#page-size" id="page-size"></a>

The number of forms shown on a given page can be set with the following property:

```
<property
    as="xs:integer"
    name="oxf.fr.home.page-size"
    value="20"/>
```

#### Link to the summary or new page <a href="#link-to-the-summary-or-new-page" id="link-to-the-summary-or-new-page"></a>

The table listing the forms links, for each form, either to the `summary` or the `new` page, based on which page the user has access to. If the user has access to both, then it links to the `summary` page, in essence giving the priority to the `summary` page. You can also change this priority by setting the value of the following property, of which the default is shown below.

```
<property
    as="xs:string"
    name="oxf.fr.home.table.link-to"
    value="summary new"/>
```

For instance, if you set it to `new summary`, the priority will be given to the `new` instead of the `summary` page. If you list only one page, say `new`, then entries in the table will only link to the `new` page, of course if the user has access to it. If you leave the value blank, the forms will only be listed, with no link.

