# Section templates

### Section template libraries <a href="#section-template-libraries" id="section-template-libraries"></a>

Form Builder supports defining reusable sections called _section templates_.

Section templates are grouped within _section template libraries_. There can be one library per application name. In addition, a global section template library with the `orbeon` application name is available. This library is made available to all form definitions, no matter what their application name is.

### How section templates work <a href="#how-section-templates-work" id="how-section-templates-work"></a>

You create, edit, and publish a section template library from Form Builder as you do with a regular form definition. The only difference is that the form name for a section template library is `library`. This is a special name recognized by Form Builder.

When you publish a section template library form definition, all top-level sections in that form definition are made available to other form definitions created in Form Builder, depending on their application name, and appear in the Form Builder toolbox, which shows:

* **Global section templates.**
  * Those are created and updated with the `orbeon/library` form definition.
* **Application section templates.**
  * Those are created in the current application's `library` form definition.
  * For example all form definitions with app name `acme` can use section templates defined in the `acme/library` form definition in addition to the global `orbeon/library` section templates.

The title of the section in the section template library form definition is used as the title of the section template that shows in the toolbox.

For example, you can create a generic "US Address" section and reuse it in multiple forms:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC77FzUho25tFKGNL%252Fsection-template.png%3Fgeneration=1528743364651857%26alt=media\&width=768\&dpr=4\&quality=100\&sign=a26e73ac5328d57376086573e9d9e9a261e940f6388abdc7d67947dd2cddcba1)

Defining a section template in Form Builder

Section templates can contain:

* nested grids, with or without repeats
* nested subsections, with or without repeats
* services and actions (see below)

When you click on a given section template, the section is inserted into the form after the currently selected section. You can then select a new title for the section. It is possible to include a section template more than once.

The section appears with read-only fields (which means that you cannot change properties of the controls once inserted):

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-32f02d8947ccf4d2364139d375164d1571159c15%252Fsection-template-use.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=52c4eae17cae1de0bc469e87955f482b9d14c7f7e6ce18c4fd772f6bc893b7af)

Using section templates in Form Builder

When deploying the form, section templates appear like regular editable sections:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC77JUg\_T2xQZO49q%252Fsection-template.png%3Fgeneration=1528743364758545%26alt=media\&width=768\&dpr=4\&quality=100\&sign=807ea6087fbcd524a610aff1c9dc0574116782a0d863307cfde9af547bd4e8b6)

Section templates in Form Runner

\[SINCE Orbeon Forms 2021.1]

In Form Builder, the inserted section templates show, at the top, the following information:

* section template library name
* section template section name
* section template library version

[\[SINCE Orbeon Forms 2023.1.1\]](https://github.com/orbeon/orbeon-forms-doc/blob/master/release-notes/orbeon-forms-2023.1.1.md)

When inserting a section template, the following information is copied over to the current form:

* section title
* section help
* custom CSS classes
* Visibility and Readonly formulas for the section itself

Visibility and Readonly formulas are special, as, when edited within the library:

* it usually doesn't make sense for these formulas to refer to control values _within_ the section itself
* they can refer to data and form controls from other sections

However, when inserted into a form, these formulas typically do not have access to other sections. As a workaround, they can depend on the enclosing form's data using formulas of the following shape:

Copy

```
xxf:instance('|fr-form-instance|')//my-control-name
```

In addition, they can depend on values from known sections using the `fr:control-string-value()` function, for example:

Copy

```
fr:control-string-value('my-control-name', false(), 'my-section-name')
```

If possible, the section name from the library is now preserved. If there is already a section with the same name in the enclosing form, a new name is generated.

After insertion, you can modify those settings using the "Section/Grid Settings" dialog.

### Services and actions <a href="#services-and-actions" id="services-and-actions"></a>

#### Introduction <a href="#introduction" id="introduction"></a>

Actions involving controls in a given section are automatically included with the section template, along with the services called by the actions.

#### Cross-section actions <a href="#cross-section-actions" id="cross-section-actions"></a>

\[SINCE Orbeon Forms 2021.1]

When editing a library form, you can define actions involving controls present in more than one section. For example, you can define an action that sets the value of a control in one section based on the value of a control in another section, and that is triggered by the activation of a button that is in a third section.

However, when using sections from the library in a form, nothing prevents you from only including one of the sections. What should then happen with cross-section actions?

With Orbeon Forms 2021.1, there is limited support for this kind of scenarios. For each section template:

1. Orbeon Forms gathers actions that:
   * can update at least one control of the current section (for example with "Set control value" result)
   * or update no control at all but are triggered by a control of the current section (for example with a button activation)
2. Gathers the services that are used by the actions kept in step 1.

A resulting section template will include the actions and services gathered above in steps 1 and 2.

Following this logic, a given action or service can get included in multiple sections.

Limitations:

* In general, an action can update controls in multiple sections. If that's the case, then that action will be included in all sections that include the controls that are updated by the action. This means that the action and associated service can run more than once.
* If a section template includes cross-section actions, and the related sections are all included in the destination form, they will be automatically connected. If a section is missing, however, then the behavior might be unexpected. It is up to the form author to ensure that the resulting form includes the required section templates.

\[SINCE Orbeon Forms 2022.1.2]

Actions and related listeners using the [Action Syntax](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions-syntax) are also included in section templates.

### Updating section templates <a href="#updating-section-templates" id="updating-section-templates"></a>

When you open Form Builder, the latest version of the controls and section templates is retrieved from the database and shown in the toolbox.

If you make changes to section templates by modifying and publishing a library form, you must reload the toolbox in your form to reflect the latest changes using the "Reload Toolbox" icon at the top of the toolbox (Orbeon Forms 2017.1 and earlier) or the "Reload Toolbox" button in the "Advanced" tab (Orbeon Forms 2017.2).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-6792fc8450886ca68dd55af58966e8f411c03d84%252Fadvanced-menu.png%3Falt=media\&width=300\&dpr=4\&quality=100\&sign=c039813767652bbadfcfb4099a826517472a86b8f50e0350b4ca02727157c4d0)

For more about reloading, see [Reloading the toolbox](https://doc.orbeon.com/form-builder/form-editor/toolbox#reloading-the-toolbox)

Note that when you publish your form, the section templates _currently_ loaded in Form Builder at the time of publishing are included with the published form. This means that changes to section templates after the deployment of a form do not affect the deployed form. If you need to update a deployed form with a new version of controls, you must re-publish the form.

### Versioning of section templates <a href="#versioning-of-section-templates" id="versioning-of-section-templates"></a>

\[SINCE Orbeon Forms 2019.1]

When [versioning](https://doc.orbeon.com/form-runner/features/versioning) is available:

* Form Builder allows you to publish multiple versions of a section templates library.
* Form Builder allows you to select which version of a section templates library you want to use in a particular form definition.

For example, if you have published versions 1, 2 and 3 of the `acme` library form, the toolbox shows a dropdown with those versions.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LXUlw-5bvuKpqSO4gHx%252F-LXUm77CZ4gxcOG8JZqq%252Fsection-templates-v2.png%3Fgeneration=1548868946077542%26alt=media\&width=768\&dpr=4\&quality=100\&sign=54a23d48af047f4632b9afbf5ca474c45a0c33348b9a517b21467565c177aa8c)

Version 2 of the section templates library

When you select a particular version, the toolbox reloads with the latest published form definition with that version.

A section templates library may contain different versions of a given section template, identified by section name, or add or remove section templates compared with other versions of the library.

If the form definition currently being edited is using a section template which is not present in the section template library chosen, the section content appears empty.

For more about reloading, see [Reloading the toolbox](https://doc.orbeon.com/form-builder/form-editor/toolbox#reloading-the-toolbox)

### Merging section templates <a href="#merging-section-templates" id="merging-section-templates"></a>

#### Availability <a href="#availability" id="availability"></a>

\[SINCE Orbeon Forms 2017.2]

#### Merging <a href="#merging" id="merging"></a>

You can merge section templates into your current form definition with the "Merge Section Template" icon associated with the section:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC77RFu2I\_C7fz1Ks%252Fsection-template-unmerged.png%3Fgeneration=1528743364703613%26alt=media\&width=768\&dpr=4\&quality=100\&sign=f601dc4f1f835fe72ff2b29d04a1afb7cf459cdb5927b8b170202868f4bd95d8)

Unmerged section template

After activating the icon, a dialog shows:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC77TZZ0VoJXA9NK1%252Fsection-template-merge-dialog.png%3Fgeneration=1528743364685277%26alt=media\&width=300\&dpr=4\&quality=100\&sign=1e69402485a90881484e5cf5780224a509cffed1a800a752dfb8c7d1e05c9f84)

The dialog shows a list of all control names within the section template and how they will be changed after the merge. Since control names are unique within a form definition, two controls cannot have the same name. Names that are available show in green. Names that conflict show in yellow and an automatic name is generated.

You can optionally set a prefix and/or a suffix for all names. This can help prevent automatic generation of names. For example, you could insert a US Address twice:

* first, with all control names prefixed by `shipping-`
* second, with all control names prefixed by `billing-`

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC7JH1q3eWq5l0ZFd%252Fsection-template-merge-dialog-prefix.png%3Fgeneration=1528743365717919%26alt=media\&width=300\&dpr=4\&quality=100\&sign=d2a3f1e3a9b39af35a623e1e6ac83886be5608c36713e128d59f431aabfa3622)

_NOTE: The enclosing section name itself is not part of this renaming process, as the section was already part of the containing form definition and therefore its name was already unique._

After merging the section template, it becomes part of the current form definition and is completely unlinked from the original section template. You can modify the section and its content as if it had been directly created by hand within the form definition:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC77ay5wduG7aw4pK%252Fsection-template-merged.png%3Fgeneration=1528743364672448%26alt=media\&width=768\&dpr=4\&quality=100\&sign=d993d012ff4b3d06701e6ec8aa37adbd541ef9796caf3d98d54f2d1df9eec9fc)

Merged section template

This also means that if you make changes to the section templates library and reload the toolbox, the merged section template does not update.

#### Limitations <a href="#limitations" id="limitations"></a>

Services and actions in the section template are _not_ merged into the enclosing form.
