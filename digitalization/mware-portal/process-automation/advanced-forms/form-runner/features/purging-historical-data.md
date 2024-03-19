# Purging historical data

The "Purge" dialog is similar to the "Export" dialog, except that it allows you to delete form data instead of exporting it. A typical scenario will involve exporting the data first, e.g. for archival purpose, then deleting/purging it.

Like in the "Export" dialog, you can either purge the forms that were explicitly selected:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-9a50aeb0b440e17e1722aeb77b7cd84b39da0789%252Fpurge-selected-forms.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=a8e8d629bcdd2bc871ae0e8da32c8d2708e3a4551f3bf372d6a6bb29098bce89)

Purge selected forms

Or you can purge forms by specifying a list of application, form, and version filters:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-8095cff5b2ba81f4077b163026fcb7f93ab0df7e%252Fpurge-following-forms.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=bcd19cb29c845c7f9f4b927ca8c1f7df23206c3bc6c67deef94000bd8691d133)

Purge following forms

### Limitations <a href="#limitations" id="limitations"></a>

At the moment, it is only possible to purge form data, not form definitions.

At the moment, once the "Purge" button has been clicked, the dialog will be closed and the purge will be performed in the background. This means that you won't be able to see the progress of the purge, and you won't be able to cancel it. This also means you shouldn't leave the current page until the zip file has been downloaded. This will be improved in a future version.
