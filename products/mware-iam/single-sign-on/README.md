# Single Sign-On

Single sign-on (SSO) is one of the key features of MWARE IAM that enables users to provide their credentials once and obtain access to multiple applications. The users are not prompted for their credentials when accessing each application until their session is terminated. Additionally, the user can access all these applications without having to log into each and every one of them individually. So, if users log into application A, for example, they would automatically have access to application B as well for the duration of that session without having to re-enter their credentials.

MWARE IAM can act as the identity provider of a single sign-on system with minimal configurations. This topic briefly introduces single-sign-on and how to configure the MWARE IAM with different inbound authenticators by Configuring a Service Provider to achieve this.

## About SSO <a href="#about-sso" id="about-sso"></a>

Single Sign-On which is known as SSO, is a property of access control for independent software systems which are multiple related. With this property, a user can access to a connected system or systems using one user name and password without using a different user name or password.

In a single sign-on system there are two roles; Service Providers and Identity Providers (IP). The important characteristic of a single sign-on system is the pre-defined trust relationship between the service providers and the identity providers. Service providers trust the assertions issued by identity providers which are essentially statements reading the authentication, authorization, and attributes related to the principal. Identity providers issue assertions based on the results of authentication and authorization of principles which access services on the service provider's side.

The following are some of the advantages you can have with SSO:

* Users need only a single username/password pair to access multiple services. Thus they do not have the issue of remembering multiple username/password pairs.
* Users are authenticated only once at the identity provider and then they are automatically logged into all services within that "trust-domain". This process is more convenient to users since they do not have to provide their username/password at every service provider.
* Service providers do not have the overhead of managing user identities, which is more convenient for them.
* User identities are managed at a central point. This is more secure, less complex and easily manageable.

## SSO in reality <a href="#sso-in-reality" id="sso-in-reality"></a>

Single Sign-On is widely used in web technologies. Google is one of the best examples.

Try this simple exercise,

1. Visit [www.google.com](https://www.google.com/) from your web browser.
2. Click on the SIGN IN button on the top right of the page.
3. Once you sign in, you are redirected to [https://accounts.google.com/ServiceLogin](https://accounts.google.com/ServiceLogin). There you are requested to enter your Username and Password. Enter your Google credentials there.
4. Once you enter your Username and Password, you are directed back to [www.google.com](https://www.google.com/) where you started.
5. Next visit [www.gmail.com](https://www.gmail.com/), the Google mail server.
6. Notice that you are automatically signed in and you directly access your Gmail Inbox. You did not have to enter your Username and Password at Gmail.
7. In addition to that; now try [www.youtube.com](https://www.youtube.com/).
8. You are automatically signed in. You do not have to enter your username and password at YouTube.

Single Sign-On (SSO) requires you to sign in only once but provides access to multiple resources without having to re-enter your username and password.

## SSO and Federation <a href="#sso-and-federation" id="sso-and-federation"></a>

You use SSO on it's own or use SSO and Federation coupled together. Identity Federation involves configuring a third party identity provider as the federated authenticator to login to an application. When federation is coupled with SSO, the user can log in to one application using the credentials of the federated authenticator, and simultaneously be authenticated to other connected applications without having to provide credentials again.

For instance, you can set up google as a federated authenticator and then set up SSO between App1 and App2. This will allow users to log in to App1 using their google credentials. Once the user is logged in, when the user attempts to access App2, he/she will not be prompted for credentails again and is logged in automatically.
