# Enable Social Media Interaction

By default, the community links option is **disabled** (hidden). As a result, the sharable Developer Portal links of the APIs for Facebook, Twitter, and Reddit will not appear in the Developer Portal by default. Instead, the administrators have to enable the social media sharing option if they wish to allow API Consumers to use this feature.

In addition, the GitHub and Slack channel URLs will not appear in the Developer Portal unless the administrators have defined these URLs via the Publisher.

## Enable sharing API link on social media <a href="#enable-sharing-api-link-on-social-media" id="enable-sharing-api-link-on-social-media"></a>

Follow the instructions below to enable API Consumers to be able to share the API link on Facebook, Twitter, and Reddit via the Developer Portal:

### Step 1 - Enable the community links option <a href="#step-1-enable-the-community-links-option" id="step-1-enable-the-community-links-option"></a>

1.  Open the `<API-M_HOME>/repository/deployment/server/webapps/devportal/src/main/webapp/site/public/theme/userTheme.json` configuration file in the WSO2 API Manager server.

    Note

    * The default configuration comes with an empty configuration object declaration. You can override the default configuration in the `userTheme.js` file.
    * For more information on the content that is in the default configuration, see [Overriding the Developer Portal theme](https://apim.docs.wso2.com/en/4.2.0/reference/customize-product/customizations/customizing-the-developer-portal/overriding-developer-portal-theme/#content-of-defaultthemejs).
2.  Enable the community links option.

    ```json
    {
        "custom": {
            "social": {
                "showSharing": {
                    "active": true
                }
            }
        }
    }
    ```

### Step 2 - Verify the changes <a href="#step-2-verify-the-changes" id="step-2-verify-the-changes"></a>

1.  Sign in to the Developer Portal.

    `https://<hostname>:9443/devportal`

    For testing purposes, you can use `https://localhost:9443/devportal` and `admin` as the username and password.
2.  Click on any published API to view its details.

    The social media sharable links and the email composer link appear along with the iframe code embedding of the API.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/community-features/devportal-default-community-links.png" alt=""><figcaption></figcaption></figure>

## Add GitHub and Slack channel URLs to an API <a href="#add-github-and-slack-channel-urls-to-an-api" id="add-github-and-slack-channel-urls-to-an-api"></a>

Follow the instructions below to enable the API related GitHub and Slack channel URLs to appear in the Developer Portal:

### Step 1 - Define your GitHub and Slack channel URLs <a href="#step-1-define-your-github-and-slack-channel-urls" id="step-1-define-your-github-and-slack-channel-urls"></a>

1.  Sign in to the Publisher.

    `https://<hostname>:9443/publisher`

    `https://localhost:9443/publisher`
2. Click on any published API to view its details.
3.  Click **Basic info**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/community-features/publisher-community-link-basic-info.png" alt=""><figcaption></figcaption></figure>
4.  Provide your GitHub and Slack channel URL in the respective input fields and click **Save**



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/community-features/publisher-slack-urls.png" alt=""><figcaption></figcaption></figure>

### Step 2 - Verify the changes <a href="#step-2-verify-the-changes_1" id="step-2-verify-the-changes_1"></a>

1.  Sign in to the Developer Portal.

    `https://<hostname>:9443/devportal`

    For testing purposes, you can use `https://localhost:9443/devportal` and `admin` as the username and password.
2.  Click on the API.

    The API's overview page appears with the GitHub and Slack channel URLs.



    Now, API Consumers can use the GitHub and Slack channel community links to engage in community activities, explore the API source, and much more.

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/community-features/devportal-community-links-with-slack-github.png" alt=""><figcaption></figcaption></figure>
