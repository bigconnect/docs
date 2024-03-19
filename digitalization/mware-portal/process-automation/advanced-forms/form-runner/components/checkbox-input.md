# Checkbox input

The `fr:checkbox-input` component displays a single checkbox and by default stores a value of `true` when selected and `false` when deselected.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC7Fj2bphKrKuP-Tr%252Fxbl-checkbox-input.png%3Fgeneration=1528743364939882%26alt=media\&width=768\&dpr=4\&quality=100\&sign=9b60f8771b2418d17d631d0d0c587194d284618116874ab0b2b9d2d94b0b01e8)

_NOTE: It differs from the (deprecated in Orbeon Forms 2016.1) Boolean Input field (obtained by binding an `xf:input` to a field of `xs:boolean` type) in that the Checkbox Input displays the label right next to the checkbox._

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC7Fpauu2iajiX2WU%252Fxbl-checkbox-input-and-boolean-input.png%3Fgeneration=1528743364978138%26alt=media\&width=768\&dpr=4\&quality=100\&sign=503891fe6ac02e8f8324c028cd73d8dbf1afa954a9bddb949c280c3b37ed7f1f)

Appearance

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

You use `fr:checkbox-input` like a regular XForms controls. Example:

Copy

```
<fr:checkbox-input ref="initially-open">
    <xf:label>Initially Open</xf:label>
</fr:checkbox-input>
```

### Advanced usage <a href="#advanced-usage" id="advanced-usage"></a>

The datatype of the bound node is typically `xs:boolean` but doesn't have to.

The following attributes can be passed:

* `selected-value`: value to store when the checkbox is selected, by default `true`
* `deselected-value`: value to store when the checkbox is deselected, by default `false`

Example:

Copy

```
<fr:checkbox-input
    ref="whitespace"
    selected-value="trim"
    deselected-value="">
    <xf:label>Trim leading and trailing spaces</xf:label>
</fr:checkbox-input>
```

The control works like `xf:select` in that it adds and removes the selected value (by default `true`) into a space-separated list of tokens without affecting other tokens. With the following example, say the bound node initially contains:

Copy

```
<whitespace>foo bar</whitespace>
```

When the user selects the checkbox. The node now contains:

Copy

```
<whitespace>foo bar trim</whitespace>
```

And when the user deselects the checkbox. The node contains again:

Copy

```
<whitespace>foo bar</whitespace>
```

_NOTE: The control only store a value into the bound node when the user changes the checkbox selection. Say the data by default is blank: the control will not automatically change it to `false` (or to the value set by `deselected-value`) until the user selects then deselects the value again._
