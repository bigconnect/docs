# Single-selection tree

This control allows the user to select a single item from a hierarchical tree representation.

![Appearance](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC8-vRaurBn2PySZP%252Fxbl-tree-select1-collapsed-selected.png%3Fgeneration=1528743368578097%26alt=media\&width=300\&dpr=4\&quality=100\&sign=5078abf7dee5d13af14928b60874eccce8a3cf88defb1114d174cfd81c5f90b7)

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

You:

* specify `appearance="tree"` on an `<xf:select1>` control
* generally use an `<xf:itemset>` element to point to items which are organized in a hierarchy

```
<xf:select1 appearance="tree" ref="value">
    <xf:itemset ref="instance('tree-items')//item">
        <xf:label ref="@label"/>
        <xf:value ref="@value"/>
    </xf:itemset>
</xf:select1>
```

The `tree-items` instance might look like:

```
<xf:instance id="tree-items">
    <items>
        <choices>
            <item label="Item 1" value="1">
                <item label="Item 1.1" value="1.1">
                    <item label="Item 1.1.1" value="1.1.1">
                        <item label="Item 1.1.1.1" value="1.1.1.1"/>
                        <item label="Item 1.1.1.2" value="1.1.1.2"/>
                    </item>
                    <item label="Item 1.1.2" value="1.1.2">
                        <item label="Item 1.1.2.1" value="1.1.2.1"/>
                        <item label="Item 1.1.2.2" value="1.1.2.2"/>
                    </item>
                </item>
                <item label="Item 1.2" value="1.2">
                    <item label="Item 1.2.1" value="1.2.1">
                        <item label="Item 1.2.1.1" value="1.2.1.1"/>
                        <item label="Item 1.2.1.2" value="1.2.1.2"/>
                    </item>
                </item>
            </item>
            <item label="Item 2" value="2">
                <item label="Item 2.1" value="2.1">
                    <item label="Item 2.1.1" value="2.1.1">
                        <item label="Item 2.1.1.1" value="2.1.1.1"/>
                        <item label="Item 2.1.1.2" value="2.1.1.2"/>
                        <item label="Item 2.1.1.3" value="2.1.1.3"/>
                    </item>
                </item>
                <item label="Item 2.2" value="2.2">
                    <item label="Item 2.2.1" value="2.2.1"/>
                    <item label="Item 2.2.2" value="2.2.2"/>
                </item>
            </item>
        </choices>
    </items>
</xf:instance>
```

The user can expand or collapse tree nodes. This doesn't have an impact on the currently-selected value:

![Expanded tree](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC804gtP\_L8vntLH0%252Fxbl-tree-select1-expanded-selected.png%3Fgeneration=1528743368553644%26alt=media\&width=300\&dpr=4\&quality=100\&sign=3f2907f816f971472a50b3b198951160a35dfad644352fdfaea7b82a35247c28)

### Readonly appearance <a href="#readonly-appearance" id="readonly-appearance"></a>

If the control is readonly, it appears grayed out. It is not possible to select items in this case, but it is possible to open/close tree nodes:

![Expanded tree in readonly mode](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC80637gvHO04649N%252Fxbl-tree-select1-expanded-selected-readonly.png%3Fgeneration=1528743368559242%26alt=media\&width=300\&dpr=4\&quality=100\&sign=bb25adc275727bb5d55b0f0abcadb0483d814f69116528274f849990afa95769)

### Controlling which tree nodes are initially open <a href="#controlling-which-tree-nodes-are-initially-open" id="controlling-which-tree-nodes-are-initially-open"></a>

The `<xf:itemset>` element supports the `xxf:open` extension attribute, which determines which nodes of the tree are open when the tree is first rendered. You can use `<xf:itemset xxf:open="true">` if you want all the nodes to be open. If you have a condition under which nodes should be open, use an AVT. For instance, the following will open just the first level of the tree. If you wanted to open all the nodes up to 3 levels of depth, you would instead write `{count(ancestor-or-self::item) le 3}`.

```
<xf:select ref="value">
    <xf:itemset
            ref="instance('tree-items')//item"
            xxf:open="{count(ancestor-or-self::item) = 1}">
        <xf:label ref="@label"/>
        <xf:value ref="@value"/>
    </xf:itemset>
</xf:select>
```

The result:

![Tree with first level open only](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC80Ao1ljq-rYIu9h%252Fxbl-tree-select1-initially-open.png%3Fgeneration=1528743368464198%26alt=media\&width=300\&dpr=4\&quality=100\&sign=8e6d8b2030c8a8603c6a8dd9f23860ef427d37cf3ad565a852217b644381aeb1)

### Adding class attributes <a href="#adding-class-attributes" id="adding-class-attributes"></a>

You can use an AVT in the the `class` attribute on the `<xf:itemset>` element to produce different CSS classes for different nodes.

```
class="my-class-{position()}" 
```

### Static-readonly mode <a href="#static-readonly-mode" id="static-readonly-mode"></a>

When used in Form Runner's readonly modes, the control doesn't show a tree but a field with the label associated with the user selection.

### Usage from plain XForms <a href="#usage-from-plain-xforms" id="usage-from-plain-xforms"></a>

You need the following configuration to use from a plain XForms (non-Form Runner) page, as the component's JavaScript is part of Form Runner:

```
<xh:script type="text/javascript" src="/apps/fr/resources/scalajs/forms-form-runner.js"/>
```

And then, on your main XForms model, put the `xxf:assets.baseline.excludes` attribute like this:

```
<xf:model xxf:assets.baseline.excludes="/ops/javascript/scalajs/forms-xforms.js">
```

### Custom styling <a href="#custom-styling" id="custom-styling"></a>

The implementation of the `tree` appearance uses the [Fancytree](https://github.com/mar10/fancytree) JavaScript library.

However you can look at the markup produced by Fancytree with your browser's development tools and apply custom CSS to style the tree. All icons are set with CSS. Here is an example of the HTML layout:

Copy

```
 <ul class="ui-fancytree fancytree-container fancytree-plain" tabindex="0">
    <li class="">
        <span class="fancytree-node fancytree-expanded fancytree-folder fancytree-has-children fancytree-exp-e fancytree-ico-ef">
            <span class="fancytree-expander"/>
            <span class="fancytree-icon"/>
            <span class="fancytree-title">Item 1</span>
        </span>
        <ul>
            <li class="">
                <span class="fancytree-node fancytree-folder fancytree-has-children fancytree-exp-c fancytree-ico-cf">
                    <span class="fancytree-expander"/>
                    <span class="fancytree-icon"/>
                    <span class="fancytree-title">Item 1.1</span>
                </span>
            </li>
            <li class="fancytree-lastsib">
                <span class="fancytree-node fancytree-folder fancytree-has-children fancytree-lastsib fancytree-exp-cl fancytree-ico-cf">
                    <span class="fancytree-expander"/>
                    <span class="fancytree-icon"/>
                    <span class="fancytree-title">Item 1.2</span>
                </span>
            </li>
        </ul>
    </li>
    <li class="fancytree-lastsib">
        <span class="fancytree-node fancytree-expanded fancytree-folder fancytree-has-children fancytree-lastsib fancytree-exp-el fancytree-ico-ef">
            <span class="fancytree-expander"/>
            <span class="fancytree-icon"/>
            <span class="fancytree-title">Item 2</span>
        </span>
        <ul>
            <li class="">
                <span class="fancytree-node fancytree-folder fancytree-has-children fancytree-exp-c fancytree-ico-cf">
                    <span class="fancytree-expander"/>
                    <span class="fancytree-icon"/>
                    <span class="fancytree-title">Item 2.1</span>
                </span>
            </li>
            <li class="fancytree-lastsib">
                <span class="fancytree-node fancytree-folder fancytree-has-children fancytree-lastsib fancytree-exp-cl fancytree-ico-cf">
                    <span class="fancytree-expander"/>
                    <span class="fancytree-icon"/>
                    <span class="fancytree-title">Item 2.2</span>
                </span>
            </li>
        </ul>
    </li>
</ul>
```
