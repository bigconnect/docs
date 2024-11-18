# Enforce SSO

Enforced SSO is a feature that allows login from a configured identity provider to be required for all of your users. Any existing password users will no longer be able to login after enabling the feature.

To access the feature you can navigate to the Settings section of your application, then Auth.

<figure><img src="https://files.readme.io/7d50982-Screenshot_2023-03-04_at_00.00.32.png" alt="Enforce SSO in the Auth settings"><figcaption><p>Enforce SSO in the Auth settings</p></figcaption></figure>

### Usage

Use the provided Single Sign-On URL to access your tenants unique login page. Here you will find your configured identity provider(s).

<figure><img src="https://files.readme.io/4bdf517-Screenshot_2023-03-04_at_00.11.39.png" alt="Login page showing enforced SSO" width="375"><figcaption><p>Login page showing enforced SSO</p></figcaption></figure>

### Troubleshooting

#### Testing the feature

When first enabling enforced SSO it is recommended to stay logged into your active session where you can make changes to settings, while using another private browsing window to login using a different account.

#### What to do if you are experiencing lockout

If you are having a problem with your identity provider you may be subject to lockout. Follow the SSO Maintenance Mode guide.
