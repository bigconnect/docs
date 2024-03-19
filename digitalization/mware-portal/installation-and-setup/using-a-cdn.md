# Using a CDN

A Content Delivery Network (CDN) is a distributed infrastructure for caching static, file-based content. When visitors hit your site, your static content is loaded from the server closest to them, speeding up requests.

The CDN functions as a caching proxy. This means that once static content is copied to a local server, it’s stored in a cache for quick and easy retrieval. This improves latency time, because browsers can download static resources from a local server down the street instead of halfway around the world. An algorithm finds the server closest to the request. The figure below depicts using geographical proximity to minimize latency.

![Using CDN allows a user to request static resources from a much closer local server, improving download times.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/setting-up-liferay/using-a-cdn/images/01.png)

The red lines on the map represent the required distances traveled by requests from a server to the user. Because of the reduced wait time for requests and reduced load on your application server, a CDN is a great option to improve performance.

Here are the resources Liferay supports serving via a CDN:

* Files included in Liferay and custom plugins
* Items stored in Documents and Media
* Dynamically generated CSS \*
* Dynamically generated JavaScript \*
* Dynamically generated images \*

_\* Requires enabling CDN dynamic resources._

NOTE

The CDN does not serve resources that are dynamically loaded from external sources.

### [Liferay CDN Requirements](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/using-a-cdn#liferay-cdn-requirements) <a href="#liferay-cdn-requirements" id="liferay-cdn-requirements"></a>

The CDN must satisfy these requirements:

* _Dynamically store and retrieve resources:_ Check with your CDN provider to make sure you don’t have to upload anything manually for the CDN to work. The CDN must automatically fetch the resources.
* _Work like a transparent proxy:_ A request first goes to the CDN. If the CDN doesn’t have the requested resource, the CDN makes an identical request back to the origin (Liferay), caches the resource, then serves the resource.

After setting up a qualified CDN, configure Liferay to use it.

### [Configuring Liferay to Use a CDN](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/using-a-cdn#configuring-liferay-to-use-a-cdn) <a href="#configuring-liferay-to-use-a-cdn" id="configuring-liferay-to-use-a-cdn"></a>

Here are the two different methods for configuring a CDN:

1. Set [Content Delivery Network properties](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Content%20Delivery%20Network) in a [portal properties](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) file.
2. Use the Control Panel

Here’s how to configure your CDN in the Control Panel:

1. In Liferay, click the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and select the _Control Panel_ tab.
2. In the Configuration category, select _Instance Settings_. The Instance Settings page appears.
3. In the Virtual Instance Scope navigation, click _General_. The General Main Configuration page appears and includes these three CDN-related fields:
   * _CDN Host HTTP_
   * _CDN Host HTTPS_
   * _Enable CDN Dynamic Resources_

![Configure the CDN in the Control Panel.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/setting-up-liferay/using-a-cdn/images/02.png)

Make sure to include the protocol and domain when specifying your CDN host(s) URLs:

_CDN Host HTTP:_ `http://cdnhost1.liferay.com` _CDN Host HTTPS:_ `https://cdnhost2.liferay.com`

When _CDN Dynamic Resources_ is selected (the default), Liferay serves dynamically generated CSS, JavaScript, and images via the CDN. Disabling this allows you to use CDNs that do not support lazy loading of resources.

You’ve configured the CDN host. Now Liferay generates static resource URLs that replace the old host with your new CDN host so they are automatically cached and served afterwards by the CDN.

### [Using a CDN in Custom Plugins](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/using-a-cdn#using-a-cdn-in-custom-plugins) <a href="#using-a-cdn-in-custom-plugins" id="using-a-cdn-in-custom-plugins"></a>

If you want to add a custom resource to the CDN, package it in your plugin. For example, include the resource in your theme plugin or reference a JavaScript file in your portlet plugin’s `liferay-portlet.xml` file.

To use a resource via the CDN, prefix the resource reference with the CDN host name, using `themeDisplay.getCDNDynamicResourcesHost()`.

IMPORTANT

Don’t put anything on the CDN which requires permission checking or complex policy access

Now you know how to serve your site’s static resources faster using a CDN.
