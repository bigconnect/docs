# Currency

The currency component is an input field specialized to capture amounts in a particular currency.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0Ige5wLLD\_Ap6Kb%252Fxbl-currency6.png%3Fgeneration=1528743346611042%26alt=media\&width=768\&dpr=4\&quality=100\&sign=0582012b1a8b15118b28965a859eaab57e4e2f1ebc79b945d9aa00cea81e5e26)

When the input field doesn't have the focus, it shows a formatted currency, such as `1,234.00`. When the control has the focus, it shows the plain number, such as `1234` to facilitate input.

While working with currency amounts, the component is careful not to do any manipulation that relies on how numbers are represented on a particular platform, to avoid any possibility of rounding or precision error.

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

You use the number component like a regular input field, for example:

Copy

```
<fr:currency ref="my-amount">
  <xf:label>Amount</xf:label>
</fr:currency>
```

### Parameters <a href="#parameters" id="parameters"></a>

`fr:currency` supports parameters, which you can set via properties or directly on `fr:currency`:

* `prefix`: optional prefix shown before the number ( by default the dollar sign)
* `suffix`: optional suffix shown after the number
* `digits-after-decimal`: digits to show after the decimal point (by default 2)
* `decimal-separator`: single character to use as decimal separator
* `grouping-separator`: single character to use as thousands separator separator (can be blank)
* `round-when-formatting`: when formatting the number for display, whether to round the value to `digits-after-decimal` if there are more digits after the decimal point or not. The default is `false`.
  * SINCE Orbeon Forms 2016.1
  * UNTIL Orbeon Forms 2017.2: Rounding uses the [half to even](https://en.wikipedia.org/wiki/Rounding#Round\_half\_to\_even) method.
  * SINCE Orbeon Forms 2018.1: Rounding uses the [half up](https://en.wikipedia.org/wiki/Rounding#Round\_half\_up) method.
* `round-when-storing`: when storing the number entered by the user, whether to round the value to `digits-after-decimal` if there are more digits after the decimal point or not. The default is `false`.
  * SINCE Orbeon Forms 2016.1
  * UNTIL Orbeon Forms 2017.2: Rounding uses the [half to even](https://en.wikipedia.org/wiki/Rounding#Round\_half\_to\_even) method.
  * SINCE Orbeon Forms 2018.1: Rounding uses the [half up](https://en.wikipedia.org/wiki/Rounding#Round\_half\_up) method.

These are the default values of the properties:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.currency.prefix"
    value="$"/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.currency.suffix"
    value=""/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.currency.digits-after-decimal"
    value="2"/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.currency.decimal-separator"
    value="."/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.currency.grouping-separator"
    value=","/>
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.currency.round-when-formatting"
    value="false"/>
<property
    as="xs:boolean"
    name="oxf.xforms.xbl.fr.currency.round-when-storing"
    value="false"/>
```

#### Currency prefix <a href="#currency-prefix" id="currency-prefix"></a>

By default the dollar sign ("$") is used as a currency prefix. You can override this either:

* Globally for all your forms in your ‘properties-local.xml’ by setting the following property:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.currency.prefix"
    value="£"/>
```

* For a particular instance of the component:
  * With the `prefix` attribute, for static values.
  * With the `<fr:prefix>` element, for dynamic values.

#### Digits after the decimal sign <a href="#digits-after-the-decimal-sign" id="digits-after-the-decimal-sign"></a>

By default, the component shows the value to 2 digits after the decimal sign. You can override this either:

* Globally for all your forms in your `properties-local.xml` by setting the following property:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.currency.digits-after-decimal"
    value="3"/>
```

* For a particular instance of the component:
  * With the `digits-after-decimal` attribute, for static values.
  * With the `<fr:digits-after-decimal>` element, for dynamic values.

If you set `digits-after-decimal` to 0, then the decimal separator isn't shown. The component truncates any additional digits beyond the specified number; it doesn't perform rounding.

### Examples <a href="#examples" id="examples"></a>

#### Control without focus <a href="#control-without-focus" id="control-without-focus"></a>

Copy

```
<fr:currency ref="value"/>
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0IsjGK5mIFtbTUn%252Fxbl-currency1.png%3Fgeneration=1528743356610728%26alt=media\&width=768\&dpr=4\&quality=100\&sign=c30f7c6bb34de481d3804522d5ccafc754b411ba7dd87100d4ba6fa250355520)

#### Setting a static prefix with the prefix attribute <a href="#setting-a-static-prefix-with-the-prefix-attribute" id="setting-a-static-prefix-with-the-prefix-attribute"></a>

Copy

```
<fr:currency ref="value" prefix="£"/>
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0Iw85qXYx5VNGPc%252Fxbl-currency2.png%3Fgeneration=1528743348562464%26alt=media\&width=768\&dpr=4\&quality=100\&sign=f13730222a09763025148dd77e6f5d260f68f062c3651715095064e9c7b59613)

#### Setting a dynamic prefix with the fr:prefix element <a href="#setting-a-dynamic-prefix-with-the-fr-prefix-element" id="setting-a-dynamic-prefix-with-the-fr-prefix-element"></a>

Copy

```
<fr:currency ref="value">
    <fr:prefix ref="/config/prefix"/>
</fr:currency>
```

#### Showing 3 digits after decimal sign <a href="#showing-3-digits-after-decimal-sign" id="showing-3-digits-after-decimal-sign"></a>

Copy

```
<fr:currency
    ref="value"
    digits-after-decimal="3/>
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0Iw85qXYx5VNGPc%252Fxbl-currency2.png%3Fgeneration=1528743348562464%26alt=media\&width=768\&dpr=4\&quality=100\&sign=f13730222a09763025148dd77e6f5d260f68f062c3651715095064e9c7b59613)

#### Showing 0 digits after the decimal sign <a href="#showing-0-digits-after-the-decimal-sign" id="showing-0-digits-after-the-decimal-sign"></a>

Copy

```
<fr:currency
    ref="value"
    digits-after-decimal="0/>
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0Iyky8MDwbWmC3a%252Fxbl-currency3.png%3Fgeneration=1528743359003878%26alt=media\&width=768\&dpr=4\&quality=100\&sign=7202b7097548a465eb828df15a80a48c38bd2433f811990d0552a996f19ff89a)

#### Read-only input field, because bound to node set as read-only with a MIP <a href="#read-only-input-field-because-bound-to-node-set-as-read-only-with-a-mip" id="read-only-input-field-because-bound-to-node-set-as-read-only-with-a-mip"></a>

Copy

```
<xforms:bind ref="readonly-node" readonly="true()"/>

<fr:currency ref="readonly-node"/>
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0J-ABKt7XyQr3r7%252Fxbl-currency4.png%3Fgeneration=1528743344770178%26alt=media\&width=768\&dpr=4\&quality=100\&sign=ace34182881d28c119aeccf3b5568e8599763b9041e65d5c2ee7d80bc57322f4)

#### Rounding <a href="#rounding" id="rounding"></a>

\[SINCE Orbeon Forms 2016.1]

For examples of rounding, see the [`fr:number`](https://doc.orbeon.com/form-runner/component/number) component.
