# Formulas

### Formulas and XPath <a href="#formulas-and-xpath" id="formulas-and-xpath"></a>

In Orbeon Forms, formulas are expressed using _XPath_, a standard _expression language_ for XML. It does not allow you to _modify_ XML data, but it allows you to _query_ XML data and compute values.

For those familiar with Microsoft Excel or other spreadsheet software, you can think of formulas with Orbeon Forms as what follows the "=" sign in a spreadsheet. It is similar to that, but with a slightly different syntax and set of rules.

In general, you don't need to know about XPath in Form Builder, with the exception of some properties in the Control Settings and Section Settings dialogs. Formulas are considered an advanced feature of Form Builder, which might require some programming knowledge.

_NOTE: Incorrect XPath expressions may cause the form to behave improperly, so caution must be applied._

### Examples of formulas <a href="#examples-of-formulas" id="examples-of-formulas"></a>

See [Examples of formulas](https://doc.orbeon.com/form-builder/formulas/formulas-examples).

### Referring to control values from formulas <a href="#referring-to-control-values-from-formulas" id="referring-to-control-values-from-formulas"></a>

#### Basic usage <a href="#basic-usage" id="basic-usage"></a>

To refer to a form control value from a formula, use the variable notation `$foo` where `foo` is the control name. For example:

Copy

```
$price * $quantity
```

This assumes a form control named "price" and another form control named "quantity".

#### Variables in depth <a href="#variables-in-depth" id="variables-in-depth"></a>

In a formula, when referring to `$price`, the result is actually an XML node, as this is how Orbeon Forms stores data internally. Think of a node as a container for a value:

Copy

```
<price>4.99</price>
```

There are two ways to obtain a value from this node:

* using the `string()` function
  * this returns, of course, a value of type `string`
  * example: `$price/string()` or `string($price)`
* using the `data()` function
  * this returns a value of the type associated with the associated form control
  * example: `$price/data(.)` or `data($price)`

Now in many cases, XPath gets the value for you without calling `string()` or `data()`. For example when you perform a multiplication:

Copy

```
$price * $quantity
```

Here, XPath "sees" that the multiplication cannot apply to nodes, and automatically fetches ("atomizes") the value from the nodes. In this case, if the associated form control has type "Decimal", then the values will be retrieved from the nodes and converted to decimal numbers as if you had used the `data()` function explicitly:

Copy

```
data($price) * data($quantity)
```

Note that the multiplication will fail if the form controls do not contain valid decimal numbers.

The `data()` function can be a little tricky because of this. Now say you'd like instead to _concatenate_ the text of those nodes containing decimal values even if the don't contain valid decimal values. Then you have to explicitly use the `string()` function:

Copy

```
concat('Price: ', string($price), 'Quantity: ', string($quantity))
```

#### Where you can use variables <a href="#where-you-can-use-variables" id="where-you-can-use-variables"></a>

You can use the variable notation in the following formulas in the Control Settings dialog:

* Required
* Validation
* Calculated Value
* Initial Value
* Visibility
* Read-Only

\[SINCE Orbeon Forms 2022.1]

You can also use the variable notation in the following formulas:

* Repeated grids and sections:
  * Minimum Number of Repetitions
  * Maximum Number of Repetitions
  * Freeze Repetitions
* Number and Currency fields
  * Prefix
  * Suffix
* Dynamic Dropdown:
  * Resource URL
  * Choices formula
  * Label formula
  * Value formula
  * Hint formula
* Actions
  * [Legacy actions](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions)
  * [Action syntax](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions-syntax)
* [Template parameters](https://doc.orbeon.com/form-builder/advanced/template-syntax)
  * Control name
  * Formula

See also the blog post [Improvements to variables in formulas](https://blog.orbeon.com/2022/04/improvements-to-variables-in-formulas.html).

#### Resolution of repeated controls <a href="#resolution-of-repeated-controls" id="resolution-of-repeated-controls"></a>

When referring to controls that are repeated, for example within repeated grids or repeated sections, a variable can return a sequence of multiple values. Each value corresponds to a repeated control.

However, not all controls in the form are necessarily selected. Instead, the "closest" controls are selected, as follows:

* The closest enclosing repeat iteration between the location of the formula and the control identified by the variable is identified. If there is none, then this is the top-level of the form.
* Then all the controls within that iteration, or within the entire form when there is no such iteration, are selected.

For example, consider

* a repeated grid
* a decimal field called `price` on each row
* an integer field called `quantity` on each row
* a decimal text output field called `row-total` on each row
* a decimal text output field called `total` below the grid

Calculated value expression for `row-total`:

Copy

```
$price * $quantity
```

The `row-total` calculated value applies to the closest `price` and `quantity` controls, that is, those on the same row, and each row gets its own row total.

Calculated value expression for `total`:

Copy

```
sum($row-total[string() castable as xs:decimal], 0.0)
```

On the other hand, the `total` calculation is outside the repeat, and when it refers to `$row-total`, all `row-total` values are returned. Therefore, the sum applies to all the `row-total` values (assuming they can be cast as `xs:decimal`, in this example).

TODO: Is this the same as using `fr:control-string-value('$price', false())`, or is there a subtle difference?

### Renaming of controls and formulas <a href="#renaming-of-controls-and-formulas" id="renaming-of-controls-and-formulas"></a>

\[SINCE Orbeon Forms 2019.1]

When a control or section or grid is renamed, dependent formulas which use the variable notation `$foo` (where `foo` is the control name) are automatically updated.

\[SINCE Orbeon Forms 2022.1]

In addition, other references which use the variable notation `$foo` where `foo` is the control name, are automatically updated, including:

* Repeated grids and sections:
  * Minimum Number of Repetitions
  * Maximum Number of Repetitions
  * Freeze Repetitions
* Number and Currency fields
  * Prefix
  * Suffix
* Dynamic Dropdown:
  * Resource URL
  * Choices formula
  * Label formula
  * Value formula
  * Hint formula
* Actions
  * [Legacy actions](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions)
  * [Action syntax](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions-syntax)
* [Template parameters](https://doc.orbeon.com/form-builder/advanced/template-syntax)
  * Control name
  * Formula

### Where do formulas appear? <a href="#where-do-formulas-appear" id="where-do-formulas-appear"></a>

#### Control Settings dialog <a href="#control-settings-dialog" id="control-settings-dialog"></a>

In the Control Settings dialog, formulas are used to specify the following aspects of a control.

**Validations and alerts**

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LImOuyX6v8jYBpAvh\_S%252F-LEkBwwzQBuFcIscjgrv%252Fcontrol-settings-validations.png%3Fgeneration=1533075263475905%26alt=media\&width=768\&dpr=4\&quality=100\&sign=0ab7df4237f0b7dbd2c129ba14edbfb3929f864b6d101b429e32690a79e82515)

Validations

* **Constraint:** Boolean expression specifying whether the control is valid.
  * If this field is left blank, then the validity of the control depends on the data type and the "Required" option.
  * Otherwise, the control is valid if in addition to all the other constraint being met, the result of the Boolean expression is `true()`.

**Formulas**

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LQ5z6SJ1C6JNSNI51lo%252F-LEkBwx0uppKKLVgHDn6%252Fcontrol-settings-formulas.png%3Fgeneration=1540936764793083%26alt=media\&width=768\&dpr=4\&quality=100\&sign=be764afc0484307d03f12cdd6577db34d9d159f38d6f3da1564aa4485d453360)

Formulas

* **Visibility:**
  * Boolean expression specifying whether the control is visible.
  * If this field is left blank, then the control is always visible, unless the section is not visible.
  * Otherwise, it is visible only if the result of the boolean expression is `true()`.
* **Read-Only:**
  * Boolean expression specifying whether the control is read-only (not editable).
  * If this field is left blank, then the control is editable unless the section is read-only.
  * Otherwise, the field is editable only if the result of the Boolean expression is `false()`.
* **Initial value:**
  * String expression returning the initial value of the control when the form first shows. This is only applied in `new` mode.
  * Default: the value set into the field at design time (usually a blank value).
* **Calculated Value:**
  * String expression specifying a calculated value of the control which updates while the form user interacts with the form.
  * Default: the default value of the control, or the value entered by the form user.

#### Section/Grid Settings dialog <a href="#section-grid-settings-dialog" id="section-grid-settings-dialog"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-11604b458c173c3982e89da1966b210e98fb0056%252Fsection-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=3cfd2923875262a890477ab9073a5c426f73737e127d37de5bcac707d088b68e)

Basic Settings and Formulas

In the Section/Grid Settings dialog, formulas are used to specify the following aspects of a section or grid:

* **Visibility:** Boolean expression specifying whether the section is visible or not.
  * If this field is left blank, then the section is always visible.
  * Otherwise, it is visible only if the result of the Boolean expression is true().
* **Read-Only:** Boolean expression specifying whether the section is shown as read-only or not.
  * If this field is left blank, then the section content is always editable.
  * Otherwise, the section content is editable only if the result of the Boolean expression is false().

\[SINCE Orbeon Forms 2020.1]

In the Form Settings dialog, an XPath expression can be used to specify whether the entire form is read-only (not editable).

#### Actions <a href="#actions" id="actions"></a>

In the Section/Grid Settings dialog, formulas are used to specify the following aspects of a section or grid:

TODO

### Examples <a href="#examples" id="examples"></a>

See [examples of formulas](https://doc.orbeon.com/form-builder/formulas/formulas-examples).
