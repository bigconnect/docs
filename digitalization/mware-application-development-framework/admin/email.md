# Email

For the system to send emails, you must add your SMTP settings. After you have set this up, you can invite users and send emails using the email Action.

<figure><img src="https://files.readme.io/b283bce-CleanShot_2022-04-12_at_14.18.152x.png" alt="" width="563"><figcaption></figcaption></figure>

#### Email setup

| Property                                           | Description                                                                                                                                                                                                                                                                                                                                                                                                           | Example answer                     |
| -------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| Host                                               | An SMTP email server will have an address (or addresses) that can be set and is generally formatted as smtp.serveraddress.com.                                                                                                                                                                                                                                                                                        | email-smtp.eu-east-1.amazonaws.com |
| Security type                                      | Both SSL and TLS facilitate email sending                                                                                                                                                                                                                                                                                                                                                                             | None/STARTTLS                      |
| Port                                               | <p>An “SMTP port” refers to the specific part of the Internet address that’s used to transfer email.<br><br>If you run your own SMTP server you can find the configured SMTP port number and address from the SMTP server configuration.<br><br>Ports 25, 465, 587, or 2525 for SMTP have all been considered standard SMTP ports at some point, but only 587 or 2525 really should be considered for modern use.</p> | 587                                |
| From email address                                 | The email address your onboarding emails are sent from.                                                                                                                                                                                                                                                                                                                                                               | noreply@adress.com                 |
| Require sign-in                                    | SMTP auth - some SMTP server hosts require auth to send email.                                                                                                                                                                                                                                                                                                                                                        | True \[recommended]                |
| Username (visible when require sign-in is checked) | Username for SMTP server                                                                                                                                                                                                                                                                                                                                                                                              | AKIAX5ZKDVGGJ2SASAHT2M7Z           |
| Password (visible when require sign-in is checked) | Password for SMTP server                                                                                                                                                                                                                                                                                                                                                                                              | Password!23IhopeNot                |

&#x20;

### Templates

Mware Application Development Framework comes with three different email templates: Password Recovery, Invitation, and Welcome. In addition to that, you can also create custom templates.

To edit a template, simply select it in the table. To insert dynamic content you can use the Bindings on the right side of the page. This makes it easier to include things such as the user's email, your organization's logo, or the name of your organization. When you're done hit the preview button to make sure it looks OK. Then hit save.

<figure><img src="https://files.readme.io/11c9942-templates.png" alt=""><figcaption></figcaption></figure>

The email templates work on all devices, email platforms, and themes (light and dark modes). Below is an image of the invitation email template.

<figure><img src="https://files.readme.io/824b249-CleanShot_2022-04-12_at_15.22.54.png" alt=""><figcaption></figcaption></figure>
