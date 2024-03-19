# Automatic calculations dependencies

Calculated and initial values expressed with formulas can depend on the value of other form controls. In turn, those form controls can be calculated from yet other form controls. Such controls may appear further down in the form, or, in fact, in any place.

It is therefore important to compute calculated and initial values in an order based on the _dependencies_ of that value, not just in the order in which the controls appear in the form. This is very similar to what happens in a spreadsheet.

Form Runner supports dependencies of calculated and initial values when the associated formulas refer to other controls via _variables_ (such as `$units`, where `units` is the _name_ of a control on the form).

### Availability <a href="#availability" id="availability"></a>

\[SINCE Orbeon Forms 2018.1]

Orbeon Forms features a user interface option to enable and disable automatic calculations dependencies in the "Form Settings" dialog's "Formulas" tab.:

* Use property: use the `oxf.xforms.analysis.calculate` property
* Always or Never: enable or disable automatic calculation dependencies, no matter how the property is configured.

In addition, for _new form definitions_, the "Automatic Calculations Dependencies" option is enabled by default starting with Orbeon Forms 2018.1.

_NOTE: Existing form definitions which do not have the `xxf:analysis.calculate="true"` attribute set in the form definition source are not automatically upgraded to enable automatic calculations dependencies. You must enable dependencies explicitly in the "Form Settings" dialog._

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-9ca6b4686f0bc30dc2043948883b1762480c83d0%252Fform-settings-formulas.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=a98c816af3bf73757085fe72f77e4de14dfe1ecbb6d8431d258a8848a973619f)

Form Options

_NOTE: Prior to Orbeon Forms 2022.1.1, this setting was under the "Form Settings" dialog's "Form Options" tab._

\[SINCE Orbeon Forms 4.10]

The feature was available for the first time with Orbeon Forms 4.10, but without a user interface. With such older versions of Orbeon Forms, you enable the feature by adding the `xxf:analysis.calculate="true"` attribute on the first model of the form, via "Edit Source".

### Renaming of controls and formulas <a href="#renaming-of-controls-and-formulas" id="renaming-of-controls-and-formulas"></a>

\[SINCE Orbeon Forms 2019.1]

See [Renaming of controls and formulas](https://doc.orbeon.com/form-builder/formulas#renaming-of-controls-and-formulas).

### Examples <a href="#examples" id="examples"></a>

Consider the following grid, where the Total Total appear on each line item row:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-a5c0fe2dcaad984a4ba27a0c01a8b5c53ba11c88%252Fcalculations-dependencies-grid.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=8387d8d22db42fb85237717af1cb5172a61ff95d2bdc401dcd59cbc193dfc621)

Grid with calculations

Each Row Total is expressed with the following calculated value formula:

Copy

```
$units * $unit-price
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-3b111e7076b72f60fdf9e57ca542733ec62ee85f%252Fcalculations-dependencies-row-total.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=260eaae1358575b48c53f01e77c662efe0a7431317ed3c73cf360368b8885348)

Row totals

And the Total Units and Total Price are expressed with the following calculated value formula:

Copy

```
sum($units)
```

and:

Copy

```
sum($row-total)
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-66d4d74b928502475b1e48ade5595fdee0350afb%252Fcalculations-dependencies-global-total.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=3fb8b34c77cd83b59def25e0f4ea06429297b9fb0c16c81c7af77ec771fc4bd2)

Total price

Automatic calculations dependencies ensure that the totals update correctly as the user modifies the "Units" and "Unit Price" fields. This is the case whether the totals are positioned after or before the fields they depend on in the form.

_NOTE: It is important to note that using variable is key for the dependencies to work. Using other expressions referring to form data, such as `/form/my-section/my-control`, or `//my-control`, will not work for the purpose of dependencies._
