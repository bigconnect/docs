# Verifying A/B Test Requirements

Before running A/B Testing on your Content pages, you must verify these requirements are met:

* Liferay DXP is connected to Analytics Cloud. Fore information on how to set up this connection, see [Connecting your Liferay DXP site to Analytics Cloud](https://learn.liferay.com/analytics-cloud/latest/en/connecting-data-sources/connecting-liferay-dxp-to-analytics-cloud.html).
* Your page is a [Content Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages). Widget Pages do not support Experiences for different Segments.
* The Content Page you intend to test is published.
*   You have the _Update_ permission in the Content Page.

    ![Selecting the Site in the Liferay DXP configuration for Analytics Cloud](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/ab-testing/verifying-ab-test-requirements/images/01.png)

    NOTE

    When setting up the connection to Analytics Cloud, you must select the Site containing the content you want to test. Click _Control Panel_ → _Configuration_ → _Instance Settings_ → _Analytics Cloud_ → _Analytics Cloud Connection_.

To verify or configure the _Update_ permission,

1. Go to _Site Administrator_ → _Site Builder_ → _Pages_.
2. Click the Actions menu (![Action Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the Content Page and Select _Permissions_.
3. Check or verify the _Update_ permission box for the Role that must have access to the Content Page.
4. Click _Save_.

\
