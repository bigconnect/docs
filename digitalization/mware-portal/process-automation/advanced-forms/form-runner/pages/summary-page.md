# Summary page

The Form Runner Summary page shows, for a given published form, the list of accessible data in a table with paging. It allows:

* Listing and searching data
* Creating new data
* Editing or visualizing existing data
* Deleting data
* Duplicating data
* Navigating to the Home page and Import page
* Opening a PDF or TIFF version of the form

<figure><img src="../../../../../../.gitbook/assets/image (7).jpg" alt=""><figcaption><p>Summary Page for the Bookshelf form</p></figcaption></figure>

### Columns <a href="#columns" id="columns"></a>

* **Created:**
  * This is the data's creation date.
  * You can optionally remove this column via configuration.
* **Modified:**
  * This is the data's last modification date.
  * You can optionally remove this column via configuration.
* **Custom columns:**
  * You specify those when editing the form definition, using the Control Settings dialog.

#### Sorting <a href="#sorting" id="sorting"></a>

It is possible to sort the data by clicking on the column headers. The sort order is indicated by an arrow next to the column name. Clicking multiple times on the same header will toggle between ascending and descending order.

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-493d7d3ea0e1ee45a6e681871b8c224973f5adb2%252Fsummary-sorting.png%3Falt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=c637c3817dabdcba89137bf6065bdcbba2717df19767e144560fed6c24fdcfd8" alt=""><figcaption></figcaption></figure>

Limitations:

* Currently, all values except the "Created" and "Modified" columns are sorted as strings. This can lead to unexpected results, in particular for numbers and HTML values. Numbers will be sorted alphabetically (e.g. "1", "10", "11", "2", "37", "4", etc.) and HTML values will be sorted by their HTML representation (i.e. including HTML markup).
* Dropdown controls will be sorted by their value, not their label.

**Enabling support**

Implementations of the persistence API may support the sorting feature, but they are not required to. For this reason, if your implementation does support the sorting feature, you must set the following property to `true` for your provider:

```
<property 
    as="xs:boolean" 
    name="oxf.fr.persistence.[provider].sort"
    value="true"/>
```

### Search <a href="#search" id="search"></a>

By default, the Summary page shows a single search box which does a full-text search in the form data.

You can open the search options using the "Show Search Options" button. The search options area contains individual search fields which allow performing a structured search, or search by field.

#### Search by created, modified, created by, modified by, and workflow stage <a href="#search-by-created-modified-created-by-modified-by-and-workflow-stage" id="search-by-created-modified-created-by-modified-by-and-workflow-stage"></a>

When the corresponding `oxf.fr.summary.show-*` properties are enabled, the search options area contains individual search fields which allow performing a search by created, modified, created by, modified by, and workflow stage.

The created by, modified by, and workflow stage search fields contain distinct values from accessible data.

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-0f0e15313bc589fe20f963ab722577bb34f6482c%252Fsummary-search-created-last-modified.png%3Falt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=9f4a48038d716cd8f83d5fcea01417db1e9e0cb5067a6a8572e84dcbf422b922" alt=""><figcaption></figcaption></figure>

#### Dynamic dropdowns <a href="#dynamic-dropdowns" id="dynamic-dropdowns"></a>

For dynamic dropdowns, the original service used to populate the dropdown is not called again, but distinct values from accessible data are listed. This is also done in the bulk edit area if dynamic dropdowns are present.

<figure><img src="../../../../../../.gitbook/assets/image (8).jpg" alt=""><figcaption><p>Search Options</p></figcaption></figure>

You specify which fields appear in the search options area when editing the form definition, using the Control Settings dialog.

### Versioning <a href="#versioning" id="versioning"></a>

When more than one form version is available, the user has the choice of the version to access. Different versions can behave like very different forms. Also see the properties to configuring the behavior of the summary page with regards to versioning.

![Summary Page for version 1 of the form](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-d2dd762be40c29a11c4b8cc87409c7d01d2f3c6a%252Fsummary-version-1.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=d2f92a5348ed326f4378d45b9dbd3056c9519cc977ffbe7ec9b4f92e6bdf277f)

![Summary Page for version 2 of the form](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-47ab592e4b6fcf6d85c8627a978fb40d4aa6f083%252Fsummary-version-2.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=ed9ec649d5ccf8c8e4db7af45656f22bbdfc3639fe8051f4ef0867146b4e120f)

### Bulk edit <a href="#bulk-edit" id="bulk-edit"></a>

Just like for search options, you can open the bulk edit area using the "Show Bulk Edit" button. The bulk edit area contains individual fields which allow performing a bulk edit of multiple forms at once.

<figure><img src="../../../../../../.gitbook/assets/image (9).jpg" alt=""><figcaption></figcaption></figure>

Bulk Edit

You specify which fields appear in the bulk edit area when editing the form definition, using the Control Settings dialog.

