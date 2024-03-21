# Enabling Optimization of Animated GIFs

To scale animated GIFs, Adaptive Media uses an external tool called Gifsicle. If Gifsicle isn’t installed and `image/gif` is included as a supported MIME type, Adaptive Media scales only one frame of the GIF, making a static GIF.

Optimizing animated GIFs using Adaptive Media requires three steps: installing Gifsicle on your local server where the portal is running, setting the `PATH` environment variable, and then enabling Gifsicle in _System Settings_.

1. Install Gifsicle and add it to path of the server environment.
2.  Navigate to the _Global Menu_ → _Control Panel_.

    ![Navigate to the Control Panel.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/enabling-optimization-of-animated-gifs/images/01.png)
3. Click _System Settings_.
4.  Click _Adaptive Media_.

    ![Access the Adaptive Media system settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/enabling-optimization-of-animated-gifs/images/02.png)
5. Click _Images_ in the left navigation under _System Scope_.
6.  Scroll down and and then click the box next to _Gifsicle Enabled_.

    ![Enable Gifsicle.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/enabling-optimization-of-animated-gifs/images/03.png)
7. Click _Save_ when finished.

\
