# Email settings

The dialog allows you to configure the content of emails sent by Advanced Forms. You can use this emailing capability to send a confirmation message to users who submitted the data, or to inform someone within your organization that new data has been submitted and requires processing. The Email Settings dialog allows you to define what the subject and the body of those emails are, and to whom they are sent to using an _email template_. As the name suggests, the email template can have placeholders that get filled when the email is sent, for instance with values entered by users in the form.

### Multiple email templates <a href="#multiple-email-templates" id="multiple-email-templates"></a>

You can have as many email templates per form as you'd like. This enables you to create different templates, for various languages, or have a template to inform users their data has been received and another template to email the person who needs to process the data. Additionally, if your form is used to implement a workflow, you can have different templates: for example, to notify a user that their request has been accepted or rejected. This opens up numerous possibilities for implementing workflows with Advanced Forms.

![Email Settings dialog](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-0f202001c11d07e8abaf2b639039de222ecd081e%252Femail-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=289347b8923b613aa1679d38aaa017b563caff962e76a8cd55bd65818c3d393b)

### Usage <a href="#usage" id="usage"></a>

You can open the "Email Settings" by clicking on the "Email Settings" button, under the "Advanced" tab, in the toolbox.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-6792fc8450886ca68dd55af58966e8f411c03d84%252Fadvanced-menu.png%3Falt=media\&width=300\&dpr=4\&quality=100\&sign=c039813767652bbadfcfb4099a826517472a86b8f50e0350b4ca02727157c4d0)

* Template name – Each email template must have a _name_. This name is not exposed to end users; it is only for your own use, to understand what each template is for, and to be able to refer to a specific template in the `email(template = "my-template")` action. Localization – If your form is available in multiple languages, a "Language" dropdown shows next to the template name, allowing you to select which language this template is for. When an email is sent, the template that corresponds to the user's language is automatically used.
* Enabling the template using a formula - An optional formula can be entered to allow the template to be conditionally sent or not.
* Dynamic email headers - Headers can be specified using form control values, formulas, or static texts. Custom headers are also supported (see below).
* Dynamic email subject and body – An email subject or body can be dynamic and include the value of form controls or formulas, as shown in the examples above.
* Attachments - Attachment controls can be included in the email.
* Excluded from All Control Values - It is possible to specify here which controls will be excluded from the "All Control Values" parameter value.&#x20;

### Custom headers <a href="#custom-headers" id="custom-headers"></a>

Custom headers can be added to the list of email headers by using the following property:

```
<property 
    as="xs:string" 
    name="oxf.fb.email.custom-headers.*.*"
    value="X-Priority X-SMTPAPI"/>
```

The value is a space-separated list of custom header names.
