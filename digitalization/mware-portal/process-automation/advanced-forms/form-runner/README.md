# Form Runner

This page explains some terms which occur often in the Advanced Forms documentation.

### Form controls <a href="#form-controls" id="form-controls"></a>

A form "control" is an interactive user interface element that allows the user to enter or view data. Examples include text fields, dropdown menus, radio buttons, and more.

With Advanced Forms, not all form controls are called "fields", and we reserve that latter word for form controls where the user enters text, usually on a single line.

### Form definitions and form data <a href="#form-definitions-and-form-data" id="form-definitions-and-form-data"></a>

We make a distinction between:

* A **Form definition** is the description of a given form, including which sections, grids, and fields it contains, and all associated properties. You typically create a form definition using Form Builder. With Advanced Forms, a form definition is encoded as an XML document using XHTML and XForms. Examples of form definitions include an "Address" form, or a "Claim" form.
* **Form data** is a set of data entered by a user via a form definition. For example, Hillary might have used the "Claim" form to file a claim. With Advanced Forms, form data is encoded as an XML document.

### Application name and form name <a href="#application-name-and-form-name" id="application-name-and-form-name"></a>

In Form Builder, a form definition is identified by a two-level hierarchy of names:

* **Application name**: a name which allows grouping form together. The application name (or app name) can be, for example:
  * a company name such as "forms" or "acme"
  * a company entity such as "hr" or "sales" or "engineering"
  * a project name such as "mercury" or "phoenix"
* **Form name**: a name which is local to an application name, for example "address" or "claim".

This two-level hierarchy allows for easy grouping of forms, and allows using a single instance of Form Builder to host distinct applications.&#x20;

Application names and form names don't have to be human-readable, although it is generally convenient to make them human-readable.

Form data is identified by a three-level hierarchy which includes:

* the **application name/form name** couple that identifies the form definition
* plus a **unique form data id** or **document id** provided by Form Runner for each instance of form data

### Summary Page, Detail Page and Home Page <a href="#summary-page-detail-page-and-home-page" id="summary-page-detail-page-and-home-page"></a>

Form Runner and Form Builder have a few pages (or screens) which are referred to as follows:

* Form Builder
  * **Summary Page**:
    * This page is for _form authors_.
    * It lists and allows searching the _form definitions_ created with Form Builder. These might not have been _published_ yet.
    * It is also the starting point for the Form Builder Detail Page (see below).
  * **Detail Page**:
    * This page is for _form authors_.
    * It is the actual Form Builder editor, which allows you to create, edit and publish form definitions.
* Form Runner
  * **Summary Page**:
    * This page is usually for _end users_.
    * It lists and allows searching _form data_ for a given form definition.
    * It is also the starting point for the Form Runner Detail Page (see below).
  * **Detail Page**:
    * This page is usually for _end users_.
    * This page allows creating, editing, viewing, saving, and submitting _form data_.
  * **Published Forms Page**:
    * This page shows _published form definitions_.
    * This page is useful for _end users_ to see which _form definitions_ they have access to.
    * It can be a starting point to create new form data or to access the Form Runner Summary page.
    * See Published Forms Page for more.
* **Forms Admin Page**:
  * This page is useful for _admins_, who can set form availability, move forms between servers, and more.
  * See Forms Admin Page for more.
