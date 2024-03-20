# Configuring Your Site’s Friendly URL

Friendly URLs are readable URLs that can helps Users quickly access your Site. Each URL must be unique and is appended to `https://localhost:8080/web` (e.g., `https://localhost:8080/web/helloworld/`).

NOTE

When Private Pages are enabled, you can set friendly URLs for them as well. Once set, they are appended to `https://localhost:8080/group`.

### Setting a Site’s Friendly URL <a href="#setting-a-sites-friendly-url" id="setting-a-sites-friendly-url"></a>

Follow these steps to set your Site’s friendly URL:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Configuration_, and go to _Site Settings_ → _Site Configuration_ → _Site URL_.
2. Enter a _Friendly URL_.
3. Click _Save_.

![You can configure a friendly URL for your Site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/configuring-your-sites-friendly-url/images/01.png)

If you want to add a friendly URL for your Site’s Home Page, follow the additional steps in the next section.

### Setting Your Instance’s Home URL <a href="#setting-your-instances-home-url" id="setting-your-instances-home-url"></a>

If you add a friendly URL for your instance’s Home Page, you should update your instance’s Home URL field so that page requests to `http://localhost:8080` redirect properly:

1. Open the Global Menu ( ![Global Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), click the _Control Panel_ tab, and go to _Instance Settings_.
2. Select _Instance Configuration_ under the _Platform_ heading and click the _General_ link.
3. Under _Navigation_, enter your new friendly home URL into the _Home URL_ field. For example, setting the friendly URL of your default Site to `/my-site` makes your Site’s public Home Page’s URL `https://localhost:8080/web/my-site/home`. So, you would enter `/web/my-site/home`.

![Enter the updated home URL to redirect to your new friendly URL.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/configuring-your-sites-friendly-url/images/02.png)

Once you’ve entered this setting, page requests to `localhost:8080` redirect to the friendly URL of your portal instance’s new Home Page.
