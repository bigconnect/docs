# Changing Your Site’s Appearance

The portal has many ways to customize your site’s appearance. Your site’s favicon and theme are two of the first elements you may want to change. Themes set a default look and feel for your site, while the favicon is an icon that identifies your site in the user’s browser tab.

### Changing Your Site’s Favicon <a href="#changing-your-sites-favicon" id="changing-your-sites-favicon"></a>

Follow these steps:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Site Builder_ → _Pages_.
2.  Click _Actions_ (![Action button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar and select _Configuration_.

    ![Open the Pages application, click Actions in the Application Bar, and select Configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/changing-your-sites-appearance/images/01.png)
3.  Under Favicon, click _Select Favicon_ (![Select Favicon button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-switch.png)).

    ![Click the Select Favicon button.](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/changing-your-sites-appearance/images/02.png)
4. Select or upload an image.
5. Click _Save_.

Your site uses the chosen image for its favicon.

### Changing Your Site’s Theme <a href="#changing-your-sites-theme" id="changing-your-sites-theme"></a>

By default, portal instances use the Classic theme, but you can deploy and use custom themes.

#### Deploy a New Theme <a href="#deploy-a-new-theme" id="deploy-a-new-theme"></a>

1.  Download and unzip the Acme Sample Blue Theme WAR file:

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/getting-started/changing-your-sites-appearance/resources/liferay-5b2v.zip -O
    ```

    ```bash
    unzip liferay-5b2v.zip
    ```
2.  Go to the folder where the .war file is placed and deploy it:

    ```bash
    docker cp liferay-5b2v-theme.war [docker-container-id]:[path-to-deploy-folder]
    ```

NOTE

To replace `[docker-container-id]`, use `docker ps` to find the portal container’s ID. The `[path-to-deploy-folder]` is `opt/liferay/deploy`.

This loads the sample theme into your portal instance. You can check your console for the following message to confirm the theme successfully deployed:

```log
2023-12-21 16:01:20.836 INFO  [fileinstall-directory-watcher][BundleStartStopLogger:68] STARTED liferay-5b2v-theme_1.0.0 [1617]
```

#### Change Your Site’s Theme <a href="#change-your-sites-theme" id="change-your-sites-theme"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Site Builder_ → _Pages_.
2.  Click _Actions_ (![Action button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar and select _Configuration_.

    ![Open the Pages screen, click Actions in the Application Bar, and select Configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/changing-your-sites-appearance/images/01.png)
3.  Under Theme, click _Change Current Theme_:

    ![Click Change Current Theme to select a new theme.](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/changing-your-sites-appearance/images/03.png)
4. Select the sample theme: _Acme Sample Blue Theme_.
5. Click _Save_.

Navigate to the Home page and confirm the background color is now blue.

![Confirm the background color is now blue.](https://resources.learn.liferay.com/images/dxp/latest/en/getting-started/changing-your-sites-appearance/images/04.png)

\
