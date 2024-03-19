# Hidden field

This control supports all the features of a regular control, including:

* label
* validations and alerts
* formulas

However, it doesn't show in the form at runtime. At design-time, the control shows as a box with an icon so that the form author knows that a hidden control is in use.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LV0SEt6dQcUrbflOiwf%252F-LV0RAMZ3jBYZPyd5pBV%252Fxbl-hidden.png%3Fgeneration=1546212705923865%26alt=media\&width=768\&dpr=4\&quality=100\&sign=6ff97f3927aa35ce00c28bb3c21857127887009c2e1b984689e52dbaff65201d)

Hidden field in Form Builder

This is useful in the following cases:

* **Holding intermediary calculations:** If the control has a "Calculated Value" and/or "Initial Value" formula, it can be referred to by variable name (like `$my-intermediary-result`) from other formulas.
* **Handling out-of-band validations:** If the control has a label, alert and validations, a failing validation will show in the Error Summary. In addition, this also impacts the Wizard View's lax and strict validation modes, preventing forward navigation until the error is fixed by the user.

_NOTE: With Form Builder, you cannot directly edit the label of a hidden field inline. Instead, use "Control Settings"._

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

Copy

```
<fr:hidden ref="validating">
    <xf:label>Validation</xf:label>
    <xf:alert>Please start the validation before proceeding to the import</xf:alert>
</fr:hidden>
```
