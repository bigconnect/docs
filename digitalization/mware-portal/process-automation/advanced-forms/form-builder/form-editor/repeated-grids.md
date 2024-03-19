# Repeated grids

### Creating a repeated grid <a href="#creating-a-repeated-grid" id="creating-a-repeated-grid"></a>

You insert a new repeated grid with the "New Repeated Grid" toolbox button.

Once the grid is inserted, you can add and remove (using the grid arrow and trash icons which appear on mouseover) columns and rows, and add controls to grid cells as you would in a regular non-repeated grid.

With only one row, the control labels are used as column headers and are not repeated within the grid. If present, control hints are also added to the column headers. That single row is repeatable with the "plus" icon.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44IFCSIsQiBbifV%252Frepeated-grid-single.png%3Fgeneration=1528743352890729%26alt=media\&width=768\&dpr=4\&quality=100\&sign=65c7a6adeba80edc68dcf3bfae41eeb0b8e2e895be10ee5ec4bd06becd9f537d)

Repeating a single row

You can add multiple heterogeneous rows with the arrow icons. In this case the entire group of rows is repeatable. Control labels and hints do not appear in column headers, but appear alongside the controls in the grid.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44NNsucjF2bogFT%252Frepeated-grid-multiple.png%3Fgeneration=1528743356410586%26alt=media\&width=768\&dpr=4\&quality=100\&sign=0b7ed13bb5e05b4365013d33d7567ae8bf1cca3ba60b98a0cbe6a7fbae0edd5b)

Repeating multiple rows

Whether there is a single or multiple repeatable rows, you can add and remove repetitions (iterations) of those rows with the "plus" icon. You typically let the user add iterations at runtime, but it is possible to create iterations in advance at design time as well.

### How things look at runtime <a href="#how-things-look-at-runtime" id="how-things-look-at-runtime"></a>

At runtime, notice how in the first grid a single row is repeated, and in the second grid the two rows are repeated.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44R0\_pMGvDuWGuf%252Frepeated-grids.png%3Fgeneration=1528743362656748%26alt=media\&width=768\&dpr=4\&quality=100\&sign=f5e8e462f2d66d72a53854506d8b42fa679dff9652667a7f7278e31e679d3bd2)

Repeated grids

In review mode and PDF mode, icons and menus disappear and the grid appears entirely readonly.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44TljULuRoMgXgm%252Frepeated-grids-view.png%3Fgeneration=1528743352939403%26alt=media\&width=768\&dpr=4\&quality=100\&sign=b50228b42cb47dbe314d5f1b695d36bc0576ce7fc591db25feacbfc24f3ea15a)

Repeated grids in review mode

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44VTchgFp9lz443%252Frepeated-grids-pdf.png%3Fgeneration=1528743352961087%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1d7a44541ef0d517156244380edab5032948618e8388176e48fb1b8abea4e44d)

Repeated grids in PDF mode

### Grid settings <a href="#grid-settings" id="grid-settings"></a>

Once a grid is inserted, you can edit its properties with the "Grid Settings" icon.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44Xz\_uPl5TSk68d%252Frepeated-grid-settings-icon.png%3Fgeneration=1528743352878743%26alt=media\&width=768\&dpr=4\&quality=100\&sign=a55b9164c3007a5bb7dc19b77e8e39e91a17d4496cf859a99f3730d64d8a0e64)

Grid Settings

_NOTE: Since Orbeon Forms 4.8, these settings (correctly) apply to the entire grid. Previously, the grid's repeat headers did not hide properly for example when the grid was hidden. See issue_ [_#635_](https://github.com/orbeon/orbeon-forms/issues/635)_._

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-c2b8a0eb7517089e7488335444473abce85a6bdd%252Fgrid-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=103e9b7997ca8ea188e639fa8c2e667cf86c3bd56d46a16f141aa14fde2f5b82)

Basic Settings and Formulas

\[SINCE Orbeon Forms 2019.1]

These settings also apply to non-repeated grids.

#### Repeat settings <a href="#repeat-settings" id="repeat-settings"></a>

See [Repeat settings](https://doc.orbeon.com/form-builder/form-editor/repeat-settings).
