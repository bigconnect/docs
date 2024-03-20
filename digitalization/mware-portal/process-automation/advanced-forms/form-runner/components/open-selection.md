# Open selection

The `fr:open-select1` component presents:

* a list of radio buttons predefined by the form author
* and a custom choice ("Other") which the user has the option to specify instead

![Open selection component](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzP3AfwSd3tEmF1D%252Fxbl-open-select1.png%3Fgeneration=1528743336640214%26alt=media\&width=768\&dpr=4\&quality=100\&sign=8135813e3098ffb03c3e11e685a8b5ca75f9419bd5718a255cca594f181d9e4d)



### Usage <a href="#usage" id="usage"></a>

You use `fr:open-select1` like a regular `xf:select1` control:

```
<fr:open-select1 id="flavor" bind="flavor-bind">
    <xf:label>Make your selection</xf:label>
    <xf:item>
        <xf:label>Chocolate</xf:label>
        <xf:value>chocolate</xf:value>
    </xf:item>
    <xf:item>
        <xf:label>Vanilla</xf:label>
        <xf:value>vanilla</xf:value>
    </xf:item>
    <xf:item>
        <xf:label>Strawberry</xf:label>
        <xf:value>strawberry</xf:value>
    </xf:item>
</fr:open-select1>
```

### Advanced usage <a href="#advanced-usage" id="advanced-usage"></a>

`fr:open-select1` supports the following attributes:

* `appearance`
  * `full`: shows radio buttons (the default)
  * `minimal`: shows a dropdown menu

This is the control with the `minimal` appearance:

![Open selection component with \`minimal\` appearance](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzP\_G\_bjtRbwEtmu%252Fxbl-open-select1-minimal.png%3Fgeneration=1528743349603508%26alt=media\&width=768\&dpr=4\&quality=100\&sign=9dac63b1a77d691e722bbd1b784e2752c073a1ab2e941be6387569c62116f2f3)



### Data format <a href="#data-format" id="data-format"></a>

The predefined radio buttons have a _label_ and a _value_, and, like all radio buttons, store their _value_ into the data.

The text of the custom choice entered by the user is saved directly into the data.
