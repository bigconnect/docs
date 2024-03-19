# Versioning

### What is form definition versioning? <a href="#what-is-form-definition-versioning" id="what-is-form-definition-versioning"></a>

#### Without form definition versioning <a href="#without-form-definition-versioning" id="without-form-definition-versioning"></a>

Without form definition versioning, Orbeon Forms only keeps track of a single [published](https://doc.orbeon.com/form-builder/form-editor/publishing) form definition for a given application name/form name (see [terminology](https://doc.orbeon.com/form-runner/overview/terminology#application-name-and-form-name)). This means that if you republish a form, the previously-published form definition gets overwritten (although there is an [auditing trail](https://doc.orbeon.com/form-runner/api/persistence/versioning#auditing-trail)).

This also means that when opening existing data for editing or viewing, Orbeon Forms always uses the latest published version of the form definition. This can lead to unexpected results, as fields might have been added, removed, or renamed in the new form definition. In fact, the form definition could be completely different from the form definition used to create and save the data currently stored in the database!

#### With form definition versioning <a href="#with-form-definition-versioning" id="with-form-definition-versioning"></a>

Form definition versioning introduces the idea that published form definitions for a given application name/form name also have a version number. When creating a _new_ version upon publishing, the version number is "1", and when publishing again with a new version the number increases: "2", "3", etc. So you can talk about "version 2 of the Acme Order form".

![Creating a new version](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwwLIDKHC9nZMEPp%252Fpublish-version-next.png%3Fgeneration=1528743328368439%26alt=media\&width=300\&dpr=4\&quality=100\&sign=5c74321714cb2298003488bdc3cc178f5a4ac49f3dc8e02444b02697afaa5d4f)

When saving form data, the data in the database is associated not only with the application name and form name, but also with the _version_ of the form definition that was used to create or edit that data. So when you open that data for editing or viewing at a later time, Form Runner can use the matching version of the form definition.

When publishing, if versioning is supported by the persistence layer, you have a choice to _create a new form version_ or to _override an existing one_. This is detailed in [Publishing](https://doc.orbeon.com/form-builder/form-editor/publishing).

### Compatibility of changes <a href="#compatibility-of-changes" id="compatibility-of-changes"></a>

There are different types of changes you can make to a form definition which impact whether _existing data_ in the database will be compatible with those changes.

_NOTE: If there is not yet any data created by that given form definition version in the database, then compatibility is not a problem._

Examples of compatible changes:

* updating labels, hints, help messages, and explanatory texts
* adding items to selection controls
* changing the initial value of a control, either directly with the control or via an _Initial Value_ formula
* moving a control within the same grid or section
* changing grid column widths
* adding a language
* updating permissions
* updating email settings

Examples of incompatible changes:

* renaming a control
* adding a control, grid, or section
* removing a control, grid, or section
* renaming a control, grid, or section
* moving a control between grids or sections
* changing the nesting of grids or section within subsections

See also [Simple data migration](https://doc.orbeon.com/form-runner/features/simple-data-migration), which enables some of the incompatible changes listed above.

### Limitations <a href="#limitations" id="limitations"></a>

* \[SINCE Orbeon Forms 2021.1] If you are using the built-in implementation of the persistence API for relational databases to store data, if you want to use another implementation to store your forms, then that implementation needs to support versioning.
* \[UP TO Orbeon Forms 2020.1] You are using a provider that uses the built-in implementation of the persistence API for relational databases to store data, then you also need to use the same provider to store forms (see [#3926](https://github.com/orbeon/orbeon-forms/issues/3926)). This limitation is lifted starting in Orbeon Forms 2021.1.
