# Managing Form Entries

Once users begin submitting form entries, you’ll want to do these things with them:

* View form entries
* Export form entries
* Delete form entries

Start by learning how to access and view the entries.

### Viewing Form Entries <a href="#viewing-form-entries" id="viewing-form-entries"></a>

When users fill out forms, they’re generating data. You’ll want to see that data at some point.

1. From the Menu, navigate back to the _Content_ → _Forms_ section of The Lunar Resort site.
2.  Click the _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030162191/icon-actions.png)) button for the form and select _View Entries_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162211/forms-view-entries.png" alt="Figure 1: You can view the entries right in the Forms application." data-size="original">

    Figure 1: You can view the entries right in the Forms application.
3.  What if you have a lot of form fields and you can’t see all the data for each entry in the search container? Just click the _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030162191/icon-actions.png)) button for the entry and select _View_. You’re shown all the specifics for that form entry.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193492/forms-view-entry.png" alt="Figure 2: You can view a single entry right in the Forms application." data-size="original">

    Figure 2: You can view a single entry right in the Forms application.

Viewing entries is great, but this is serious data we’re talking about. You might need to get all the entries into a spreadsheet so you can work with them.

### Exporting Form Entries <a href="#exporting-form-entries" id="exporting-form-entries"></a>

So you need to put your form entries in a spreadsheet to do spreadsheet things with them? No problem.

1. Navigate to the Forms application in The Lunar Resort site’s Content & Data section again.
2. Click the _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030162191/icon-actions.png)) button and select _Export_.
3. Choose a File Extension. You can export entries in CSV, JSON, XLS, or XML formats by default. For this example, pick CSV.
4. Click _Okay_, and open the file or save it locally. Open it with your favorite spreadsheet program and verify your form entries.

![Figure 3: You can export entries as CSV, JSON, XLS, or XML.](https://liferay-support.zendesk.com/hc/article\_attachments/360030193512/forms-export-csv.png)

**Note:** The Forms application itself has an _Import/Export_ window accessible from the application’s Configuration menu (![Configuration](https://liferay-support.zendesk.com/hc/article\_attachments/360030162231/icon-options.png)). This is how you import and export the application configuration and its data (forms and form entries). The file format for this type of import and export is a LAR file.

There’s a system level setting to determine whether administrators can export entries in CSV format:

1. Go to Control Panel → Configuration → System Settings and click the _Forms_ category in the CONTENT AND DATA section.
2. Click the _Forms_ entry under SITE SCOPE.
3. The CSV Export property has three options:
   * _Enabled_ to enable CSV Export without a warning
   * _Enabled (Show Warning)_ to enable CSV Export with the following warning to administrators: This CSV file contains user supplied inputs. Opening a CSV file in a spreadsheet program may be dangerous.
   * _Disabled_ to turn off CSV Export.

Once you export a batch of form entries, it can make sense to delete them from the database.

### Deleting Form Entries <a href="#deleting-form-entries" id="deleting-form-entries"></a>

What if you export a form’s entries and now you want to remove them from the portal database? It’s easy to delete all of a form’s entries at once.

1. Navigate back to the Forms application In The Lunar Resort Content section.
2. Click the _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030162191/icon-actions.png)) button next to the Guest Survey form and select _View Entries_ again.
3.  Select all entries by checking the box next to _Filter and Order_. An X appears in the top right corner of the Form Entries screen. Click it.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193472/forms-delete-entries.png" alt="Figure 4: Delete all form entries in one fell swoop." data-size="original">

    Figure 4: Delete all form entries in one fell swoop.

If you just wanted to delete a single entry, select that entry by checking its box; then delete it.

If you’re worried about deleting everything irrecoverably by accident, don’t worry. You must confirm the deletion in a dialog box that pops up after clicking the trash can.

Now you can create basic forms and manage the entries. Keep reading in this section to learn about the many additional form building features available to you.
