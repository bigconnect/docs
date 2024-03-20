# Forms Admin page

The Forms Admin page allows you to perform admin tasks on published forms.

You access the Forms Admin page by adding `fr/admin` to the path on which you deployed Advanced Forms. If you deployed Advanced Forms on `http://www.example.com/forms/`, then you can access the Forms Admin page at `http://www.example.com/forms/fr/admin`.

The Forms Admin page is very similar to the Published Forms page, but features additional administration functions and shows library forms.

This page is also accessible directly from the Landing page.

![The Forms Admin page](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-447fdfc55e50c0a871e3e8fa5f8ca1ee120bbec0%252Fform-runner-admin.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=ed8b95f964d32de34ee7795ef260cb33daed22e6c95bff161b9e518e7bb67c95)

### Permissions <a href="#permissions" id="permissions"></a>

If the user has permissions set in `form-builder-permissions.xml`, the Forms Admin page shows and is linked from the Landing page.

The list of forms listed depends on the roles set in `form-builder-permissions.xml`. For example, with:

```
<role name="*" app="*" form="*"/>
```

the user can perform any admin operation on any form. But with:

```
<role name="forms-user" app="acme" form="*"/>
```

the user can perform admin operations on `acme` forms only.

### Admin operations on published forms <a href="#admin-operations-on-published-forms" id="admin-operations-on-published-forms"></a>

In addition to information shown in the Published Forms page, the Forms Admin page shows the following information:

* whether the form is available or unavailable
* whether the form is a library form

Unavailable forms remain on the server, but any user access to an unavailable form behaves as if the form definition had never been published. Data associated with the form definition is not touched, but cannot be read or updated via the form definition when it is unavailable.

Administrators can select forms in two ways:

* by using the checkboxes next to the form
* by using the _Select_ menu to automatically select all forms matching a certain condition

![Select Menu](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-e7b57ff3d3087a786f3f32f505ec00fa311022df%252Fhome-select-menu.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=68564ba3afbd90bc080c2f8381cad9ba283e32662c5252b8e715f52497330ed7)

The operations listed in the Operation menu are then available depending on the current selection:

![Operation Menu](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-34d3ad276172d0d163980be9224cf71b6d691e1b%252Fhome-operation-menu.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=5709cc130697da94209f5c77af87d8e8184bdbff27b3986d0b52db82c6a65110)

### Controlling form definitions availability <a href="#controlling-form-definitions-availability" id="controlling-form-definitions-availability"></a>

* **Make available local forms:** make an unavailable form definition available again.
* **Make unavailable local forms:** make a published form definition unavailable.

![Unavailable](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2xvQHKhQIaa64yx%252Fhome-unavailable.png%3Fgeneration=1528743371039156%26alt=media\&width=768\&dpr=4\&quality=100\&sign=086c28f7b862e195a1cecb3ca84c365644faa541e6bc77afbf648ed8763b928b)

Initially, when publishing a form definition with Form Builder, the form definition is made available, unless you choose the option "Make published form available" to "No".

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwwLIDKHC9nZMEPp%252Fpublish-version-next.png%3Fgeneration=1528743328368439%26alt=media\&width=768\&dpr=4\&quality=100\&sign=5c74321714cb2298003488bdc3cc178f5a4ac49f3dc8e02444b02697afaa5d4f)

Making a form definition available or unavailable also takes into account the _version_ of the form definition(s) selected.

### Deleting published form definitions <a href="#deleting-published-form-definitions" id="deleting-published-form-definitions"></a>

* **Delete local forms:** delete a locally-published form definition.
* **Delete remote forms:** delete a remotely-published form definition.

Unlike the "Make unavailable" operations, this operation removes the selected published form definitions. Once this is done, the form definitions are no longer visible in the Admin page and they can no longer be accessed.

This feature is useful in cases a form definition was published by mistake, for testing, or if at a later time the form definition is deemed unnecessary.

Important notes:

1. This does not cause any data entered using this form definition to be removed. The data will remain in the database. However, that data is no longer accessible by a form definition. If a form definition with the same application name, form name, and form version is later published, then it will have access to that data again.
2. This action cannot be undone. However, the [auditing trail](https://doc.orbeon.com/form-runner/persistence/auditing) in the database might allow recovery of a deleted form definition at the database level.

### Upgrading form definitions <a href="#upgrading-form-definitions" id="upgrading-form-definitions"></a>

#### Introduction <a href="#introduction-1" id="introduction-1"></a>

* **Upgrade local form definitions:** upgrade the selected local published form definitions, including using published section templates.
* **Upgrade remote form definitions:** upgrade the selected remote published form definitions, including using published section templates.

This operation, for each selected form definition:

1. Reads the selected version of the published form definition, whether it is marked available or unavailable.
2. Loads published section templates used by the form definition if needed: loads the _specific versions_ of the published section templates that are associated with that form definition.
3. Upgrades the form definition markup to the latest format, as if loading it in Form Builder and saving it again.
4. Includes the latest section templates loaded above into the form definition.
5. Publishes back the form definition, either as a new version or as the same version:
   * the upgraded form definition _overrides_ the previously-published form definition (which means that it has the same application name, form name, and form definition version).

In general, it is not required to run this function even after upgrading to a new version of Advanced Forms, since form definitions are backward compatible.

#### Upgrading section templates <a href="#upgrading-section-templates" id="upgrading-section-templates"></a>

Here is how you can use this operation to update your form definitions' section template to the latest published section templates:

1. Publish your section templates from Form Builder.
2. In the Forms Admin page, select which forms you want to update.
3. Run the "Upgrade local form definitions" action.

We advise that you only republish form definitions which use section templates if you know that the format of the data covered by the section templates remains compatible, or that Simple Data Migration is enabled and expected to cover changes such as adding fields.

### Remote server operations <a href="#remote-server-operations" id="remote-server-operations"></a>

#### Introduction <a href="#introduction-2" id="introduction-2"></a>

This feature allows you to configure access to a remote server and to make available, make unavailable, and transfer (push/pull) forms between the local and remote server.

#### Configuration on the local server <a href="#configuration-on-the-local-server" id="configuration-on-the-local-server"></a>

In order to configure a remote server, you need to setup a property.

Use the `oxf.fr.home.remote-servers` property, which allows configuring multiple remote servers:

```
<property as="xs:string" name="oxf.fr.home.remote-servers">
  [
    {
      "label": "Staging",
      "url":   "http://staging.example.org:8080/forms"
    },
    {
      "label": "Production",
      "url":   "http://prod.example.org:9090/forms"
    }
  ]
</property>
```

![Multiple Remote Servers](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2y10oe0f56fB40A%252Fremote-server-credentials.png%3Fgeneration=1528743371014431%26alt=media\&width=768\&dpr=4\&quality=100\&sign=d41dee8c9405b9db7d11fca538afafd4bf9ba55cc99cc5a9cf42e926bfe5253f)

The format is a JSON array containing any number of JSON objects. Each object must have a non-empty `label` property indicating the label to display to the user when selecting a remote server, and a non-empty `url` property indicating the location of the remote server.

#### Configuration on the remote server <a href="#configuration-on-the-remote-server" id="configuration-on-the-remote-server"></a>

You also need to authorize the remote server to accept incoming connections for services.&#x20;

You deploy this WAR file alongside `forms.war` on the remote server, and you add this property to the remote server's `properties-local.xml`:

```
<property
  as="xs:anyURI"
  processor-name="oxf:page-flow"
  name="authorizer"
  value="/forms-auth"/>
```

This tells the remote server to use the `forms-auth` webapp to authenticate requests for services or pages which are not public.

By default, `forms-auth` requires that all external requests to Form Runner services are authenticated with `BASIC` authentication and have the `forms-service` role. It blocks any other request.

If you are using Tomcat, you can then configure a user with role `forms-service`. For example, in `tomcat-users.xml`:

Copy

```
<role rolename="forms-service"/>
<user username="forms-admin" password="changeme" roles="forms-service"/>
```

Then, on the local server, you would use username `forms-admin` and password `changeme` when prompted.

With this configuration, the local Advanced Forms connects to services on the remote Advanced Forms, which calls up `forms-auth` to authenticate the connection. `forms-auth` requires that the username/password provided authenticate as a valid Tomcat user with the `forms-service` role. If that's successful, then the service proceeds, otherwise it fails.

#### Remote operations <a href="#remote-operations" id="remote-operations"></a>

When the remote server is configured as above, the first time you go to the Forms Admin page you are prompted for credentials:

![Credentials](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yCtL5T0tdKITx1%252Fhome-credentials.png%3Fgeneration=1528743371001045%26alt=media\&width=768\&dpr=4\&quality=100\&sign=b25a87bcc03c9521544d63709b4f425b4ba1b1f5ff22d4db7d9df486ada77618)

Once the credentials are correct, the Forms Admin page retrieves the remote server's list of deployed forms and metadata, which appears in a new _Remote_ column group. You then have access to new operations:

* **Make available remote form:** make an unavailable form available again on the remote server.
* **Make unavailable remote form:** make a previously published form unavailable on the remote server.
* **Push to remote:** copy a form definition and its attachments from the local to the remote server.
* **Pull from remote:** copy a form definition and its attachments from the remote to the local server.
* **Upgrade remote form definitions:** upgrade the remote form definition

![Push to Remote](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yFpcGIguGLyE71%252Fhome-push.png%3Fgeneration=1528743371060970%26alt=media\&width=768\&dpr=4\&quality=100\&sign=2e93e6c0512dce5d128862a031f803deadb95086b2f5bcd79b99254591ec766d)

You are always prompted to confirm the operation to perform:

![Confirmation](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yHaHxs40y-VXQE%252Fhome-confirmation.png%3Fgeneration=1528743371022437%26alt=media\&width=768\&dpr=4\&quality=100\&sign=7aa8bab5f576e4add87469d0d8f157c26c010c801855e9e003254306481d394e)

In addition, if the latest modification time of the form definitions differ, a _Newer_ label appears:

![Newer](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yJRbRbAVRZt\_Wj%252Fhome-newer.png%3Fgeneration=1528743370963141%26alt=media\&width=768\&dpr=4\&quality=100\&sign=45c838f8890f423f6a1322ac521b4020fc188f914f0fd69559bb3d12fedeb013)

### Re-encryption <a href="#re-encryption" id="re-encryption"></a>

#### Limitations to re-encryption <a href="#limitations-to-re-encryption" id="limitations-to-re-encryption"></a>

Currently, re-encryption is only supported in the built-in implementation of the persistence API for relational databases. The built-in implementation of the persistence API for eXist doesn't support re-encryption, and custom implementations can't yet support re-encryption.

#### Creating a database backup before re-encryption <a href="#creating-a-database-backup-before-re-encryption" id="creating-a-database-backup-before-re-encryption"></a>

Unlike typical update and delete operations that are performed at the database level by adding a new row, thus keeping the old data, re-encryption is done _in-place_. All data is being re-encrypted, including historical data that is part of the "journal". This means that you could loose data if something were to go wrong during re-encryption. As such, we strongly recommend you create a backup of your data before re-encryption.

#### Starting re-encryption <a href="#starting-re-encryption" id="starting-re-encryption"></a>

To re-encrypt all the data for certain forms, from the Forms Admin page:

1. Mark the checkboxes corresponding to the relevant forms.
2. Click on the "Operations" button, and in the menu click on "Re-encrypt form definitions". If that entry is greyed out, make sure that you haven't selected a form for which data is stored with a persistence layer that doesn't support re-encryption.
3. After you confirm you desire to go ahead with the re-encryption, you will see, in the "Status" column:
   * "Re-encryption started," right after you triggered the re-encryption.
   * "Re-encrypting 392/2401," while the re-encryption is in progress, here assuming it has already re-encrypted 391 documents out of 2401 it needs to re-encrypt.
   * "Re-encryption finished," when the re-encryption is done for that form.

### Reloading resources <a href="#reloading-resources" id="reloading-resources"></a>

Because Advanced Forms caches form resources, if you override resources with the `oxf.fr.resource` properties, those resources are not updated until the server restarts.

Using the "Reload resources" action will invalidate the cached resources of all the selected forms immediately. The next time the form is loaded, it will use up to date resources including those overridden in properties.

### Reindexing <a href="#reindexing" id="reindexing"></a>

#### UI <a href="#ui" id="ui"></a>

In admin view, the Forms Admin page shows a reindex button:

![Reindex button](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yLZmEnyA11-lGY%252Fhome-reindex-button.png%3Fgeneration=1528743370915987%26alt=media\&width=768\&dpr=4\&quality=100\&sign=dd4d07a9cb3e86b9aea0bb5176f560403276d1812e615985e45020700eb3f2d3)

After you click it, and confirm you want to go ahead, if this is indeed what you want to do (see above paragraph for more information about what happens during reindexing), the Forms Admin page shows the reindexing status:

![Reindex status](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yNZx7KFurXFjqx%252Fhome-reindex-status.png%3Fgeneration=1528743371048935%26alt=media\&width=768\&dpr=4\&quality=100\&sign=a0bdcba67f193a2f2a221ea2be868a2b7277b4f08f1081be2ecbe07c4a69fc72)

If, for some reason, you want to stop reindexing, you can do so by clicking on the _Stop reindexing_ button. When doing so, the index is left in an incoherent state, and you should restart indexing at a latter time. While reindexing stops, you'll see the following message:

![Reindex stopping](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yPjqc4E1oEvXYK%252Fhome-reindex-stopping.png%3Fgeneration=1528743370972678%26alt=media\&width=768\&dpr=4\&quality=100\&sign=dc0dc3a2fd09ca7e3d432a172d6a47a872ea845ce5171f7a5fbc3405927dc065)

Finally, once reindexing is done, you'll see:

![Reindex done](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2yRbPZDYero2\_h3%252Fhome-reindex-done.png%3Fgeneration=1528743371045531%26alt=media\&width=768\&dpr=4\&quality=100\&sign=8b4bf53b7911ab83345ac6c0bae0dfa0baf90ff4fb618222db90637f85f41a59)

#### Logging <a href="#logging" id="logging"></a>

While reindexing happens, the indexer writes to the log:

* When indexing starts and ends.
* Which providers it will reindex.
* For each provider how many document it will reindex.
* A "progress message" for each document being reindexed.

The last message is logged at the `debug` level, to avoid your log growing unnecessarily in case you have lots of documents, while all the other messages are logged at the `info` level. For instance, you'll see something along those lines in your `forms.log`:

```
INFO  - Reindex status - Starting, will index [oracle_dev, oracle_staging]
INFO  - Reindex status - Indexing oracle_dev 1/2
INFO  - Reindex status - Indexing oracle_dev 1/2, 8475 documents
DEBUG - Reindex status - Indexing oracle_dev 1/2, document 1/8475
DEBUG - Reindex status - Indexing oracle_dev 1/2, document 2/8475
…
INFO  - Reindex status - Stopped
```

If you'd like to enable logging at the `debug` level, add the following to your `log4j.xml`:

```
<category name="org.forms.relational">
    <priority value="debug"/>
</category>
```

### Exporting form definitions and form data <a href="#exporting-form-definitions-and-form-data" id="exporting-form-definitions-and-form-data"></a>

See Exporting form definitions and form data.

### Purging historical data <a href="#purging-historical-data" id="purging-historical-data"></a>

See Purging historical data.

### Configuration properties <a href="#configuration-properties" id="configuration-properties"></a>

The same configuration properties as for the Published Forms page apply.

