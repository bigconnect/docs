# Flat view

If you're using Oracle, SQL Server, or DB2 or PostgreSQL, when you deploy a form created in Form Builder, Advanced Forms can create a form-specific view of your data, with one column for each form field.

#### Property to enable <a href="#property-to-enable" id="property-to-enable"></a>

You enable this feature by setting the relevant property listed below to `true`.

| Database   | Property                                         |
| ---------- | ------------------------------------------------ |
| Oracle     | `oxf.fr.persistence.oracle.create-flat-view`     |
| SQL Server | `oxf.fr.persistence.sqlserver.create-flat-view`  |
| DB2        | `oxf.fr.persistence.db2.create-flat-view`        |
| PostgreSQL | `oxf.fr.persistence.postgresql.create-flat-view` |

For instance, if using Oracle, you set:

```
<property
    as="xs:boolean"
    name="oxf.fr.persistence.oracle.create-flat-view"
    value="true"/>
```

### Generated names <a href="#generated-names" id="generated-names"></a>

#### View name <a href="#view-name" id="view-name"></a>

When you enable this property, upon publishing a form, the persistence layer creates a view specific to that form. The name of the view is based on the app name, form name, and form version. It has the form:

```
forms_f_#{app}_#{form}_#{form_version}
```

If your app is `hr`, your form is `expense`, and you are publishing version 2 of that form, then the view is named `forms_f_hr_expense_2`. If upon publishing, there is already a view with that name, the persistence layer deletes it before recreating a new view.

#### Metadata column names <a href="#metadata-column-names" id="metadata-column-names"></a>

The view always has the following metadata columns, with information copied from the equivalent columns in `forms_form_data`:

* `metadata_document_id`
* `metadata_created`
* `metadata_last_modified_time`
* `metadata_last_modified_by`

Note that there is no `metadata_draft` column, as drafts are not included the view.

#### Data column names <a href="#data-column-names" id="data-column-names"></a>

In addition to those columns, you have one column per form field, and each column is named by combining the section name with the control name. On some databases, like Oracle, columns names are limited to 30 characters, so the persistence layer truncates column names. It also converts dashes to underscores, removes any non alphanumeric character except inner underscores, and converts the name to uppercase (so it can be used in queries without quotes).

Names are not cut short unnecessarily, and A numerical _suffix_ is used instead for those columns which would introduce duplicates.

Examples:

* Section name: `personal-information`
  * Control name: `first-name` ⇒ column name: `PERSONAL_INFORMATIO_FIRST_NAME`
  * Control name: `last-name` ⇒ column name: `PERSONAL_INFORMATION_LAST_NAME`
  * Control name: `address` ⇒ column name: `PERSONAL_INFORMATION_ADDRESS`
* Section name: `company`
  * Control name: `name` ⇒ column name: `COMPANY_NAME`
  * Control name: `industry` ⇒ column name: `COMPANY_INDUSTRY`
* Section name: `section-with-long-name`
  * Control name: `my-control-with-a-pretty-long-name` ⇒ column name: `SECTION_WITH_L_MY_CONTROL_WITH`
  * Control name: `my-control-with-a-pretty-long-name-too` ⇒ column name: `SECTION_WITH_L_MY_CONTROL_WIT1`
  * Control name: `my-control-with-a-pretty-long-name-really` ⇒ column name: `SECTION_WITH_L_MY_CONTROL_WIT2`

