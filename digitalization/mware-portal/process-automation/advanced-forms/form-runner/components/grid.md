# Grid

The `fr:grid` component organizes contained controls in a grid of rows and columns. Features:

* configurable number of rows and columns
* optional repetition of its content
  * configurable min/max number of iterations
  * can repeat over several heterogeneous rows
  * built-in icons and menus to add, remove, and move repeated rows
  * supports `relevant` and `readonly` MIPs

### Appearance <a href="#appearance" id="appearance"></a>

#### Full repeat appearance <a href="#full-repeat-appearance" id="full-repeat-appearance"></a>

**Menu**

A menu is available to perform operations on or around a line. Not all operations are always available, depending on the position of the line or the `freeze` setting. When the entire grid is made readonly, the menu is not present.

* Insert Above
  * When allowed, insert a new repetition above the current one.
* Insert Below
  * When allowed, insert a new repetition below the current one.
* Move Up
  * When allowed, move the current repetition one level above.
* Move Down
  * When allowed, move the current repetition one level below.
* Clear Line
  * Clear or reset all the values in the current repetition.
* Remove Line
  * When allowed, remove the current repetition.

![Repeated grid menu](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LxNZd9pylI96dTuT4G-%252F-LxNZnu6lBwB2OfvmEeT%252Fxbl-grid-menu.png%3Fgeneration=1577739044203862%26alt=media\&width=768\&dpr=4\&quality=100\&sign=8ab245f03bef547985f09c529b1399b4149446eba82551c743772e2d5de15ddf)



**Single-line**

![Full appearance / single-line](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkCAbnFWxn72b7jx7n%252Fxbl-grid-full-single-line.png%3Fgeneration=1528743382406409%26alt=media\&width=768\&dpr=4\&quality=100\&sign=da2c3699713903110401d3b9c93b186c3f31d439a4f057329fd1a3e769f601d5)



**Multi-line**

![Full appearance / multi-line](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkCAbtYoXC\_rSD\_Sh6%252Fxbl-grid-full-multi-line.png%3Fgeneration=1528743382493360%26alt=media\&width=768\&dpr=4\&quality=100\&sign=5925460e2649e42e048dffeaa1ef7e66391f746450a740d336ad55622382c9ba)



#### Minimal repeat appearance <a href="#minimal-repeat-appearance" id="minimal-repeat-appearance"></a>

![Minimal appearance / single-line](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkCAbxpOZ4nAZGiMhn%252Fxbl-grid-minimal-single-line.png%3Fgeneration=1528743382431710%26alt=media\&width=768\&dpr=4\&quality=100\&sign=a1cd469b3c9ac2976542d8c3092f7ef33d72699802039210677b8ed4a1430b13)



![Minimal appearance / multi-line](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkCAc0FSqbbZWCLF8J%252Fxbl-grid-minimal-multi-line.png%3Fgeneration=1528743382491075%26alt=media\&width=768\&dpr=4\&quality=100\&sign=5bb20eb5ee3e57285f5074ecfd082cf8cb7375aa854405d76ebbbe97ddafb6da)



### Basic usage <a href="#basic-usage" id="basic-usage"></a>

#### Format of rows and cells <a href="#format-of-rows-and-cells" id="format-of-rows-and-cells"></a>

The format for grid rows and cells produced by Form Builder has changed. Instead of using `<xh:tr>` and `<xh:td>`, it uses `<fr:c>`. `<xh:tr>` and `<xh:td>` are still supported for backward compatibility.

The following attributes are supported on `<fr:c>`:

* `x`:
  * x position
  * required
  * minimum: 1
  * maximum: 12
* `y`:
  * y position
  * required
  * minimum: 1
  * maximum: none
* `w`:
  * width of the cell
  * required
  * minimum: 1
  * maximum: 12
* `h`:
  * height of the cell
  * optional, defaults to 1
  * minimum: 1
  * maximum: none

The following must hold:

* cells must not overlap
* cells must not extend beyond column 12

In other words, the grid must be well-formed.

However, the grid may have holes, and not all cells need be present.

#### Non-repeated mode <a href="#non-repeated-mode" id="non-repeated-mode"></a>

**General information**

Attributes:

* `id`: optional grid id. Form Builder always places an id.
* `repeat`: can be optionally set to `false`

_NOTE: In the future `bind` and `ref` should be supported._

```
<fr:grid id="note-grid">
    <fr:c y="1" x="1" w="12">
        <!-- A form control will be placed here -->
    </fr:c>
</fr:grid>
```

Example with multiple cells spanning rows and columns:

```
<fr:grid id="grid-1-grid">
    <fr:c y="1" x="1" w="6">
        <!-- A form control will be placed here -->
    </fr:c>
    <fr:c y="1" x="7" w="6" h="2">
        <!-- A form control will be placed here -->
    </fr:c>
    <fr:c x="1" y="2" w="3">
        <!-- A form control will be placed here -->
    </fr:c>
    <fr:c x="4" y="2" w="3">
        <!-- A form control will be placed here -->
    </fr:c>
</fr:grid>
```

#### Repeated mode <a href="#repeated-mode" id="repeated-mode"></a>

**General information**

Attributes:

* `id`: optional grid id.
* `repeat`
  * `true`: legacy repeat mode
  * `content`: new repeat mode with an enclosing element
* `bind` or `ref`
  * with `repeat="content"`: single item binding which binds to the enclosing element
  * with `repeat="true"`: item sequence binding which binds to the repeated elements
* `template`
  * XML data template used to insert new data when iterations are inserted
  * optional when the grid is readonly
* `min` and `max`
  * minimum vs. maximum number of repeat iterations supported
  * can be an AVT
  * evaluation context
    * with `repeat="content"`: context of the binding (`bind` or `ref`)
    * with `repeat="true"` (deprecated): context of the `fr:grid` element
  * are checked when the user attempts to add/remove iterations with the UI
  * `max` can also take the explicit value `none`
* `remove-constraint`
  * optional XPath expression
  * evaluation context
    * context of the iteration item
  * if the constraint returns `false()`, the current row cannot be removed
* `clear-constraint`
  * optional XPath expression
  * evaluation context
    * context of the iteration item
  * if the constraint returns `false()`, the "Clear Line" menu item is disabled
* `freeze`
  * optional number of rows at the top which cannot be removed or moved
  * can be an AVT&#x20;
  * evaluation context
    * with `repeat="content"`: context of the binding (`bind` or `ref`)
    * with `repeat="true"`: context of the `fr:grid element`
* `readonly`
  * force the grid to be readonly when set to `true`
  * this cannot be an AVT
* `apply-defaults`
  * `true`: dynamic initial values (via the `xxf:default` MIP) apply to new iterations
  * missing or \`false: dynamic initial values do not apply to new iterations
  * can be an AVT
* `appearance`
  * `full`
    * the default appearance
    * row menu
      * reordering of rows
      * insertion of rows at specific points
      * removing of specific rows
  * `minimal`
    * does not show the "+" button at the top left
    * does not show the row menu and associated features
    * instead just provides "Add another" and "Remove" links at the bottom
* `insert`&#x20;
  * `index`
    * the "Add Another" or "+" button adds a new repetition after the iteration currently with keyboard focus
    * this is the default in `full` appearance
  * `bottom`
    * the "Add Another" or "+" button adds a new repetition after the last iteration
    * this is the default in `minimal` appearance

```
<fr:grid
    id="note-grid"
    repeat="content"
    bind="note-bind"
    template="instance('note-template')"
    min="0">
    <fr:c y="1" x="1" w="12">
        <!-- A form control will be placed here -->
    </fr:c>
</fr:grid>
```

### Data format <a href="#data-format" id="data-format"></a>

With `repeat="content"`:

* An enclosing element is present.
* Nested elements, typically suffixed with `-iteration`, enclose each repetition.

```
<note>
    <note-iteration>
        <note-text/>
    </note-iteration>
    <note-iteration>
        <note-text/>
    </note-iteration>
</note>
```

With legacy `repeat="true"`:

* No enclosing element is present.
* Elements are repeated at the top-level.

```
<note>
    <note-text/>
</note>
<note>
    <note-text/>
</note>
```

A template instance contains the XML data to insert for a new iteration:

```
<xf:instance id="note-template">
    <note-iteration xmlns="">
        <note-text/>
    </note-iteration>
</xf:instance>
```

### Relevance and readonly behavior <a href="#relevance-and-readonly-behavior" id="relevance-and-readonly-behavior"></a>

With `repeat="content"`:

* if the bound item is non-relevant, the grid is entirely hidden
* if the bound item is readonly, the grid is entirely readonly and menus and icons don't show

In either case, `fr:grid` contains the rows to repeat as `xh:tr`. Each `xh:tr` contains `xh:td` cells as needed.

### Events <a href="#events" id="events"></a>

The following events are dispatched to the `fr:grid` element:

| Event name             | Description                                            |
| ---------------------- | ------------------------------------------------------ |
| `fr-iteration-added`   | Dispatched when the user has just added an iteration   |
| `fr-iteration-removed` | Dispatched when the user has just removed an iteration |

These events are not dispatched if the number of iterations changes by other means, for examle if the data is replaced, or inserts/deletes happen outside of the component.

Both events take an `index` context information:

* `event('index') as xs:integer`: the index of the row added or removed
