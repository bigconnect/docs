# JWT-based authentication

You can connect Answers to your identity provider using JSON Web Tokens (JWT) to authenticate people.

## Authentication flows

Answers supports two auth flows that can be used with JWT:

* Authorization Code Flow
* Authorization Code Flow with PKCE

Answers’s auth flows are custom workflows modelled after OAuth 2.0. You can use the auth flow with PKCE to incorporate random keys generated on demand.

Currently, the only algorithm Answers supports is [HS256](https://en.wikipedia.org/wiki/JSON_Web_Token) ([HMAC](https://en.wikipedia.org/wiki/HMAC) + [SHA-256](https://en.wikipedia.org/wiki/SHA-2)).

## Typical flow for a JWT-based SSO interaction with Answers

Assuming your site is localhost serving on port 3000:

1. Person attempts to view a question, e.g., `http://localhost:3000/question/1-superb-question`.
2. If the person isn’t logged in, Answers redirects them to `http://localhost:3000/auth/sso/`.
3. Retaining the original `/question/1-superb-question` URI, Answers redirects the person to the SSO provider (the authentication app).
4. Person logs in using the basic form.
5. In the event of a successful sign-in, your authentication app should issue a GET request to your Answers endpoint with the token and the “return to” URI: `http://localhost:3000/auth/sso?jwt=TOKEN_GOES_HERE&return_to=/question/1-superb-question`.
6. Answers verifies the JSON Web Token, logs the person in, then redirects the person to their original destination, `/question/1-superb-question`.

## Enabling JWT authentication

Navigate to the **Admin**>**Settings** section of the Admin area, then click on the **Authentication** tab. Click the **Configure** button in the JWT section of this page, and you’ll see this form:

<figure><img src="https://www.metabase.com/docs/latest/people-and-groups/images/JWT-auth-form.png" alt=""><figcaption></figcaption></figure>

Here’s a breakdown of each of the settings:

**JWT Identity Provider URI:** This is where Answers will redirect login requests. That is, it’s where your users go to log in through your identity provider.

**String Used by the JWT Signing Key:** The string used to seed the private key used to validate JWT messages. Both Answers and the authentication app should have the same JWT signing key.

## User attribute configuration (optional)

These are additional settings you can fill in to pass user attributes to Answers.

* **Email attribute:** the key to retrieve each JWT user’s email address.
* **First Name attribute:** the key to retrieve each JWT user’s first name.
* **Last Name attribute:** if you guessed that this is the key to retrieve each JWT user’s last name, well then you have been paying attention.

You can send additional user attributes to Answers by adding the attributes as key/value pairs to your JWT. These attributes will be synced on every login.

## Group schema

You can use your JWT to assign Answers users to custom groups by following these steps:

1. Add this to your JWT: `groups: ["group_name"]`
2. In the Admin Panel in Answers, go to the Authentication tab of the Settings section and click the Configure button on JWT. On this screen, turn on the toggle under “SYNCHRONIZE GROUP MEMBERSHIPS”.
3. Next, click Edit Mappings. In this modal, type in the name of one of your groups as defined in the JWT, then click Add. In the row that appears, click the dropdown to pick the Answers group that this should map to. Repeat this for each of the groups you want to map.

## Creating Answers accounts with SSO

A new SSO login will automatically create a new Answers account.

Answers accounts created with an external identity provider login don’t have passwords. People who sign up for Answers using an IdP must continue to use the IdP to log into Answers.

## Disabling password logins

> **Avoid locking yourself out of your** Answer&#x73;**!** This setting will apply to all Answers accounts, _including your Answers admin account_. We recommend that you keep password authentication **enabled**. This will safeguard you from getting locked out of Answers in case of any problems with SSO.

To require people to log in with SSO, disable password authentication from **Admin settings** > **Authentication**.

<figure><img src="https://www.metabase.com/docs/latest/people-and-groups/images/password-disable.png" alt=""><figcaption></figcaption></figure>
