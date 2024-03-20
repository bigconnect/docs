# Configuring Hreflang Meta Data

Hreflang tags are HTML attributes used to identify the language and geographic region targeted by alternative versions of a Site Page. In some cases, these attributes can help search engines serve the correct version of a Page to users and contribute to SEO. By default, the portal populates Page hreflang meta data with all available languages in the Site.

Beginning with version 7.4+, you can configure this behavior to only generate hreflang meta data for languages with translated content in a Page. This can be configured at both the instance and Site levels.

When hreflangs are limited in this way, the portal’s behavior can vary depending on Page type.

* **Widget Pages**: Languages used to translate a Widget Page’s name are added to the hreflang tags.
* **Content Pages**/**Display Page Templates**: Languages used to translate a Page Fragment are added to the hreflang tags.

### Instance Settings <a href="#instance-settings" id="instance-settings"></a>

Follow these steps to configure hreflang behavior for your porral instances:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _Instance Settings_ → _Pages_ → _SEO_ (Virtual Instance Scope).
2.  Check/Uncheck _Enable Only Translated Hreflangs_.

    ![Enable or disable the hreflang behavior for all instance Pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/configuring-hreflang-meta-data/images/01.png)
3. Click _Save_.

When enabled, hreflang meta data is only generated for languages with translated content in a Page. When disabled, Page hreflang tags are populated with all available languages in a Site.

### Instance Site Scope Settings <a href="#instance-site-scope-settings" id="instance-site-scope-settings"></a>

Follow these steps to configure hreflang behavior for all Sites in an instance using the Site Scope:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _Instance Settings_ → _Pages_ → _SEO_ (Site Scope).
2.  Check/Uncheck _Enable Only Translated Hreflangs_.

    ![Enable or disable the hreflang behavior for all Sites.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/configuring-hreflang-meta-data/images/02.png)
3. Click _Save_.

When enabled, hreflang meta data is only generated for languages with translated content in a Page. When disabled, Page hreflang tags are populated with all available languages in a Site.

NOTE

By default, Site Scope instance settings are applied to all instance Sites. However, individual Site configurations override configurations in the Global Menu. To restore the default behavior and synchronize changes between both menus, you must restore the individual Site’s settings to default.

### Individual Site Settings <a href="#individual-site-settings" id="individual-site-settings"></a>

Follow these steps to configure hreflang settings for individual Sites:

1. Navigate to the desired _Site_.
2. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Configuration_, and go to _Site Setting_ → _Pages_ → _SEO_.
3.  Check/Uncheck _Enable Only Translated Hreflangs_.

    This configuration only affects Pages in the current Site.

    ![Enable or disable the hreflang behavior for a Site's pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/configuring-hreflang-meta-data/images/03.png)
4. Click _Save_.

When enabled, hreflang meta data is only generated for languages with translated content in a Page. This configuration affects all Pages in the current Site and overrides the instance configurations settings.

NOTE

By default, individual Site settings are inherited from Site Scope instance settings. However, individual Site configurations override this behavior. To restore the default behavior and synchronize changes between both menus, you must restore the individual Site’s settings to default.

\
