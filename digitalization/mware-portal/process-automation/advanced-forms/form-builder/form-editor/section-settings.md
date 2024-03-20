# Section settings

### Settings shared with grids <a href="#settings-shared-with-grids" id="settings-shared-with-grids"></a>

See [Section and grid settings](section-and-grid-settings.md).

### Basic settings <a href="#basic-settings" id="basic-settings"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-11604b458c173c3982e89da1966b210e98fb0056%252Fsection-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=3cfd2923875262a890477ab9073a5c426f73737e127d37de5bcac707d088b68e)

#### Section collapsing <a href="#section-collapsing" id="section-collapsing"></a>

The following only applies when the form doesn't use the Wizard view.

A section can be open/expanded or closed/collapsed. By default, sections are open when the form loads, unless the "Initially open" checkbox is _deselected_.

The "Collapsible" radio buttons control whether the user can collapse and expand sections:

* **Use property:** use the configuration specified with the `oxf.xforms.xbl.fr.section.collapsible` property
* **Always:** the section is collapsible no matter what the `oxf.xforms.xbl.fr.section.collapsible` property specifies
* **Never:** the section is not collapsible no matter no matter what the `oxf.xforms.xbl.fr.section.collapsible` property specifies

### Repeat settings <a href="#repeat-settings" id="repeat-settings"></a>

See [Repeat settings](repeat-settings.md).

### Label, help message, and short labels <a href="#label-help-message-and-short-labels" id="label-help-message-and-short-labels"></a>

The label can be configured directly in the form area, by clicking on the section title, or in the "Label" tab.

The help message is configured in the "Help Message" tab. Similarly, the help message can be in plain text or use rich text (HTML).

A short label can be configured in the "Short Label" tab. This short label is used instead of the regular label in the table of contents when using the wizard view.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-bf36d1bce35e4543d9b2013188bb7c4770834493%252Fsection-settings-short-label-1.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=ecdb2cb9855921970f4b1856a8b6c16ae22776675137a7cdd2b2df92b3d98b8c)

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-dc44e0715d8f3fca24026e88db0d3cd3162f4922%252Fsection-settings-short-label-2.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=f2d886b53c0825799621d1227999971ecd2dd16320732d2c6e0bf44ccd3132ef)

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-d08d28759fd5a7cbf1ccb600c348e2bf6cf4a53d%252Fsection-settings-short-label-3.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=2f49c375a9afbcab23d5a0f82c5a3569d054af347323ce91d460d97e596dfc98)

#### Dynamic label and help message <a href="#dynamic-label-and-help-message" id="dynamic-label-and-help-message"></a>

In most cases, labels and help messages are simply localized messages without dynamic parts.

However, sections also support _dynamic_ labels and help messages. This means that, instead of being specified once and for all at form design time, labels and help messages can incorporate dynamic parts such as control values and other custom expressions.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-459f62aaf0dfbe7f3824d9230f0f42ab3a626485%252Fsection-settings-label.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=f9344675203c098e5331a64955c44c05d18355e8b0b35f495de5b7f1f5e5e0ef)

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-43b43e5c4654804629c4fdb5d9e7b27cc724dfa2%252Fsection-settings-help.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=7029319c4de219a9ec95b968aaa5d9c2dc3f1e546c4d1ff2896c91e6040fe2c5)

#### Dynamic iteration label <a href="#dynamic-iteration-label" id="dynamic-iteration-label"></a>

For sections with repeated content, the "Repetition Label" tab allows you to set a label that applies to individual repetitions. Typically, this label will use a template so that values from the repeated content can be used.

When using the Wizard view, repetition labels will show in the Wizard's table of contents.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-afc9df258a1e7e3970894a38d43442a4fd992f06%252Fsection-settings-repetition-label.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=08b9e38593ac82b010d985fdeafddbc6cdbab0c0ff904ad716e69f94a684a44c)

#### Custom "Add Repetition" label <a href="#custom-add-repetition-label" id="custom-add-repetition-label"></a>

For sections with repeated content, the "Add Repetition Label" tab allows you to set a label that applies to the button used to add repetitions.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-232bfcfc234b465a543f2c3a08c0895e60d720d2%252Fsection-settings-add-repetition-label.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=9b4afe1fe2a1c55b21a12a415bff9f10169976d348ee061c6b32cf5eed2a060b)
