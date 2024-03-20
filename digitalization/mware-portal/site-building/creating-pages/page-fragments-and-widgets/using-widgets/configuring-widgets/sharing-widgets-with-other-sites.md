# Sharing Widgets with Other Sites

You can share widgets with other sites by embedding an instance of a widget running on your site into another website, such as Facebook, OpenSocial and Netvibes. This opens up a whole new avenue of exposure to your website that you would not have had otherwise.

![The Sharing tab in your widget's Configuration menu can share your widget in a variety of ways.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-widgets/configuring-widgets/sharing-widgets-with-other-sites/images/01.png)

1. Click the widget’s _Options_ icon (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) → _Configuration_ → _Sharing_.
2. There are three sub-tabs under sharing: Facebook, OpenSocial Gadget and Netvibes. Proceed according to the context you choose.

NOTE

With any website, switch the toggle to allow the navigation. Copy and paste the provided snippet of JavaScript code into the website where you want to add the widget. When a user loads the page on the other website, the code pulls the relevant widget from your Site and displays it.

### Facebook <a href="#facebook" id="facebook"></a>

You can add any widget as a Facebook app. To do this, you must first get a developer key.

1. Go to the Facebook tab on Sharing.
2.  Retrieve the _API Key_ and the _Canvas Page URL_ from Facebook. Copy and paste them on the Sharing tab.

    ![The Sharing tab in your widget's Configuration menu can share your widget in a variety of ways.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-widgets/configuring-widgets/sharing-widgets-with-other-sites/images/02.png)
3. Click _Save_ and go back to the Facebook tab in MWARE Portal. You’re given the _Callback URL_, which you can copy and paste into Facebook. When opening your app in Facebook, the correct callback URL is used to render the application.
4. Optionally, enable the _Allow users to add \[application-name] to Facebook_. Then you can navigate to your app’s Options menu and select _Add to Facebook_.

### OpenSocial Gadget <a href="#opensocial-gadget" id="opensocial-gadget"></a>

OpenSocial comprises a container and a set of APIs for social networking and other web applications. MWARE Portal can serve up applications to be used as OpenSocial Gadgets on any OpenSocial-compatible pages.

1.  To serve a portal widget on an OpenSocial platform, copy and paste the provided gadget URL, add it to the appropriate configuration page of the OpenSocial platform you’re using. Your portal instance serves that widget directly onto that platform’s page.

    The URL provided is unique to the specific instance of the widget, so you could serve multiple instances of the same widget as different OpenSocial Gadgets.
2. Go to _OpenSocial Gadget_ on the Sharing tab, and enable the selector _Allow users to add \[application-name] to an OpenSocial platform_.
3. Click _Save_ and revisit the _Options_ button of your widget. A new button appears named _Add to an OpenSocial Platform_. When selecting this new button, the URL is provided for sharing the widget to an OpenSocial platform.

### Netvibes <a href="#netvibes" id="netvibes"></a>

Netvibes offers a similar environment where users can log in, create their own personal dashboard, and add customizable widgets to it.

Go to _Netvibes_ on the Sharing tab, enable the _Allow users to add \[application-name] to Netvibes pages_ selector. You can then use the provided URL to create a custom Netvibes widget based on the instance of the portal widget that you’re using.

\
