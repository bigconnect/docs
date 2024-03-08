# Configuring Virtual Hosts Site URLs

A virtual host connects a domain name (e.g., `www.helloworld.com`) to a Site. This can be a full domain or a subdomain (e.g. `developers.helloworld.com`). You can use this to host a number of websites as separate Sites on one Liferay DXP server. Follow these steps to configure virtual hosts Site URLs:

1. With your provider, set the DNS name to point to your Liferay DXP instance’s IP address.
2.  Inside Liferay DXP, open the Site Menu (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Configuration_, and go to _Site Settings_ → _Site Configuration_ → _Site URL_.

    ![Under Site Configuration, go to Site URL.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/configuring-virtual-hosts-site-urls/images/01.png)
3.  Enter the desired domain or subdomain in the _Virtual Host_ field (e.g., `www.helloworld.com`, `http://developers.helloworld.com`).

    ![You can point virtual hosts to specific locales.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/configuring-virtual-hosts-site-urls/images/02.png)

    IMPORTANT

    A domain can only be applied to a single Site. You can’t map a single domain to multiple Sites.
4. (Optional) Since Liferay 7.3 GA2, click the _Plus_ button (![Plus Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png)) for additional Virtual Host domains.
5.  (Optional) Since Liferay 7.3 GA2, select a language for each virtual host. If you have a domain for each translation of a Site, you can add a virtual host for each domain.

    NOTE

    Locales are constrained to the Site’s available languages, if defined. If a locale isn’t specified, the default locale for the Site is used. The locale applies to Sites for all associated domains.

    ![You can add multiple virtual hosts to point to different domains.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/configuring-virtual-hosts-site-urls/images/03.png)
6. Click _Save_.

Your site is now accessible using the configured domain.

\
