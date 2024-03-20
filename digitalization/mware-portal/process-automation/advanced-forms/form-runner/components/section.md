# Section

The `fr:section` component organizes grids under a header or title.&#x20;

Features:

* collapsible section content
* optional repetition of its content
  * configurable min/max number of iterations
  * can repeat over several heterogeneous rows
  * built-in icons and menus to add, remove, and move repeated rows

### Appearance <a href="#appearance" id="appearance"></a>

#### Non-repeated appearance <a href="#non-repeated-appearance" id="non-repeated-appearance"></a>

An open, collapsible section shows a "collapse" icon at the left of the heading.

![Open section](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LL5f7RgoXiXvVKRMgo4%252F-LL5fFrp3zrt2iQRL6qn%252Fxbl-section-open.png%3Fgeneration=1535562815480459%26alt=media\&width=768\&dpr=4\&quality=100\&sign=01432ed3eb26f2fa4c43a5dc622c3164f71795391365170d0d6e7d810c90e2bf)



The same section when closed just shows the heading.

![Closed or collapsed section](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LL5f7RgoXiXvVKRMgo4%252F-LL5fFrrTxnUUNZqnZpy%252Fxbl-section-closed.png%3Fgeneration=1535562815098586%26alt=media\&width=768\&dpr=4\&quality=100\&sign=d34da2ae197f1b8e0c6075b610cf8ff21326a7df8f2a9e6106dfa53b5489baeb)



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

![Repeated section menu](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LxNZd9pylI96dTuT4G-%252F-LxNZnu6lBwB2OfvmEeT%252Fxbl-grid-menu.png%3Fgeneration=1577739044203862%26alt=media\&width=768\&dpr=4\&quality=100\&sign=8ab245f03bef547985f09c529b1399b4149446eba82551c743772e2d5de15ddf)



**Appearance**

![Full appearance](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LL5gPVZwk84cOQApInB%252F-LL5fFrtIwfQ0-rpmSI4%252Fxbl-section-repeat-full.png%3Fgeneration=1535563155219681%26alt=media\&width=768\&dpr=4\&quality=100\&sign=3faa0a7fbb976924289592a1b91e91f9314ac53dcc91830bca54ac16b6e4e1ad)



#### Minimal repeat appearance <a href="#minimal-repeat-appearance" id="minimal-repeat-appearance"></a>

![Minimal appearance](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LL5f7RgoXiXvVKRMgo4%252F-LL5fFrvj4uF\_KpKdUED%252Fxbl-section-repeat-minimal.png%3Fgeneration=1535562815420972%26alt=media\&width=768\&dpr=4\&quality=100\&sign=22305542eaeb1d68a055aa8e08fbe100deccd682fe75a6f06eb66ec4fcccca17)



### Basic usage <a href="#basic-usage" id="basic-usage"></a>

#### Non-repeated mode <a href="#non-repeated-mode" id="non-repeated-mode"></a>

These attributes are supported when the section is in non-repeated mode.

* `open`
  * whether the section is initially open
  * `true` (default)
    * section is initially open
  * `false`
    * section is initially closed
  * _NOTE: When the Form Runner mode is readonly ("view" mode, "pdf" mode, etc.), `open` is forced to `true`._
* `animate`
  * whether the section animates when collapsing/expanding
  * `true` (default)
    * section animates
  * `false`
    * section does not animate
* `collapsible`&#x20;
  * whether the section can be collapsed and expanded by clicking on its title
  * `true` (default)
    * allow section to be collapsed
  * `false`
    * don't allow section to be collapsed
  * _NOTE: If the section is initially closed and `collapsible` is `false`, the user won't be able to open the section._
* `level`
  * level of the section, from `1` to `6`
  * default: `2`
  * alongside `base-level`, this value translates into an HTML heading `h1` to `h6`
* `base-level`
  * base level of the section, from `0` to `5`
  * default: `0`
  * when set to a non-zero value, this value is added to the value of `level` to produce the actual section level

#### Repeated mode <a href="#repeated-mode" id="repeated-mode"></a>

These attributes are supported in addition to the non-repeated mode attributes when the section is in repeated mode:

* `freeze`
  * optional number of rows at the top which cannot be removed or moved
  * can be an AVT&#x20;
* `readonly`
  * whether to disallow the user to add/remove iteration with the menu or the "-" button (depending on appearance)
  * `false` (default)
    *   show the iteration menu and buttons, unless the section binds to a readonly node or unless the Form Runner

        mode is readonly ("view" mode, "pdf" mode, etc.)
  * `true` (default)
    * don't show the iteration menu and buttons
* `appearance`&#x20;
  * `full`
    * the default appearance
    * iteration menu
      * reordering of iterations
      * insertion of iterations at specific points
      * removing of specific iterations
  * `minimal`
    * does not show the "+" button at the top left
    * does not show the iteration menu and associated features
    * instead just provides "Add another" and "Remove" links at the bottom and a "-" button to the right
* `insert`&#x20;
  * `index`
    * the "Add Another" or "+" button adds a new repetition after the iteration currently with keyboard focus
    * this is the default in `full` appearance
  * `bottom`
    * the "Add Another" or "+" button adds a new repetition after the last iteration
    * this is the default in `minimal` appearance

You move, insert, and remove iterations using the left menu:

![Menu](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LL5f7RgoXiXvVKRMgo4%252F-LL5fFsA63T9kFXWkDGy%252Fxbl-section-repeat-menu.png%3Fgeneration=1535562815284997%26alt=media\&width=768\&dpr=4\&quality=100\&sign=797b62030f4c32947a691ff79ddaf25265b27861ef0879fea0c2ef0388bb41ca)



### Data format <a href="#data-format" id="data-format"></a>

See Form data format.

### Events <a href="#events" id="events"></a>

#### Events supported <a href="#events-supported" id="events-supported"></a>

| Event name    | Description          |
| ------------- | -------------------- |
| `fr-expand`   | Expand the section   |
| `fr-collapse` | Collapse the section |

#### Events dispatched <a href="#events-dispatched" id="events-dispatched"></a>

The following events are dispatched to the `fr:section` element:

| Event name             | Description                                            |
| ---------------------- | ------------------------------------------------------ |
| `fr-iteration-added`   | Dispatched when the user has just added an iteration   |
| `fr-iteration-removed` | Dispatched when the user has just removed an iteration |

These events are not dispatched if the number of iterations changes by other means, for examle if the data is replaced, or inserts/deletes happen outside of the component.
