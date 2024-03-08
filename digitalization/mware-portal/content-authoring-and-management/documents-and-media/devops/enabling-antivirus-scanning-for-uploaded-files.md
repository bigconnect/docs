# Enabling Antivirus Scanning for Uploaded Files

You can automatically scan any file uploaded to Liferay for viruses. When you enable the antivirus scanner, it checks files on upload to Liferay applications, such as [Documents and Media](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media), [Message Boards](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/message-boards/user-guide/getting-started-with-message-boards), and more. If a virus is found, it’s reported for users to reject.

![The scanner detects virus-infected files on upload to Documents and Media and other Liferay applications.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/enabling-antivirus-scanning-for-uploaded-files/images/01.png)

Liferay integrates with the [ClamAV Daemon](https://www.clamav.net/documents/scanning#clamd) (Clamd). For best performance, run Clamd on a separate server.

Here’s how to enable the antivirus scanning:

1.  On a separate server, [configure and start Clamd](https://www.clamav.net/documents/scanning#clamd).

    IMPORTANT

    Load your ClamAV database before starting Clamd.
2.  Enable antivirus for your [File Store](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage) (Document Library) by setting the following [portal property](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) or [Docker environment variable](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/installing-liferay/using-liferay-docker-images/configuring-containers).

    Portal property:

    ```properties
    dl.store.antivirus.enabled=true
    ```

    Docker environment variable:

    ```properties
    -e LIFERAY_DL_PERIOD_STORE_PERIOD_ANTIVIRUS_PERIOD_ENABLED=true
    ```
3. Start your Liferay server.
4. Go to _Control Panel_ → _System Settings_ and select _Antivirus_ in the Security category.
5.  Select _Antivirus Clamd Scanner_ in the menu.

    ![Antivirus Clamd Scanner configuration](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/enabling-antivirus-scanning-for-uploaded-files/images/02.png)
6. Enter the Clamd server’s hostname or IP address, port, and a connection timeout time (milliseconds).
7. Click _Save_.

Now files are scanned on upload to Liferay applications. If a virus is detected in a file you’re uploading, the scanner reports the infected file, and you should decline saving the file.

![Here's the virus detection message.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/enabling-antivirus-scanning-for-uploaded-files/images/03.png)

IMPORTANT

Never save a virus-infected file. Reject the file by canceling the current operation.

\
