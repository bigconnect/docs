# Publishing

### Introduction <a href="#introduction" id="introduction"></a>

The notion of _publishing_ is central to Form Builder/Form Runner.

* As a form author, you first work on a form definition in a special space where the form can be modified, saved, and tested.
* Once the form definition is ready, you _publish it_ to Form Runner.
* After that moment:
  * the form becomes available by form users for data entry
  * Form Builder is no longer part of the equation

### First publish <a href="#first-publish" id="first-publish"></a>

When you start publishing a form with the Publish button at the bottom of Form Builder, a dialog opens to confirm the application name and form name:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwwFI7nLPJYpV74P%252Fpublish-initial-no-versioning.png%3Fgeneration=1528743327436235%26alt=media\&width=300\&dpr=4\&quality=100\&sign=b3979b421adbf1a7ec5c9dc310b8547c8b6a617eb15f26f263f5fcd96019248f)

If you decide to go ahead with publishing, simply use the Publish button.

\[SINCE Orbeon Forms 2017.2]

You can decide whether the published form is _available_ to end users with the "Make published form available" option. You can later change the availability of the published form either in Form Builder by re-publishing it, or from the [Forms Admin page](https://doc.orbeon.com/form-runner/pages/forms-admin-page#controlling-form-definitions-availability). By default, the form is made available.

### Versioning <a href="#versioning" id="versioning"></a>

#### New version or overwrite <a href="#new-version-or-overwrite" id="new-version-or-overwrite"></a>

When versioning is enabled, you have a choice, when publishing, of whether to _create a new form version_ or to _overwrite an existing one_.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwwLIDKHC9nZMEPp%252Fpublish-version-next.png%3Fgeneration=1528743328368439%26alt=media\&width=300\&dpr=4\&quality=100\&sign=5c74321714cb2298003488bdc3cc178f5a4ac49f3dc8e02444b02697afaa5d4f)

When creating a new form version, publishing the form definition creates a new version of the form definition in the database. Data which already exists in the database will still be viewed and edited with the previous versions of this form definition which are associated with that data. The new form definition version will be used to create, edit and view new data.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwwNtxUT8onzoITk%252Fpublish-version-overwrite.png%3Fgeneration=1528743328386773%26alt=media\&width=300\&dpr=4\&quality=100\&sign=c709509c2c987f0ea431d9e1d1bbe0fabef5b2eb0d6491d1cf2bccf090f2d491)

When overwriting an existing form definition, if the changes you have made to the form definition are small and non-structural, such as changing control labels only, the form definition will be compatible with existing form data. But if you have made structural changes, such as adding, removing, renaming, or moving controls, the form definition might be incompatible with existing data. We recommend being careful when overwriting an existing form definition version.

See also [versioning](https://doc.orbeon.com/form-runner/features/versioning) for details about the implications of these options.

\[SINCE Orbeon Forms 2016.1]

You can overwrite not only the latest published version, but any previous version.

#### Versioning comments <a href="#versioning-comments" id="versioning-comments"></a>

\[SINCE Orbeon Forms 2016.2]

You can add or update a textual comment associated with the given published version. Adding a comment is not required, but setting a comment can help the form author understand what changes a given form version includes.

When creating a new version, the field is initially empty:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwwPzY0ZOvAs9PoU%252Fpublish-comment-1.png%3Fgeneration=1528743335754976%26alt=media\&width=300\&dpr=4\&quality=100\&sign=b971867af7e7ea2b0b5e1f93efc4fda3bfed05f4dd2388ea8ef32f5ae269efd8)

You can set an explanatory comment:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwwRV9Eb2XlxCBGK%252Fpublish-comment-2.png%3Fgeneration=1528743336770889%26alt=media\&width=300\&dpr=4\&quality=100\&sign=ae2988eb5dbd1b5f1bdfb7c3ef5ee38752eb1ca99c8b53e1f65757445d5c6bef)

When overwriting an existing form version, the existing comment, if any, is read back from the published form definition and you can update it before publishing:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwwTOFNTxdrc\_XXK%252Fpublish-comment-5.png%3Fgeneration=1528743337293276%26alt=media\&width=300\&dpr=4\&quality=100\&sign=ca0d9bc75fc52ed12f05f61885c898dd557d0ac494ff4e976fa5bab2a18f5379)
