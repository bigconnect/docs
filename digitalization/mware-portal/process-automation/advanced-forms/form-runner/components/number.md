# Number

The number component is designed to enter integer or decimal numbers. See Currency for a similar component designed to enter currency amounts.

![Number field with unit](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC3\_u7rmYB2bPWzDl%252Fxbl-number.png%3Fgeneration=1528743361578803%26alt=media\&width=300\&dpr=4\&quality=100\&sign=02586dd0f965f99634555f9ba0bc6ff56e6605e7089e6ddb8b54e679eea3785c)

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

You use the number component like a regular input field, for example:

Copy

```
<fr:number ref="my-number">
  <xf:label>Quantity</xf:label>
</fr:number>
```

#### Form builder support <a href="#form-builder-support" id="form-builder-support"></a>

Form builder shows extended control settings for Number and Currency fields:

![Number and Currency field control settings](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-54bb537ae28a0f4b480af66f2346ed5c1ceb9896%252Fxbl-number-control-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=0f692946f2b2b46c1184d3ee38b54349a26339196d5459dd7b67e470597b20f7)



These are also available at the form level:

![Number and Currency field form settings](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-ca8f3c32ef3311b5e9a0647ff044e76ed6795087%252Fxbl-number-form-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=b4b9814d61d65e27399d09ce8590741daf801a1289866f697c444f7de96db4a4)



### Datatype <a href="#datatype" id="datatype"></a>

`fr:number` must be bound to either of:

* `xs:decimal`
* `xs:integer`&#x20;

### Parameters <a href="#parameters" id="parameters"></a>

`fr:number` supports parameters, which you can set via properties or directly on `fr:number`:

* `prefix`: optional prefix shown before the number
* `suffix`: optional suffix shown after the number
* `digits-after-decimal`: digits to show after the decimal point (by default 0, which means the number is an integer)
* `decimal-separator`: single character to use as decimal separator
  * use `.` or `,` only
* `grouping-separator`: single character to use as thousands separator separator (can be blank)
* `round-when-formatting`: when formatting the number for display, whether to round the value to `digits-after-decimal` if there are more digits after the decimal point or not. The default is `false`.
  * Rounding uses the [half up](https://en.wikipedia.org/wiki/Rounding#Round\_half\_up) method.
* `round-when-storing`: when storing the number entered by the user, whether to round the value to `digits-after-decimal` if there are more digits after the decimal point or not. The default is `false`.
  * Rounding uses the [half up](https://en.wikipedia.org/wiki/Rounding#Round\_half\_up) method.
* `pattern`
  * overrides the HTML `pattern` attribute
* `field-align`: determines the alignment of the value in the field
  * `left` (default): align content to the left
  * `right`: align content to the right

These are the default values of the properties:

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.prefix"
    value=""/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.suffix"
    value=""/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.digits-after-decimal"
    value=""/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.decimal-separator"
    value="."/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.grouping-separator"
    value=","/>
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.number.round-when-formatting"
    value="false"/>
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.number.round-when-storing"
    value="false"/>
```

### Validation vs. formatting <a href="#validation-vs.-formatting" id="validation-vs.-formatting"></a>

The `digits-after-decimal` parameter controls how the value entered by the user gets _formatted_ when the field displays it. This does not impact validation of the data at all.

In order to validate the number to have a specific number of decimal digits, the `xxf:fraction-digits()` function should be used.

### Mobile support <a href="#mobile-support" id="mobile-support"></a>

On iOS, when the control identifies that the value is a non-negative integer, it shows a numeric keypad:

![iOS numeric keypad](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC3\_ogtin2n56SS4v%252Fxbl-number-numeric-keypad.png%3Fgeneration=1528743351335920%26alt=media\&width=300\&dpr=4\&quality=100\&sign=a04c0ba08ab36ff1a0bf349d1e9e1f96974e4b13c29e37cba8fe99660d1db1ad)

This is the case if:

* the control is bound to `xs:integer` OR is bound to `xs:decimal` and has an `xxf:fraction-digits(0)` constraint
* AND has an `xxf:non-negative()` constraint

Otherwise, the control shows the numeric pane of the regular keyboard:

![iOS numeric keypad](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC3\_sbiVNTVU7AljZ%252Fxbl-number-numeric-pane.png%3Fgeneration=1528743351359729%26alt=media\&width=300\&dpr=4\&quality=100\&sign=641f01c0e3eb5af36f6388c64dff60db78276ced30ca1a0006596459293290e8)

### Examples <a href="#examples" id="examples"></a>

#### Use of a suffix <a href="#use-of-a-suffix" id="use-of-a-suffix"></a>

Here is an example with a suffix:

![Number field with suffix](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC3\_u7rmYB2bPWzDl%252Fxbl-number.png%3Fgeneration=1528743361578803%26alt=media\&width=300\&dpr=4\&quality=100\&sign=02586dd0f965f99634555f9ba0bc6ff56e6605e7089e6ddb8b54e679eea3785c)

```
<fr:number ref="my-number" suffix="m/s">
    <xf:label>Number</xf:label>
    <xf:hint>Number field with validation</xf:hint>
</fr:number>
```

#### Rounding <a href="#rounding" id="rounding"></a>

**Rouding when formatting only**

Assume the following properties or corresponding attributes:

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.digits-after-decimal"
    value="2"/>
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.number.round-when-formatting"
    value="true"/>
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.number.round-when-storing"
    value="false"/>
```

Scenario:

* User types "12345.678".
* Because `round-when-storing` is `false`, the value is not rounded and stored as is in the data.
* When the user focuses out of the field, the value is formatted and rounded, because `round-when-formatting` is set to `true`.
* Field displays "12,345.68".

This is useful if you want to keep the full precision of decimal values in the data, but format them nicely to a given number of decimals.

**Rounding when formatting and when storing**

Assume the following properties or corresponding attributes:

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.digits-after-decimal"
    value="2"/>
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.number.round-when-formatting"
    value="true"/>
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.number.round-when-storing"
    value="true"/>
```

Scenario:

* User types "12345.678".
* Because `round-when-storing` is `true`, the value is rounded and stored as "12345.68" in the data.
* When the user focuses out of the field, the value is formatted and rounded, because `round-when-formatting` is set to `true`.
* Field displays "12,345.68".

_NOTE: Here rounding during formatting doesn't change the value since it was rounded when stored in the first place._

This is useful if you don't want to keep the full precision of decimal values in the data and want to force a maximum number of digits after the decimal point.

#### Right-aligned content <a href="#right-aligned-content" id="right-aligned-content"></a>

Regular controls:

![Right-aligned content](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-1ef3ab63c296aef0f2c41499aa1125f69b6f113e%252Fxbl-number-right-aligned.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=656a2dfcd0ffe94682f61c29c0f9e67e4d3b088785dcddb725ec4b929ce39f6f)



Within a repeated grid:

![Right-aligned content in a repeated grid](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-9c54f29ddc88f289641190bcbecfe32a6318d38d%252Fxbl-number-right-aligned-repeated-grid.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=bdb56c42fd728d94b6f035b888b610ca30cc5da8122c4fcb8c58f8576261c99b)

