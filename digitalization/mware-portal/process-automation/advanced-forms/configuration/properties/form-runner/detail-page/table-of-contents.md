# Table of contents

### Introduction <a href="#introduction" id="introduction"></a>

The table of contents is a list of sections in the form. It is shown at the top or left of the form, and allows users to navigate to a particular section.

<div align="left" data-full-width="false">

<img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-ef0510d2556ea82071412c8d43335f8d45d39eb9%252Ftoc-top.png%3Falt=media&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=2537dcc248c08f57d902d39fe5d9105781124e15a6a34e3f72537e9e7455ccef" alt="Table of contents on top">

</div>

Note the following:

* The following configuration properties only relate to the use of the table of contents when the Wizard view is not used.
* When using the Wizard view, a table of contents is handled by the Wizard itself and the global Table of Contents discussed here is disabled.
* In View and PDF modes, the Wizard view is never in use.

### Show table of contents <a href="#show-table-of-contents" id="show-table-of-contents"></a>

```
<property
    as="xs:integer"
    name="oxf.fr.detail.toc.min-sections.*.*"
    value="0"/>
```

If the number of table of contents entries (that is, of form sections) is greater than this value, then show the Table of Contents at the top of the form.

This can be omitted or set to -1 to never show the TOC.

### Position of the table of contents <a href="#position-of-the-table-of-contents" id="position-of-the-table-of-contents"></a>

```
<property
    as="xs:string"
    name="oxf.fr.detail.toc.position.*.*"
    value="top"/>
```

Where to place the table of contents: `top`, `left`, or `none`. The default is `top`. If neither `top` nor `left` is specified, the table of contents is not shown.

![Table of contents on the left](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-403c72f66f246788f8b8cf473ac13ba954579fa2%252Ftoc-left.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=75bb5981e624992cffdc88ca9a8b4ea9cdf87cd9b14105d5e84bf03f692cdfe6)

In PDF mode, the `left` position is not supported and the table of contents is always shown at the top.

![Table of contents on top in PDF files](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-9facc29f78badc53505002299b30fdf0e6e354c1%252Ftoc-top-pdf.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=443d3fa37f0af04b50766377d19bc5c493f4ecafb0957c54a1b87c6fcefa7c41)

### Modes in which the table of contents is shown <a href="#modes-in-which-the-table-of-contents-is-shown" id="modes-in-which-the-table-of-contents-is-shown"></a>

```
<property
    as="xs:string"
    name="oxf.fr.detail.toc.modes.*.*"
    value="new edit"/>
```

The table of contents is shown in the specified modes only. The default is `new edit view`. The following modes are available: `new`, `edit`, `view`, `pdf`, `tiff`.
