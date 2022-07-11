# Integrated Windows Authentication

Integrated Windows Authentication (IWA) is a popular authentication mechanism that is used to authenticate users in Microsoft Windows servers. MWARE IAM provides support for IWA from version 4.0.0 onwards. This section gives an introduction to IWA authentication, provides information on how it works, and gives instructions on how to use it.

## Introduction to Integrated Windows Authentication <a href="#introduction-to-integrated-windows-authentication" id="introduction-to-integrated-windows-authentication"></a>



Integrated Windows Authentication (IWA) is an authentication mechanism introduced by Microsoft to authenticate users in Microsoft Windows NT based operating systems. IWA authentication provides an easier way for users to log in to web applications that use Windows Active Directory as an user store. This is a popular choice of authentication among Windows server users and administrators.

Most often, we log in to web applications by providing a user name and password in a HTML page. This type of authentication system is called form-based authentication. However, there are several other ways where the need for user name and password is eliminated when the user is already authenticated. Integrated Windows Authentication is one such method. It is known as a browser-based authentication mechanism because the authentication is handled by the browser. The web browser gets the credentials of the Windows logged in user and uses those credentials to authenticate the user with the help of the server and Active Directory. The only requirement for the user is to simply enter the Protected web application's URL in the browser, and the browser and server takes care of the rest of the authentication and automatically logs the user in (provided that the user is in the same domain and is a valid and authorized user to log in). If the authentication fails, the user is prompted to enter valid credentials to log in to the system.

## Authentication Procedure <a href="#authentication-procedure" id="authentication-procedure"></a>

The following diagram indicates how the authentication procedure works.

![iwa-sso-diagram](https://is.docs.wso2.com/en/latest/assets/img/tutorials/iwa-sso-diagram1.png)

1. The user (who is already authenticated to the Windows domain when they log in to Windows) sends a usual request to a protected page of a web application (protected by IWA).
2. The server rejects the request and sends a response saying the user needs to be authenticated using NTLM.
3. The client browser get the user's credentials which was used to log into Windows, takes the hash of it and sends it to the server.
4. With the hash received, the server looks up the user store and identifies the user and creates an unique and encrypted challenge to send back to the client browser. That challenge can be only decrypted using the user's password which the browser already has with itself.
5. The client browser decrypts the challenge with the user's credentials which the browser already knows, and sends the response back to the server.
6. The server checks whether the response for the challenge is correct and serves the user requested resource if the answer is correct. If the answer is wrong, the server denies the access to the requested resources and sends the unauthorized message.

## Pros and Cons of IWA <a href="#pros-and-cons-of-iwa" id="pros-and-cons-of-iwa"></a>

The main advantage of this authentication mechanism is that users do not need to explicitly provide their credentials. Once users are logged into the Windows domain they are automatically authenticated for the IWA enabled web apps if the user is a valid user.

In IWA authentication, the user name and password are not sent over the network. Instead it uses a hash function and a challenge response scheme to authenticate. That makes the authentication more secure from man-in-the-middle type of attacks.

The disadvantage is that since the authentication is done using the Windows Active Directory it needs both the client and the server to use Microsoft Windows NT based operating systems. Also the clients need to be connected to the domain hosted by the server. This can be used only within an intranet. Also IWA may need some configuration on certain browsers like Mozilla Firefox.

## Implementing IWA for a Java Web Application <a href="#implementing-iwa-for-a-java-web-application" id="implementing-iwa-for-a-java-web-application"></a>

IWA was initially developed by Microsoft as an authentication mechanism for their .NET based IIS servers. However, neither Java nor the server applications that host Java web applications (like Tomcat) have native support for IWA. There are several third party libraries which provide the ability of enabling the IWA for the Java web applications. Here are some of those libraries:

* **JCIFS** – JCIFS is an open source library that had been commonly used few years ago for IWA authentication of Java web applications. However this library is no longer maintained and it is not recommended to use because of security flaws.
* **JESPA** – JESPA is a commercial library that can be used to enable IWA in Java web applications.
* **Tomcat IWA** – This is tomcat's implementation of IWA. However this is not fully completed yet.
* **SPNEGO** – SPNEGO is an open source library. It uses Java Authentication and Authorization Service (JAAS) for the authentication.
* **WAFFLE** – WAFFLE is also an open source library. It can be configured easily to use with the Java web applications.

## Configuring IWA Single-Sign-On <a href="#configuring-iwa-single-sign-on" id="configuring-iwa-single-sign-on"></a>

MWARE IAM is capable of running in multiple platforms. However, the Integrated Windows Authentication (IWA) authenticator is designed only for the Windows server and enabling theIWA authenticator may cause conflict with other authenticators. Therefore, the IWA authenticator is not enabled in MWARE IAM by default but can be enabled in MWARE IAM with some configurations.

**Prerequisites:**

* Web Server
  * Windows Server 2003 or later
  * An Active Directory configured in the Windows server
  * IAM Server 5.1.0 or a later version
* Client
  * Microsoft Windows Operating System (XP, Vista, 7)
  * Internet Explorer 7+, Mozilla Firefox, Google Chrome (or any other web browser that support IWA)
  * Following are the steps to configure IWA in MWARE IAM.



**To configure IWA in IAM Server:**

1. Configure MWARE IAM to use your Active Directory as the user store (MWARE IAM is configured to use a built-in LDAP server by default).
2. Start the IAM server with `<iam_home>/bin/wso2server.bat` and check whether the user store is configured properly before the IWA is activated.
3. Start from this step if you have the IAM server already configured to use Active Directory.
4. Stop the IAM server if the server is already running.
5.  Add the following property to the `deployment.toml` file found in the `<IAM_HOME>/repository/conf` folder.

    ```toml
    [admin_console.authenticator.iwa_ui_authenticator]
    enable = true
    priority = 5
    ```

    This indicates to the IAM Server that " **IWAUIAuthenticator** " is to be enabled with a priority level of 5.&#x20;
6. Start the IAM Server. Now the server is configured to use the IWA authenticator.

**Usage:**

Access the IAM Server from a client machine (the user should be logged in to the domain of the server) by entering the IAM Server's URL (e.g., [https://192.168.4.108:9443/carbon](https://192.168.4.108:9443/carbon) ) from your client browser. You are logged into the IAM Server without having to enter your password. The following is a part of the server log when the user is logged with IWA:

![iwa-server-log](https://is.docs.wso2.com/en/latest/assets/img/tutorials/iwa-server-log.png)

Sometimes you may not be logged in automatically and you may be prompted to enter the username and password. The reason for that could be one of the following.

* The browser is either unable to do the IWA authentication or it is not configured to use the IWA authentication properly. The web server should be added to the trusted websites of the browser.
  * For Internet explorer, go to “Tools → Internet Options” and in the “security” tab select local intranet.\
    ![configure-iwa-for-browser-1](https://is.docs.wso2.com/en/latest/assets/img/tutorials/configure-iwa-for-browser1.png)
  * Click the **Sites** button. Then add the URL of IAM Server there.\
    ![configure-iwa-browser-2](https://is.docs.wso2.com/en/latest/assets/img/tutorials/configure-iwa-browser2.png)
  * For Firefox, type “ **about:config** ” in the address bar, ignore the warning and continue, this displays the advanced settings of Firefox. In the search bar, search for the key " **network.negotiate-auth.trusted-uris** " and add the IAM Server URL there.\
    ![configure-iwa-browser-3](https://is.docs.wso2.com/en/latest/assets/img/tutorials/configure-iwa-browser3.png)
* The user may be attempting to access the IAM Server from outside the domain of the user.
* The user may not have the sufficient permission within IAM Identity to log in to the system.
