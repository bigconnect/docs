# SAML-based authentication

## Turning on SAML-based SSO

Before beginning your SAML set-up, make sure you know the password for your Answers admin account. If anything becomes misconfigured during the set-up process, an “Admin backup login” option on the sign-in screen is available.

To get started, head over to the Settings section of the Admin Panel, then click on the **Authentication** tab. Click the **Configure** button in the SAML section of the Authentication page, and you’ll see this form:

<figure><img src="https://www.metabase.com/docs/latest/people-and-groups/images/saml-form.png" alt=""><figcaption></figcaption></figure>

The form itself is broken up into three parts:

1. Answers info that you’ll have to input into your identity provider (IdP).
2. IdP info that you’ll need to tell Answers about.
3. Signing SSO requests (optional).

## Setting up SAML with your IdP

First you’ll need to make sure things are configured correctly with your IdP. Each IdP handles SAML setup differently.

* Refer to your IdP’s reference docs on configuring SAML. You’ll be looking for something like this [OneLogin SAML guide](https://onelogin.service-now.com/support?id=kb\_article\&sys\_id=83f71bc3db1e9f0024c780c74b961970).
* Fill out your IdP’s SAML form using the information found on the Answers SAML form.
* For more information, see the next section on Generic SAML configuration.

## Generic SAML configuration

The top portion of the SAML form in Answers has the information you’ll need to fill out your IdP’s SAML form, with buttons to make copying the information easy.

However, the names of the fields in the Answers SAML form won’t always match the names used by your IdP. We’ve provided a description of each field below to help you map information from one place to another.

### URL the IdP should redirect back to

The redirect URL is the web address that people will be sent to after signing in with your IdP. To redirect people to your Answers, your redirect URL should be your Answers Site URL, with `/auth/sso/` at the end.

For example, if your Answers Site URL is `https://answers.yourcompany.com`, you’ll use `https://answers.yourcompany.com/auth/sso/` as the redirect URL in your IdP’s SAML form.

Different IdPs use different names for the redirect URL.

### User attributes

Answers will automatically log in people who’ve been authenticated by your SAML identity provider. In order to do so, the first assertion returned in the identity provider’s SAML response _must_ contain attributes for each person’s first name, last name, and email.

Most IdPs already include these assertions by default, but some (such as Okta) must be configured to include them.

Generally you’ll need to paste these user attributes (first name, last name, and email) into fields labelled “Name”, “Attributes” or “Parameters”.

Note: If you allow people to edit their email addresses: make sure to update the corresponding account emails in Answers. Keeping email addresses in sync will protect people from losing access to their accounts.

### Settings for signing SSO requests (optional)

These are additional settings you can fill in to sign SSO requests to ensure they don’t get tampered with.

## Enabling SAML authentication

Answers will now need to know some things about your IdP. Here’s a breakdown of each of the settings:

### SAML Identity Provider URL

Answers will redirect login requests to the Identity Provider URL, where people will go to log in with SSO.

### SAML Identity Provider Issuer

This is a unique identifier for the IdP. You might also see it referred to as “Entity ID” or “Issuer”. Assertions from the IdP will contain this information, and Answers will verify that it matches the value you set.

We recommend that you set this value to make your SAML configuration more secure.

### SAML Identity Provider Certificate

This is an encoded certificate that Answers will use when connecting to the IdP URI. The certificate will look like a big blob of text that you’ll want to copy and paste carefully — the spacing is important!

Your IdP might have you download this certificate as a file (usually `.cer` or `.pem`), which you’ll then need to open up in a text editor in order to copy the contents to then paste into the box in Answers.

Note that your certificate text may include header and footer comments that look like `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----`. These comments should be included when pasting your certificate text into Answers.

### Settings for signing SSO requests (optional)

These are additional settings you can fill in to sign SSO requests to ensure they don’t get tampered with. In addition, if your IdP encrypts SAML responses, you’ll need to ensure this section is filled out.

_Important note:_ If you change any of these settings, either during initial setup or after editing an existing value, you will need to restart Answers due to the way the keystore file is read.

**SAML keystore path:** the absolute path to the keystore file to use for signing SAML requests.

**SAML keystore password:** if it wasn’t already self-evident, this is just the password for opening the keystore.

**SAML keystore alias:** the alias for the key that Answers should use for signing SAML requests.

## Synchronizing group membership with your IdP

This setting allows you to assign users to Answers groups based on an attribute of your users in your IdP. Please note that this may not correlate to group functionality provided by your IdP — you may need to create a separate attribute on your users to set their Answers group, like `answersGroups`.

First, you will need to create a SAML user attribute that you will use to indicate which Answers groups the user should be a part of. This created user attribute can be a XML string or a list of XML strings. Different IdPs have different ways of handling this, but you will likely need to edit your user profiles or find a way to map a user’s groups to a list of Answers group names.

## Configuring the group schema in Answers

Once you’ve gotten everything set up in your SAML provider, there are just a few simple steps on the Answers side.

To start, make sure the toggle to synchronize group memberships is set to “Enabled.” Then, click Edit Mappings > Create a Mapping. Enter in the name of one of the groups you entered as your `answersGroups` attribute values, then click the Add button. Next click the dropdown that appears under the `Groups` heading to select the Answers group(s) that users with this particular `answersGroups` value should be added to. Then click Save.

After that, type in the name of the user attribute you added in your SAML provider. In this case, we told Okta that the `answersGroups` attribute should be named `AnswersGroupName`, so that’s what we’ll enter in the Group Attribute Name field in Answers.

## Creating Answers accounts with SSO

A new SSO login will automatically create a new Answers account.

Answers accounts created with an external identity provider login don’t have passwords. People who sign up for Answers using an IdP must continue to use the IdP to log into Answers.

## Disabling password logins

To require people to log in with SSO, disable password authentication from **Admin settings** > **Authentication**.

<figure><img src="https://www.metabase.com/docs/latest/people-and-groups/images/password-disable.png" alt=""><figcaption></figcaption></figure>

## New user notification emails

When users log in to Answers for the first time via SSO, this will automatically create a Answers account for them, which will trigger an email notification to Answers administrators. If you don’t want these notifications to be sent, you can turn this toggle off at the bottom of the Authentication page.
