# Configuring the Questions App

There are two primary methods for configuring the Questions app: directly through the app configuration and through the System Settings configuration.

To configure through the application,

1.  Click on the Options icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) of the app and click _Configuration_.

    ![Click on the configuration link of the app.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/01.png)

To configure through System Settings,

1. Navigate to _Control Panel_ → _System Settings_ → _Third Party_.
2.  Click on _Questions_ under Widget Scope.

    ![Navigate to the Questions app settings in system settings.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/05.png)

### [Topic Display Style](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/questions-application/configuring-the-questions-app#topic-display-style) <a href="#topic-display-style" id="topic-display-style"></a>

Topics can be displayed as cards or as a list.

1.  Click on the Options icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) of the app and click _Configuration_.

    ![Click on the configuration link of the app.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/01.png)
2. Under General Settings, there is a Show Cards for Topic Navigation option.

If this is checked, the app will display separate cards for each question topic. If it is not checked, the app will display a list of all the questions.

![The configuration window opens with different options.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/02.png)

### [Limiting Topic Display](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/questions-application/configuring-the-questions-app#limiting-topic-display) <a href="#limiting-topic-display" id="limiting-topic-display"></a>

Questions default display behavior is to show all Topics. You can opt to only display certain topics by configuring a Root Topic. To only display specific sub-topics or questions do the following,

1.  Click on the Options icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) of the app and click _Configuration_.

    ![Click on the configuration link of the app.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/01.png)
2.  Click the _Select_ button under Root Topic ID.

    ![Select an individual topic as the root topic.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/03.png)
3. Select a topic and click _Save_. The app will now only show sub-topics and questions for the selected topic.

### [Managing App Permissions](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/questions-application/configuring-the-questions-app#managing-app-permissions) <a href="#managing-app-permissions" id="managing-app-permissions"></a>

To modify permissions for the app,

1.  Click on the Options icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) of the app and click _Permissions_.

    ![Click on the configuration link of the app.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/01.png)
2.  A new window will open.

    ![Assign permissions for different roles on this page.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/04.png)
3. Assign permissions for different Roles. Click _Save_.

### [Redirecting Guest Users to Login](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/questions-application/configuring-the-questions-app#redirecting-guest-users-to-login) <a href="#redirecting-guest-users-to-login" id="redirecting-guest-users-to-login"></a>

To enable this functionality you must configure it through System Settings,

1. Navigate to _Control Panel_ → _System Settings_ → _Third Party_.
2.  Click on _Questions_ under Widget Scope.

    ![Navigate to the Questions app settings in system settings.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/05.png)
3. Check _Enable Redirect to Login_
4. Guests user are now redirected to a login page when they try to click _Ask Question_.

NOTE

A Service Access Policy needs to be created to allow Guest users to view the app.

#### [Creating a Service Access Policy to Allow Guest Viewers](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/questions-application/configuring-the-questions-app#creating-a-service-access-policy-to-allow-guest-viewers) <a href="#creating-a-service-access-policy-to-allow-guest-viewers" id="creating-a-service-access-policy-to-allow-guest-viewers"></a>

To create a new policy,

1. Navigate to _Control Panel_ → _Service Access Policy_. Click on the _Add_ icon (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
2.  Give the policy a Name and Title. Click the _Switch to Advanced Mode_ button at the bottom. Add the following two signatures under Allowed Service Signatures,

    ```java
    com.liferay.message.boards.service.MBCategoryService#getCategoriesCount
    com.liferay.message.boards.service.MBCategoryService#getCategory
    ```

    ![Create a new policy and add the signatures.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/questions-application/configuring-the-questions-app/images/06.png)
3. Click _Save_ to save the new policy.

### [System Configuration Reference](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/questions-application/configuring-the-questions-app#system-configuration-reference) <a href="#system-configuration-reference" id="system-configuration-reference"></a>

| Setting                         | Description                                                                                                                                              |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Show Cards for Topic Navigation | If this is checked, the app will display separate cards for each question topic. If it is not checked, the app will display a list of all the questions. |
| Use Topic Names in URL          | If this is checked, each topic name will be used in a more friendly URL instead of a topic ID number.                                                    |
| Root Topic ID                   | This setting is ignored.                                                                                                                                 |
