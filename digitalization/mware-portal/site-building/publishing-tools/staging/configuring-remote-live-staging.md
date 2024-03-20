# Configuring Remote Live Staging

With Remote Live staging, your staging and live environments are hosted on separate portal servers. When enabled, the site or asset library used to configure staging becomes your staging environment, and the remote server becomes your live environment.

![Remote staging hosts the environments on separate portal servers.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-remote-live-staging/images/01.png)

Before enabling Remote Live staging, you must configure the portal servers you want to use for your staging and live environments. You must also create a new blank site or asset library on your remote server and use its ID during staging configuration.

### Preparing Your Portal Servers <a href="#preparing-your-liferay-servers" id="preparing-your-liferay-servers"></a>

If you haven’t already, follow these steps to configure your portal servers for Remote Live staging:

1.  Specify a shared authentication key for your servers by adding the following properties to a [`portal-ext.properties`](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) file for both portal servers:

    ```log
    tunneling.servlet.shared.secret=[secret]
    tunneling.servlet.shared.secret.hex=[value]
    ```

    Each property value depends on your chosen encryption algorithm and whether you use hexadecimal encoding (recommended) or printable ASCII characters (less secure). If you decide to not use hexadecimal encoding, the value for `tunneling.servlet.shared.secret` must be ASCII compliant.

    The following key lengths are supported by the available encryption algorithms:

    **AES**: 128, 192, and 256 bit keys

    **Blowfish**: 32 - 448 bit keys

    **DESede (Triple DES)**: 112 or 168 bit keys. The portal ’s minimum key length does not support 56 bit keys.
2.  Add each server to the other’s list of allowed servers by adding the following property to each server’s `portal-ext.properties` file:

    ```properties
    tunnel.servlet.hosts.allowed=127.0.0.1,SERVER_IP,[OTHER_SERVER_IP]
    ```

    If the server has multiple IP addresses, each IP address must be added.

    IMPORTANT

    If you’re validating IPv6 addresses, you must configure the app server’s JVM to not force the usage of IPv4 addresses. For example, if you’re using Tomcat, add the `-Djava.net.preferIPv4Stack=false` attribute in the `$TOMCAT_HOME\bin\setenv.[bat|sh]` file.
3.  Update the remote instance’s _Tunnel Authentication Verifier Configuration_.

    To do this, go to the _Control Panel_ → _Configuration_ → _System Settings_ → _API Authentication_ → _Tunnel Authentication_.

    Click _/api/liferay/do_ and insert the additional IP addresses you’re using in the _Hosts Allowed_ field. When finished, click on _Update_.

    ![Update the remote instance's Tunnel Authentication Verifier Configuration via the Control Panel.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-remote-live-staging/images/02.png)

    NOTE

    While it is enabled by default, ensure each portal server’s tunneling servlet [authentication verifier](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers) is enabled.

    Alternatively, you can write this configuration into an OSGi file in your portal instance (e.g., `osgi/configs/com.liferay.portal.security.auth.verifier.tunnel.module.configuration.TunnelAuthVerifierConfiguration-default.config`):

    ```log
    enabled=true
    hostsAllowed=127.0.0.1,SERVER_IP,[Local server IP address]
    serviceAccessPolicyName=SYSTEM_User_PASSWORD
    urlsIncludes=/api/liferay/do
    ```
4. Restart both portal servers to implement your changes.

Once restarted, both servers are ready for staging configuration.

IMPORTANT

When applying patches to a remote staging environment, you must apply them to all your servers. Having servers on different patch levels is not a good practice and can lead to import failures and data corruption. It is essential that all servers are updated to the same patch level to ensure remote staging works correctly.

### Setting Up Remote Live Staging <a href="#setting-up-remote-live-staging" id="setting-up-remote-live-staging"></a>

Once you’ve prepared your portal servers, follow these steps to set up Remote Live staging for a site or asset library:

1.  Open the Staging application in your site or asset library.

    For Sites, go to _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) → _Publishing_ → _Staging_.

    For asset libraries, go to _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → _Applications tab_ → _Asset Libraries_. Then open the desired asset library and click on _Staging_ under Publishing.
2.  Select _Remote Live_. This reveals additional configuration fields.

    ![Select Remote Live staging.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-remote-live-staging/images/03.png)
3.  Enter the following Remote Live Connection details:

    **Remote Host/IP**: Enter your Remote server’s IP address into the _Remote Host/IP_ field. This should match the `tunnel.servlet.hosts.allowed` property specified in your `portal-ext.properties` file.

    **Remote Port**: Enter the port for your remote DXP instance into the _Remote Port_ field.

    **Remote Path Context**: Enter the _Remote Path Context_. This is only required if a non-root portal servlet context path is used on the remote portal server. Access to this context must not be blocked by a proxy or firewall.

    **Remote Site ID**/**Remote Asset Library ID**: Enter the _Site ID_/_Asset Library ID_ for the desired target.

    NOTE

    If you’re configuring an IPv6 address, it must contain brackets when entered into the _Remote Host/IP_ field (e.g., \[0:0:0:0:0:0:0:1]).

    If the remote server is a cluster, you can set the Remote Host/IP to the cluster’s load balanced IP address to increase the availability of the publishing process. See the Configuring Remote Staging in a Clustered Environment for details.
4. Determine whether to use a secure network connection (i.e., HTTPS) for publishing pages from staging to live.
5. Determine whether to define manually the remote URL used for the site or asset library.
6.  (For Sites Only) Select whether you want _Page Versioning_ enabled.

    ![Enable Page Versioning.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/04.png)
7.  Select the _data_ and _content_ types you want to stage.

    Asset libraries only support staging data for the Documents and Media and Web Content applications.

    With sites, you can stage data for any of the following applications.

    ![Select the data and content types you want to stage.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-remote-live-staging/images/05.png)

    WARNING

    When applications are checked, their data is copied, and it may not be possible to edit them directly on the live site. When unchecking an application, first make sure that any changes in staging are published, since they may be lost. See Managing Data and Content Types in Staging for more information.
8.  Click _Save_ to initiate the staging process. The duration of this process depends on the size of your site or asset library.

    NOTE

    If your attempt to enable Remote Live staging fails, please verify that you’ve properly prepared your servers.

Once the process is complete, you are ready to use Remote Live staging. See Site Staging UI Reference for information about navigating the staging environment’s publishing features.

### Remote Live Staging Permissions <a href="#remote-live-staging-permissions" id="remote-live-staging-permissions"></a>

When a user attempts to publish changes from staging to live, the portal passes the user’s email address, screen name, or user ID to the remote server to perform a permission check. To succeed, the publishing user must have identical credentials and permissions on both servers.

To give a local user permission to publish changes from staging to production/live, create an identical user account with identical permissions for the user on the remote server.

To simplify this process, you can use LDAP to copy selected user accounts from your local server to your remote server. See Connecting to an LDAP Directory for more information.

### Configuring Remote Staging’s Buffer Size <a href="#configuring-remote-stagings-buffer-size" id="configuring-remote-stagings-buffer-size"></a>

When you’re using Remote Live staging and you are publishing a large amount of content, this process can be slow and cause a large amount of network traffic. To help with this, the portal  transfers data piecemeal, instead of one large data dump. You can control the size of data transactions by setting the following portal property in your `portal-ext.properties` file:

```log
staging.remote.transfer.buffer.size
```

This property sets the file block sizes for remote staging. If a LAR file used for remote staging exceeds this size, the file is split into multiple files prior to transmission and then reassembled on the remote server. The default buffer size is 10 megabytes.

### Disabling Remote Live Staging <a href="#disabling-remote-live-staging" id="disabling-remote-live-staging"></a>

Disabling Remote Live staging disables the connection between your environments without deleting any data. Since no data is erased and no processes are started, disabling Remote Live staging is almost instantaneous.

WARNING

When you disable remote staging, you must ensure the live site or asset library is still accessible so both sides can communicate. Do not shut down your live environment. If the network connection is ever lost between environments, an error message appears and instructs you to forcibly disable staging.

Forcibly disabling staging erases your staging information from your staging server, not the content. The live environment remains in a locked state. A possible workaround is to create a new live site or asset library and import content to it, if necessary.

Follow these steps to disable Remote Live Staging:

1. Open the Staging application in your site or asset library.
2.  Click the _Actions_ button ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) located in the Application bar and select _Staging Configuration_.

    ![Click the Actions button in the Application bar, and select Staging Configuration](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-remote-live-staging/images/06.png)
3. Select _None_ for your configuration and click _Save_.

\
