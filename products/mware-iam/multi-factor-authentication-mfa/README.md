# Multi-factor Authentication (MFA)

This section describes multi-factor authentication (MFA) and guides you on how to enable MFA for the WSO2 Identity Server Management Console. By default, WSO2 Identity Server is shipped with username-and-password-based authentication. You can further strengthen the security of this authentication by adding an additional authentication step to authenticate with basic authentication, FIDO, TOTP, etc.

#### Shortcomings in traditional authentication <a href="#shortcomings-in-traditional-authentication" id="shortcomings-in-traditional-authentication"></a>

Due to increasing digital crimes and internet fraud in the world, people are highly concerned about the topic of online security. It is obvious that traditional user Id and password is not enough to secure the authentication. Processing speeds of CPUs have increased, so brute force attacks are a reality and dictionary attacks have become a common threat. GPGPU password cracking and rainbow tables have provided similar advantages to attackers.

#### Benefits of MFA <a href="#benefits-of-mfa" id="benefits-of-mfa"></a>

MFA creates a layered defence and makes it more difficult for an unauthorized person to access a target such as a physical location, computing device, web service, network, or database. If one factor is compromised or broken, the attacker still has at least one more barriers to breach before successfully breaking into the target.

#### Authentication factors <a href="#authentication-factors" id="authentication-factors"></a>

Authentication factors in MFA relies on two or more independent credentials of the three categories.

* **Knowledge factors**: Things only the user knows, such as passwords.
* **Possession factors**: Things only the user has, such as ATM cards.
* **Inherence factors**: Things only the user is, such as a fingerprint.

With a combination of two or more factors from the above three, the user is authenticated.

#### MFA with WSO2 Identity Server <a href="#mfa-with-wso2-identity-server" id="mfa-with-wso2-identity-server"></a>

MWARE IAM allows configuring multi-step authentication where you can define an authentication chain containing different authenticators in different steps. This chain can also be converted to a multi-factor authentication chain by adding different factors of authentication to each step. For example, you can configure user name-password-based authentication as the first factor (knowledge factors) and then FIDO authentication as the second factor (possession factors).

![mfa-with-is](https://is.docs.wso2.com/en/latest/assets/img/using-wso2-identity-server/mfa-with-is.png)

MWARE IAM has comprehensive support for MFA, with authenticators available for **SMSOTP, FIDO, MEPin** and more.

#### MFA with federated authenticators <a href="#mfa-with-federated-authenticators" id="mfa-with-federated-authenticators"></a>

MWARE IAM enables configuring MFA in the following ways when a federated identity provider (IdP) is configured as the first factor.

* Configuring MFA based on the claims that are provided by the locally provisioned/associated user of the federated IdP in the first factor. (Recommended)
* Configuring the second factor based on the claims that are provided by the federated IdP in the first factor. (Not recommended)
