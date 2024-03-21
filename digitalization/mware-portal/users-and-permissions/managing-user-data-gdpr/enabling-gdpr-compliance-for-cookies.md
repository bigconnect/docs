# Enabling GDPR Compliance for Cookies

You can ensure [GDPR compliance](https://gdpr.eu/cookies) for your cookies using the portal’s configuration interface.

1.  Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), and navigate to _Control Panel_ → _System Settings_ → _Platform_ → _Cookies_.

    ![Ensure GDPR compliance for your cookies through the configuration interface.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/managing-user-data/enabling-gdpr-compliance-for-cookies/images/01.png)

    There are two checkboxes here: Enabled and Explicit Cookie Consent Mode. Though enabled by default, the explicit cookie consent mode doesn’t work if the Enabled checkbox is unchecked.
2. To set your handling preference, check the _Enabled_ checkbox. With the Explicit Cookie Consent Mode checked, no cookies are set until the user explicitly agrees to cookie usages. If unchecked, all cookies are set until the user opts out of them.
3.  Click _Update_.

    ![The default cookie banner appears at the bottom of the page.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/managing-user-data/enabling-gdpr-compliance-for-cookies/images/02.png)

    Two new options appear in the left menu: _Banner_ and _Consent Panel_.

    ![Two new options appear in the left menu.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/managing-user-data/enabling-gdpr-compliance-for-cookies/images/03.png)

    NOTE

    This applies the configuration to all sites. If you want to configure cookie settings for a specific site, go to _Control Panel_ → _Instance Settings_ → _Platform_ → _Cookies_.
4.  Go to the _Banner_ tab and choose your options for the cookie banner (see Banner Options below).

    ![Find customizable options for your cookie banner under the Banner tab.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/managing-user-data/enabling-gdpr-compliance-for-cookies/images/04.png)
5. Click _Save_.
6.  Go to the _Consent Panel_ tab and choose your options for the consent panel (see Consent Panel Options below).

    ![Find customizable options for the consent panel under the Consent Panel tab.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/managing-user-data/enabling-gdpr-compliance-for-cookies/images/05.png)
7. Click _Save_.

### Banner Options <a href="#banner-options" id="banner-options"></a>

Your cookie policy appears when users log in through a banner. It displays the text from the Content field, your privacy Policy Link, and Link Display Text. Users can select from three options: _Configuration_, _Accept All_ and _Decline All_. You can customize these options in the Banner tab.

![The cookie banner appears the first time users visit your site.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/managing-user-data/enabling-gdpr-compliance-for-cookies/images/06.png)

**Content**: Add the information included in your cookie policy, like the types of cookies used, their purposes, how users can manage or disable cookies, and any other relevant information regarding data collection, tracking, and user privacy.

**Privacy Policy Link**: Add a link to your site’s privacy policy page.

**Link Display Text**: Specify the text that appears as the link to the privacy policy page.

**Include “Decline All” Button**: Check this option to add a _Decline All_ button on the cookie banner.

### Consent Panel Options <a href="#consent-panel-options" id="consent-panel-options"></a>

The Consent Panel appears when users click the _Configuration_ link on the banner. Users can use it to select the types of cookies they consent to have stored.

![Consent Panel Page](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/managing-user-data/enabling-gdpr-compliance-for-cookies/images/07.png)

You may customize the following options:

**Title**: Adds a title to your consent panel.

**Description**: Add a general overview about the usage of cookies on your site.

**Cookie Policy Link**: Add a link to your site’s cookie policy page.

**Link Display Text**: Specify the text that appears as the link to the cookie policy page.

**Strictly Necessary Cookies Description**: Describe the cookies that are necessary for the proper functioning of your site.

**Functional Cookies Description**: Describe cookies you store to provide enhanced functionality and personalization. You can check the _Prechecked_ checkbox to enable this when the user opens the consent panel.

**Performance Cookies Description**: Describe performance cookies you store, including how they measure and improve the overall performance of the website by counting visits, traffic sources, and page popularity, and any steps you have taken to maintain full data privacy. You can check the _Prechecked_ checkbox to enable this when the user opens the consent panel.

**Personalization Cookies Description**: Describe cookies that personalize the user experience. For example, you might have cookies that build a profile of users to show relevant ads. By not allowing personalization cookies, users may receive less targeted advertising. You can check the _Prechecked_ checkbox to enable this when the user opens the consent panel.
