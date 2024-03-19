# Summary page

The Form Builder summary page is your starting point when you work with Form Builder. It allows:

* Listing and searching form definitions
* Creating new form definitions
* Editing existing form definitions
* Deleting form definitions

_NOTE: Form definitions are usually stored in a database on the server. You do not usually keep them on your own computer._

### Accessing the summary page <a href="#accessing-the-summary-page" id="accessing-the-summary-page"></a>

You access the summary page from your web browser through a URL (or web address) of the form:

Copy

```
http://[SERVER NAME]/orbeon/fr/orbeon/builder/summary
```

_NOTE: The exact URL might be provided by your IT organization._

Once you reach the summary page, you will either see an empty list, or a list containing existing form definitions, as shown in this screenshot.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LV0uZhouXTII1cq6jjS%252F-LEkC028ItoCLFeR89a-%252Fsummary.png%3Fgeneration=1546220395830772%26alt=media\&width=768\&dpr=4\&quality=100\&sign=7b131dfd17e14ddd58a0ec3325df2e9824451d36041ae47dca7c5c42b4122987)

The Summary page

For each form definition, the list shows:

* Creation date
* Last modification date
* Application name
* Form name
* Form title
* Form description if any

If the list doesn't fit entirely on one page, you can navigate to further pages of results using the navigation arrows at the bottom of the results list.

If the list is too wide, a horizontal scrollbar appears at the bottom of the list and allows you to navigate horizontally.

By default, the summary page lists all the form definitions which you have access to.

_NOTE: Depending on your permissions, the Application Name field and/or Form Name field might be input fields or dropdown menus that restrict your choices._

#### Searching and navigating form definitions <a href="#searching-and-navigating-form-definitions" id="searching-and-navigating-form-definitions"></a>

The top of the summary page features search fields. There are two types of searches:

* **Free text search.** This searches any text within the form definition.
* **Structured search.** This searches on Application Name, Form Name, Form Title, or Form Description.

To search: enter a search term and press the "Search" button or the "enter" key.

Tip: to clear the search and list all the form definitions again, clear all search fields and press the "Search" button or the "enter" key.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC02EWQrIT0HKW0hA%252Fsummary-search.png%3Fgeneration=1528743356143659%26alt=media\&width=768\&dpr=4\&quality=100\&sign=e837ef506166aee01950e39eb4d81ebadbf39442400a15bb43e9e4dabda65c00)

### Creating a new form definition <a href="#creating-a-new-form-definition" id="creating-a-new-form-definition"></a>

To create a new form definition, press the "New" button at the bottom of the page. This opens the Form Builder editor in a separate browser window or tab.

### Deleting a form definition <a href="#deleting-a-form-definition" id="deleting-a-form-definition"></a>

Using the checkboxes that appear on each row, select the form definitions you wish to delete, then press the "Delete" button.

### Permissions <a href="#permissions" id="permissions"></a>

The Summary page follows the [access control rules for deployed forms](https://github.com/orbeon/orbeon-forms-doc/tree/f9c4c489b0f674f20b2269ec1d76c98482f25e54/form-builder/form-runner/access-control/deployed-forms.md). This means that a user accessing the Summary page will only see the data to which she has access to.
