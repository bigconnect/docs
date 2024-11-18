# Send Email

Before you can use the Send Email action step, please make sure you have configured [Email](https://docs.budibase.com/docs/email).

<figure><img src="https://files.readme.io/6d13e25-Screenshot_2023-09-07_at_13.55.08.png" alt="Please configure SMTP to use the Send Email action!"><figcaption><p>Please configure SMTP to use the Send Email action!</p></figcaption></figure>

### Settings

All of these settings support Bindings. For email address settings, you can provide multiple addresses by separating them with a comma, e.g. `anna@company.com,bill@company.com`.

| Setting        | Description                                                                                                                                                  |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Send To / From | The to / from email addresses                                                                                                                                |
| CC / BCC       | The carbon copy email addresses                                                                                                                              |
| Email subject  | The subject line                                                                                                                                             |
| HTML Contents  | <p>The body of the email, which support HTML tags.<br>This will also be the <code>{{ contents }}</code> binding in the custom automation email template.</p> |

#### Add calendar invite

Optionally you can attach a calendar invite to an email. Check the `Add calendar invite` box to see the additional invite settings:

<figure><img src="https://files.readme.io/69896ee-Screenshot_2023-11-22_at_13.45.36.png" alt="Sending a calendar invite"><figcaption><p>Sending a calendar invite</p></figcaption></figure>

In this screenshot above, a start and end time for the meeting was manually selected, however you can also use Binding values, by clicking the lightning bolt icon.

Both the _Location_ and _URL_ properties are optional, as your meeting may be in person, or online.

The email will then be received with an **ics** invite file attached:

\


<figure><img src="https://files.readme.io/93c0da7-Screenshot_2023-11-22_at_13.46.49.png" alt=""><figcaption></figcaption></figure>

#### Adding Attachments

When adding attachments to the Send Email step, you will need to provide 2 parameters - the URL and the filename. These values can come from bindings, or plain text, or a mixture of the two.

<figure><img src="https://files.readme.io/1f2a498-image.png" alt=""><figcaption></figcaption></figure>

These files will be attached to the email sent. Images attached this way may be displayed with the message body, or simply denoted in the list of attachments, depending on the recipient's mail client.

Alternatively, you can switch the "Use bindings" toggle to open the bindings draw. You should return an array of objects that match this pattern: `{url: string, filename: string}`. This is useful if you have an unspecified or undetermined number of attachments that you wish to attach.

JavaScript

```javascript
return $("trigger.row.photos").map(photo => {
  return {url: photo.url, filename: photo.name}
})
```

***

### Tutorial: Emailing new inventory details to a user

This tutorial will use the _Inventory_ table from the [Sample data](https://docs.budibase.com/docs/data-sources#sample-data). When a user assigns an inventory item, they will be sent an email with a summary of the item.

#### Step 1 - Automation

In the **Data** section, add a new _Text_ column to the _Inventory_ table called 'AssignedTo' and click `Save`.

Next navigate to the **Automation** section. Click `Add automation`, enter the _Name_ 'Confirmation email' and select App action as the trigger.

Use the `Add field` button to add the following fields:

<figure><img src="https://files.readme.io/147515b-Screenshot_2023-09-07_at_16.29.38.png" alt=""><figcaption></figcaption></figure>

Click the `+` button to add the **Send Email (SMTP)** action step.

Set _Send To_ as `{{ trigger.fields.email_address }}`, and provide a _Send From_ address and _Email Subject_.

For the _HTML Contents_ specify the email body text:

HTML Contents

```html
Hi {{ trigger.fields.first_name }},
<p>
The following item has been assigned to you:
<br>
<table>
  <tr>
    <th>Item Name</th><th>Notes</th><th>Status</th><th>Price</th>
  </tr>
  <tr>
    <td>{{ trigger.fields.item_name }}</td>
    <td>{{ trigger.fields.notes }}</td>
    <td>{{ trigger.fields.status }}</td>
    <td>{{ trigger.fields.price }}</td>
  </tr>
</table>
```

\


<figure><img src="https://files.readme.io/ebdf208-Screenshot_2023-09-07_at_17.09.40.png" alt=""><figcaption></figcaption></figure>

#### Step 2 - Design

In the **Design** section. Add a Data provider for the _Inventory_ table, with a filter: 'AssignedTo' _Is empty_

<figure><img src="https://files.readme.io/a6eb41c-Screenshot_2023-09-07_at_15.57.50.png" alt=""><figcaption></figcaption></figure>

Nest a Table component, and finally nest a Button.

<figure><img src="https://files.readme.io/5765864-Screenshot_2023-09-07_at_16.43.15.png" alt="Table with row buttons"><figcaption><p>Table with row buttons</p></figcaption></figure>

In the Settings Panel, click the button beside _On click_, and add a Trigger automation action, selecting the automation you created in the previous step.

Set the _email\_address_ and _first\_name_ to the mapping bindings of the Current user.

Fill out the remaining field inputs with the table row bindings, like so:

<figure><img src="https://files.readme.io/6438a53-Screenshot_2023-09-07_at_16.41.50.png" alt=""><figcaption></figcaption></figure>

Add a Save Row action, select the table component as the _Datasource_, and _Inventory_ as the target data table.

Click `Add Column`, and set _AssignedTo_ as `{{ Current User.email }}`.

<figure><img src="https://files.readme.io/8fbba9e-Screenshot_2023-09-07_at_16.49.13.png" alt=""><figcaption></figcaption></figure>

**Example of received email**

<figure><img src="https://files.readme.io/788321c-Screenshot_2023-09-07_at_17.06.00.png" alt=""><figcaption></figcaption></figure>

Ideally you want to specify styling in the custom automation email template.
