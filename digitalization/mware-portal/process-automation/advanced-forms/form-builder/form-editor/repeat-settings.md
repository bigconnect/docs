# Repeat settings

### Overview <a href="#overview" id="overview"></a>

Form Builder supports grids and sections with repeated content.

* With grids, you can repeat a single row (which is the most common case), or multiple heterogeneous rows (with different controls).
* With sections, you can repeat the entire content of the section a number of times.

Both repeated grids and repeated sections have _repeat settings_ in the "Section/Grid Settings" dialog's "Repeated Content" tab.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-04ece611ab224bbe7853e0aed7205fa670ae7feb%252Fcontainer-settings-repeated-content.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=8c1863c1f882bdd602a32884ed6fc69348620c3943ed6125c231341de6e8e68f)

Repeated Content

### Repeat Content <a href="#repeat-content" id="repeat-content"></a>

Select this checkbox to enable repeated content. This enables a series of detailed options below in the same dialog.

### Allow the user to add, remove, or move repetitions <a href="#allow-the-user-to-add-remove-or-move-repetitions" id="allow-the-user-to-add-remove-or-move-repetitions"></a>

\[SINCE Orbeon Forms 2019.1]

By default, the form user is allowed to add, remove, or move repetitions. To disable this behavior, deselect the "Allow the user to add, remove, or move repetitions" option. When the checkbox is deselected, the user does not have direct access to buttons, icons or menus to change repetitions.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-dd1d3729354488e3645868a88e64640dfac375d9%252Fcontainer-settings-repeated-content-one-repetition-highlighted.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=e956e5fed82825aa89890f33d6b17d7f92bb2a70dfbac73d2e0be55c1af42177)

Allow the user to add, remove, or move repetitions

When this option is deselected, the minimum, maximum and frozen repetitions settings do not apply.

Deselecting this option makes sense in particular when the number of repetitions:

* is defined at design-time,
* is controlled via [actions](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions-syntax),
* or is controlled via [synchronization](https://doc.orbeon.com/form-builder/advanced/services-and-actions/synchronize-repeated-content).

### Show repetition number <a href="#show-repetition-number" id="show-repetition-number"></a>

\[SINCE Orbeon Forms 2021.1]

For grids only, selecting this option automatically shows a row number at the beginning of each grid repetition.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-2fd0f10ebf2ccf1440d14130a27293aadec0c589%252Fcontainer-settings-repeated-content-show-repetition-number-highlighted.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=9404ddb7ceb1eea3afc544e1bcfb5729e6b0c117e6d68f76c6af754380100210)

Show repetition number

This is how the repetition number appears at runtime.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F0e33ddf3757d417119b98477ef21b21c7ea02b12.png%3Fgeneration=1632353418370580%26alt=media\&width=768\&dpr=4\&quality=100\&sign=51268831f65500958f48b52ca80f1c68da1ffec33142748a009416c6abfc57e5)

Repetition number at runtime

### Minimum and maximum number of repetitions <a href="#minimum-and-maximum-number-of-repetitions" id="minimum-and-maximum-number-of-repetitions"></a>

These settings can be predefined numbers or formulas when selecting "Other". Here is an example of using a firmula for the maximum number of repetitions.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-b90418f2cb6e4fe21058d278ab2af13dd541f73d%252Fcontainer-settings-repeated-content-maximum-other-highlighted.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=3a8d0e1d30fb06e6db70749a44821b9cb6f2147b68f8168ca64fc77e94f15470)

Formula for the maximum number of repetitions

### Freeze repetitions <a href="#freeze-repetitions" id="freeze-repetitions"></a>

\[SINCE Orbeon Forms 2018.2]

This setting can be a predefined number or a formula.

This allows _freezing_ the first _N_ iterations of a repeated grid or repeated section. Frozen iterations cannot be removed or moved by the user. The grid menus and buttons reflect that those operations are not possible.

The number of frozen iterations must be at most the minimal number of repetitions. If that's not the case, the number of frozen iterations will be reset to the minimal number of repetitions.

See also [Freezing rows in repeated grids or sections](https://blog.orbeon.com/2019/06/freezing-rows-in-repeated-grids-or.html).

### Simplified appearance <a href="#simplified-appearance" id="simplified-appearance"></a>

\[SINCE Orbeon Forms 2019.1]

When selected, this enables a simplified appearance for a repeated grid or section.

The default is the "full" appearance, which features:

* visible borders
* menus for inserting, removing, and moving rows

When enabled, the simplified (or "minimal") appearance features:

* no visible borders
* no ability to insert a new row at a specific position
* no ability to reorder rows
* simple buttons/icons to add/remove rows

Before Orbeon Forms 2019.1, the simplified appearance was available but only controllable via a property. See [Appearance of repeated grids](https://doc.orbeon.com/configuration/properties/form-runner#appearance-of-repeated-grids) and [Appearance of repeated sections](https://doc.orbeon.com/configuration/properties/form-runner#appearance-of-repeated-sections).

### Initial value formulas <a href="#initial-value-formulas" id="initial-value-formulas"></a>

\[SINCE Orbeon Forms 2016.1]

The "Apply initial value formulas when adding iterations" option specifies whether the "Initial Value" formulas for controls within the grid are evaluated for new iterations.

With the option enabled, new iterations can have dynamic initial values:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44pn\_QEqocCsQAR%252Fiterations-initial-values.png%3Fgeneration=1528743352962408%26alt=media\&width=768\&dpr=4\&quality=100\&sign=7a10767c7a2984dea82e1a4402af43a59f86b0e9749d2973476363a1a65d54f6)

Initial Values

With Orbeon Forms 2016.1, the option is enabled by default for new forms and new repeated grids. The option is disabled by default for grids created with previous versions of Orbeon Forms.

### Initial number of iterations uses template <a href="#initial-number-of-iterations-uses-template" id="initial-number-of-iterations-uses-template"></a>

\[SINCE Orbeon Forms 2016.1]

The "Initial Number of Iterations Uses Template" option specifies, when an _enclosing repeated section_ creates a new iteration, how many iterations this repeated grid will contain:

* when enabled: the number of iterations shown in Form Builder (which can be no iterations at all, one iteration, two iterations, etc.)
* when disabled: exactly one iteration

The following screenshot shows a case with a repeated grid within nested repeated sections. At first, when the form shows, there are two iterations of the repeated grid.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44rfyj3jsSLyNVT%252Fiterations-initial.png%3Fgeneration=1528743353025211%26alt=media\&width=768\&dpr=4\&quality=100\&sign=e791bdb0c17867f8151a983195f018e73c5a99035b65f3afbee04dc3ed00e4ae)

With the option enabled on the grid, adding a new iteration of _Repeated section 2_ causes the new iteration to contain a new repeated grid with two iterations:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44tJLL2Lndoq6Es%252Fiterations-template.png%3Fgeneration=1528743362705135%26alt=media\&width=768\&dpr=4\&quality=100\&sign=83d8328fc55991206b80a55598e3fbd52bb35bfc3b0e253f4687b877f0516e52)

While, with the option disabled on the grid, adding a new iteration of _Repeated section 2_ causes the new iteration to contain a new repeated grid a single iterations:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC44v63zQxNjAJWeQ%252Fiterations-single.png%3Fgeneration=1528743362123927%26alt=media\&width=768\&dpr=4\&quality=100\&sign=04075df7c505289fb555cb230e8cf38cc3e85442eda933e3d956a7b260ab69bb)

### Show one repetition at a time in the wizard <a href="#show-one-repetition-at-a-time-in-the-wizard" id="show-one-repetition-at-a-time-in-the-wizard"></a>

\[SINCE Orbeon Forms 2019.1]

This option can be selected only when the "Allow the user to add, remove, or move repetitions" option is deselected.

For details, see [Paging large repeated sections](https://doc.orbeon.com/form-runner/features/wizard-view#paging-large-repeated-sections).
