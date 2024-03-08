# Analyze SEO and Accessibility on Pages

Starting with Liferay DXP 7.4, you can use the Page Audit tool to examine your Pages’ SEO and accessibility. The Page Audit tool uses the [Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/) service, providing specific recommendations for Liferay DXP. Using this tool, content authors can

* Verify that Pages are optimized for search engine indexing and visibility, so Pages rank better in search results and reach the right audience.
* Check the Page’s compliance with best accessibility practices, improving the usability and overall experience for all individuals.
* Use actionable information to correct the SEO and Accessibility issues affecting the Page.

IMPORTANT

Since the Page Audit tool uses PageSpeed Insights, it only works with publicly accessible Pages. You cannot use the tool with Private Pages if they’re enabled for your Liferay system.

### [SEO and Accessibility Checks](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#seo-and-accessibility-checks) <a href="#seo-and-accessibility-checks" id="seo-and-accessibility-checks"></a>

The Page Audit tool in Liferay DXP evaluates the following elements:

SEO issues:

* Incorrect image aspect ratio
* Missing meta-description
* Link text
* Page blocked from indexing
* Invalid `hreflang`
* Invalid canonical URL
* Illegible font size
* Small tap targets
* Missing `<title>` elements
* Not all links are crawlable

Accessibility issues:

* Low contrast ratio
* Missing `img-alt` attribute
* Missing `input-alt` attribute

After running a Page audit, you can review a description for each issue, along with actionable information to fix the issue. For more information about each one of these elements, see the [Google Lighthouse documentation](https://web.dev/learn/#lighthouse).

### [Configuring the Page Audit Tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#configuring-the-page-audit-tool) <a href="#configuring-the-page-audit-tool" id="configuring-the-page-audit-tool"></a>

NOTE

Configuring the Page Audit tool requires administrative access to Liferay DXP.

The Page Audit tool is enabled by default, but you need to complete the tool configuration before running a Page audit. This configuration requires:

* Your PageSpeed Insights API key. Google requires this key to use PageSpeed Insights in an automated way and for running multiple queries per second. To create this API key, see [Get Started with the PageSpeed Insights API](https://developers.google.com/speed/docs/insights/v5/get-started) in the Google documentation.
* A Preferred Strategy for page audits. By default, the Page Audit tool uses a mobile strategy, but you can also configure a desktop strategy to analyze your Pages. The mobile strategy includes additional rules for auditing your content on small screens. The strategy you choose applies to every Page audit, so if you wish to audit the mobile and desktop versions of your Page, you must run each audit strategy separately.

![Configure the Page Audit tool API key and audit strategy](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages/images/01.png)

To configure the Page Audit tool, you configure the Google PageSpeed Insights setting in Liferay DXP. As with other settings in Liferay DXP, you can enable or disable the Page Audit tool at different scopes. To access these configuration settings you must have administrative privileges. For more information, see [Understanding Configuration Scope](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope).

#### [Configuring the Page Audit Tool for Your Current Site](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#configuring-the-page-audit-tool-for-your-current-site) <a href="#configuring-the-page-audit-tool-for-your-current-site" id="configuring-the-page-audit-tool-for-your-current-site"></a>

1. Select the Site Menu (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu-open.png)) and go to Configuration → Site Settings.
2. In the Content and Data section, click _Pages_.
3.  Under the Site Scope section, click _Google PageSpeed Insights_.

    ![Use the Google PageSpeed Insights settings at the current Site scope to configure the Page Audit tool.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages/images/02.png)

    NOTE

    The Google PageSpeed Insights setting is available at the Site Scope only when the setting is enabled as a higher scope. For more information, read [Understanding Configuration Scope](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope).
4. Verify that the Enable Google PageSpeed Insights box is checked.
5. Copy your Google PageSpeed Insights API key and paste this key under the API Key field.
6. Choose your Preferred Strategy for the PageSpeed Insights tests. The selected Strategy applies to all Page audits.
7. Click _Save_.

#### [Configuring the Page Audit Tool for the Liferay DXP System](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#configuring-the-page-audit-tool-for-the-liferay-dxp-system) <a href="#configuring-the-page-audit-tool-for-the-liferay-dxp-system" id="configuring-the-page-audit-tool-for-the-liferay-dxp-system"></a>

1. Click the Liferay DXP Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)).
2. Select the _Control Panel_ tab and, under Configuration, click _System Settings_.
3. In the Content and Data section, click _Pages_.
4. Under the desired Scope (System, Virtual Instance, or Site), click _Google PageSpeed Insights_.
5.  Check or uncheck the _Enable Google PageSpeed Insights_ box to enable or disable the Page Audit tool.

    ![Configure the Page Audit tool setting at System, Virtual Instance, or Site scope](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages/images/03.png)
6. Click _Update_.

To disable the Page Audit tool globally, uncheck the Enable Google PageSpeed Insights option at the System scope.

### [Accessing the Page Audit Tool and Running Page Audits](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#accessing-the-page-audit-tool-and-running-page-audits) <a href="#accessing-the-page-audit-tool-and-running-page-audits" id="accessing-the-page-audit-tool-and-running-page-audits"></a>

1.  Access the Page you want to audit and click the _Page Audit_ (![Page Audit](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png)) button in the Applications bar.

    ![Click the Page Audit button to start a new Page audit.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages/images/05.png)

    NOTE

    If you have not configured Page Audit yet, the Page Audit panel prompts you to complete the configuration. In this case, click _Configure_ and [complete the configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#configuring-the-page-audit-tool).
2. The Page Audit panel shows the Launch Page Audit button.
3. Click _Launch Page Audit_ to connect your Page to Google PageSpeed Insights and start the audit process.

After running a test, you can always review the last audit results by opening the Page and clicking the Page Audit (![Page Audit](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png)) button.

### [Analyzing SEO and Accessibility Issues in the Page Audit Tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#analyzing-seo-and-accessibility-issues-in-the-page-audit-tool) <a href="#analyzing-seo-and-accessibility-issues-in-the-page-audit-tool" id="analyzing-seo-and-accessibility-issues-in-the-page-audit-tool"></a>

After running an audit, the Page Audit tool shows the Page issues, according to the Google PageSpeed Insights rules. The number next to the test name shows the total number of errors for that particular test (or `+100` when there are more than 100 errors in a test.)

When you click on an issue, you see three sections:

**Description (C):** Provides general information about the selected audit test.

**Tips (D):** Includes more specific information about the error source and how to fix or mitigate the error in Liferay DXP.

**Failing Elements (E):** Lists the Page elements that didn’t pass the test using the PageSpeed Insight rules.

![The Page Audit tool GUI shows the list of issues for your Page with actionable information to fix or mitigate the issue.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages/images/04.png)

Consider this information when using the Page Audit tool:

* If you have run previous audit tests in the Page, the Page Audit tool shows the execution time and test results for the last test. To update the Page Audit results, run a new test by clicking on the Relaunch (![relaunch](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore2.png)) icon (A).
* The Page Audit tool analyzes your Page for SEO and accessibility issues at a given time. If your Page or Page’s content changes (for example, when you show dynamic content), your test results may vary.
* Page Audit shows results for the [Default Experience](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/creating-and-managing-experiences) only.
*   If your Page is localized, you can run a Page Audit test for each language. Before running the test, click the _flag_ button in the Page Audit tool and choose the language.

    ![You can change the Page language you want to audit using the flag button in the Page Audit tool.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages/images/06.gif)
* If you’re publishing your Pages using [Remote Live Staging](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging) and your Staging environment is not connected to the internet, you can use the Page Audit tool in the Live environment, but you can only fix the issues in the Staging environment.

### [Troubleshooting Page Audit Panel Errors](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#troubleshooting-page-audit-panel-errors) <a href="#troubleshooting-page-audit-panel-errors" id="troubleshooting-page-audit-panel-errors"></a>

To avoid errors when auditing your Pages, ensure that you have [configured a valid Google PageSpeed Insights API Key](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#configuring-the-page-audit-tool) and the Page you want to audit is accessible from to the internet.

#### [Common Errors](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages#common-errors) <a href="#common-errors" id="common-errors"></a>

| Error                        | Description                                                                                                                                                                                                                                                                                                                              |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The API key is invalid       | <p>The Google PageSpeed Insights API key is invalid. Click the <em>Set API Key</em> button on the error message to configure a valid API Key.<br>To obtain your API key, see <a href="https://developers.google.com/speed/docs/insights/v5/get-started">Get Started with the PageSpeed Insights API</a> in the Google documentation.</p> |
| This Page cannot be audited  | The Page you are trying to audit is not publicly accessible.                                                                                                                                                                                                                                                                             |
| An unexpected error occurred | This is a generic error. Verify that the Page Audit tool is configured correctly and that the Page you want to audit is accessible from the internet.                                                                                                                                                                                    |
