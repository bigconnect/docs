# Revision history

Each time Form Runner saves data to the database, it saves a new _revision_ of the data. Each revision of the data has an associated timestamp.

Until Orbeon Forms 2023.1, this revision was only accessible through the database, for [auditing purposes](https://doc.orbeon.com/form-runner/persistence/auditing). Starting with Orbeon Forms 2023.1, Form Runner provides a user interface to access the revision history of a given form data.

Note that this feature is distinct from [form definition versioning](https://doc.orbeon.com/form-runner/features/versioning), which regards the versioning of published form definitions.

### Enabling the feature <a href="#enabling-the-feature" id="enabling-the-feature"></a>

The feature is disabled by default. You can enable it globally by setting the following property to `true`:

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.navbar.revision-history.enable.*.*"
    value="true"/>
```

This adds a "Revision History" icon in the [navigation bar](https://doc.orbeon.com/form-runner/features/navbar).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-2eb73ae5e16a1b3c172f74a4724d518df7924f63%252Ffr-revision-history-icon.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=bdf33874d7a865df1da9892f69062f673173b09db764e6a571ab5af948385d56)

Revision History icon

Activating the icon opens the Revision History pane:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-f1021b221d24430ee7fae127988b4438aca17558%252Ffr-revision-history-pane.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=e20ce13830a4d6697a601afa8518493b5353eda6164ebc21d3f9448680096de8)

Revision History pane

### The Revision History pane <a href="#the-revision-history-pane" id="the-revision-history-pane"></a>

The Revision History pane shows the list of revisions for the current form data, with the following information:

* date and time of the change
* username of the user that made the change, or "Anonymous user"
* a description of the changes, including
  * value entered
  * value cleared
  * value changed
  * iteration added
  * iteration removed
  * other change
  * no change

For a given revision, you can click on the "View this revision" link to view the form data as it was at the time of that revision.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-1bf332afe2ea546c47f67cec00fbd1f980268f89%252Ffr-revision-history-view-revision.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=7840795e0e7180279f9cc1a0cbfa0e570f42a184de0709fcda65f00e0a4632a4)

Viewing a specific revision
