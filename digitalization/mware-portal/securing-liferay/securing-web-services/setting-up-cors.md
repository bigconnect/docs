# Setting Up CORS

CORS stands for Cross-Origin Resource Sharing. An Origin is a web server at a different domain, and a Resource is some asset stored on the server, like an image, PDF, or HTML file. Sometimes you must request resources stored on another origin. This is called a cross-origin request, and web servers have policies to allow or deny such requests.

For example, browsers themselves don’t allow cross-origin AJAX-style requests from scripts to help mitigate [cross-site scripting](https://en.wikipedia.org/wiki/Cross-site\_scripting) attacks. These APIs follow a _same origin_ policy. But for certain resources, it can be convenient to allow Liferay DXP to serve them to different origins.

For example, if you manage images in Docs & Media, you may want to allow cross-origin requests for them. You can enable CORS for matching URLs in Liferay Portal or for JAX-RS application resources.

### [Enabling CORS for Liferay DXP Services](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/setting-up-cors#enabling-cors-for-liferay-dxp-services) <a href="#enabling-cors-for-liferay-dxp-services" id="enabling-cors-for-liferay-dxp-services"></a>

You’ll find the settings in Configuration → System Settings → Security → Security Tools → Portal Cross Resource Origin Sharing (CORS):

1. Click _Add_ to create a configuration entry.
2.  Fill out the fields on the form. When finished, click _Save_.

    ![Figure 1: The CORS system settings provide a way to configure CORS headers for Liferay services.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/securing-web-services/setting-up-cors/images/01.png)

#### [Portal CORS Configuration Reference](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/setting-up-cors#portal-cors-configuration-reference) <a href="#portal-cors-configuration-reference" id="portal-cors-configuration-reference"></a>

| Configuration             | Description                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Enabled**               | Check this box to enable the entry.                                                                                                                                                                                                                                                                                                                                                                             |
| **Name**                  | Give the configuration entry a name.                                                                                                                                                                                                                                                                                                                                                                            |
| **URL Pattern**           | <p>Use the Plus button to add as many patterns as you need. Define patterns that match URLs to the resources you want to share. For example, if you have many attachments in the Knowledge Base application, you could define this pattern:<br><code>/knowledge_base/*</code><br>This would define resources stored in the Knowledge Base as applicable to the policy you set in the response header below.</p> |
| **CORS Response Headers** | Use the Plus button to add as many headers as you need. Define policies for any of the [CORS headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#CORS) here.                                                                                                                                                                                                                                     |

You can also use a configuration file to configure CORS.

### [Enabling CORS for JAX-RS Applications](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/setting-up-cors#enabling-cors-for-jax-rs-applications) <a href="#enabling-cors-for-jax-rs-applications" id="enabling-cors-for-jax-rs-applications"></a>

You’ll find the settings in Configuration → System Settings → Security → Security Tools → Web Contexts Resource Origin Sharing (CORS):

1. Click _Add_ to create a configuration entry.
2.  Fill out the fields on the form. When finished, click _Save_.

    ![Figure 2: There's a separate system settings category for CORS web contexts.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/securing-web-services/setting-up-cors/images/02.png)

#### [JAX-RS CORS Configuration Reference](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/setting-up-cors#jax-rs-cors-configuration-reference) <a href="#jax-rs-cors-configuration-reference" id="jax-rs-cors-configuration-reference"></a>

| Configuration                       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dynamic Web Context OSGi Filter** | <p>Define an LDAP-style <a href="https://osgi.org/specification/osgi.cmpn/7.0.0/service.http.whiteboard.html">filter</a> to define which JAX-RS whiteboard applications the CORS headers in this entry apply to. This is the default filter:<br><code>(&#x26;(!(liferay.cors=false))(osgi.jaxrs.name=*))</code><br>It applies CORS headers to all deployed JAX-RS whiteboard applications without a <code>liferay.cors=false</code> property. This helps during development, but in production you should use the narrowest configuration possible.</p> |
| **URL Pattern**                     | Use the Plus button to add as many patterns as you need. Define patterns that match URLs to the web services you want to access.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **CORS Response Headers**           | Use the Plus button to add as many headers as you need. Define policies for any of the [CORS headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#CORS) here.                                                                                                                                                                                                                                                                                                                                                                             |

JAX-RS developers can use the `@CORS` annotation to set policies for their deployed applications.

\
