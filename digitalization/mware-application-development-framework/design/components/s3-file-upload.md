# S3 File Upload

The `S3 File Upload` component is a special-built component that allows your users to directly upload from Budibase into an S3 Bucket.

Visually the `S3 File Upload` component looks the same as the Attachment component.

![](https://files.readme.io/78f281b-attachment.png)

### Adding the S3 File Upload

The `S3 File Upload` component requires a [Form](https://docs.budibase.com/docs/forms) element to be a parent.

<figure><img src="https://files.readme.io/2d5c5e5-Screenshot_2023-02-06_at_14.05.48.png" alt="Adding an S3 File Upload component"><figcaption><p>Adding an S3 File Upload component</p></figcaption></figure>

An S3 datasource must also have been added to the **Data** section in Budibase. After the S3 connection is established, you can select the Datasource, and enter a bucket and file name in the Settings Panel.

You will also need to provide a custom field name.

<figure><img src="https://files.readme.io/3330f48f0873c2e60ccc74d1a86415b2b7062c0cfc9e8bba4dc280673fe441ad-Screenshot_2024-09-27_at_14.16.25.png" alt=""><figcaption></figcaption></figure>

### Upload File to S3 action

Whilst the upload component has been added, you still need to use an upload Action so that the user can submit their selected files to the S3 bucket.

The easiest way to do this is to add a Button. Click `Define actions`, add the **Upload File to S3** action, and select your `S3 File Upload` component from the dropdown. Be sure to click `Save`.

<figure><img src="https://files.readme.io/1c07e65-Screenshot_2023-02-06_at_14.51.02.png" alt=""><figcaption></figcaption></figure>
