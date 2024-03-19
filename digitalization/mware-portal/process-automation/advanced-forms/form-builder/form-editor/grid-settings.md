# Grid settings

### Settings shared with sections <a href="#settings-shared-with-sections" id="settings-shared-with-sections"></a>

See [Section and grid settings](https://doc.orbeon.com/form-builder/form-editor/container-settings).

### Basic settings <a href="#basic-settings" id="basic-settings"></a>

#### Overview <a href="#overview" id="overview"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-c2b8a0eb7517089e7488335444473abce85a6bdd%252Fgrid-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=103e9b7997ca8ea188e639fa8c2e667cf86c3bd56d46a16f141aa14fde2f5b82)

#### Number of Grid Columns <a href="#number-of-grid-columns" id="number-of-grid-columns"></a>

\[SINCE Orbeon Forms 2020.1]

Orbeon Forms introduced a [12-column layout](https://doc.orbeon.com/form-builder/form-editor/form-area#the-12-column-layout) with Orbeon Forms 2017.2.

It is now possible to control whether an individual grid has 24 columns instead of 12. This is useful in particular when using a fluid form layout, which can be wider and accommodate more form controls on a given line.

The default remains 12 columns. To change to 24 columns, choose the "24" option. The grid then allows moving cell boundaries on a 24-column resolution.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-M-Gk22Iaku1NxbrUkqC%252F-M-GkArkwzRx\_\_RWabFT%252Fgrid-12-columns.png%3Fgeneration=1580845810648695%26alt=media\&width=768\&dpr=4\&quality=100\&sign=6145295ef7a5b384efdd7deac43dda8505fe56d9d1ee13afbe1647630cc1c049)

Moving cell boundaries with a 12-column grid

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-M-Gk22Iaku1NxbrUkqC%252F-M-GkArnVKoTt-mtxabr%252Fgrid-24-columns.png%3Fgeneration=1580845810228062%26alt=media\&width=768\&dpr=4\&quality=100\&sign=e2c623d3238958e4a2eb3e4af5a8817b46a4ebebfa81a07f94c9f22457f15f29)

Moving cell boundaries with a 24-column grid

When going from 12 to 24 columns, the grid cells positions and widths are adjusted so that the grid looks the same. Similarly, when going from 24 to 12 columns, the grid cells positions and widths are adjusted so that the grid looks the same. However, if the grid contains controls whose position or width does not allow migrating the grid back to 12 columns, the "12" option is disabled.

#### Number of Grid Rows <a href="#number-of-grid-rows" id="number-of-grid-rows"></a>

\[SINCE Orbeon Forms 2021.1]

The dialog shows the current number of grid rows. This is the "static" number of grid rows, independently of the number of grid repetitions in the case of repeated grids.

See also [Enhancements to grids](https://blog.orbeon.com/2021/09/enhancements-to-repeated-grids.html).

#### Grid Tab Order <a href="#grid-tab-order" id="grid-tab-order"></a>

[\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)

Cells inside a grid can be ordered in two ways:

* By row first, then by column
* By column first, then by row

Ordering the cells by row is the default behaviour.

Ordering the cells by column impacts the tab order (i.e. the order in which cells are navigated using the tab key) and the order in which the cells are placed on devices with a narrow display (e.g. phones) when all cells are stacked up (responsive relayout).

A default value can be set for all grids in the [Form Settings dialog](https://doc.orbeon.com/form-builder/form-settings#form-options).
