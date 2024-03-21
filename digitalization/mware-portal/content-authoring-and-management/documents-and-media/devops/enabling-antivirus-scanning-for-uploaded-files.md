# Enabling Antivirus Scanning for Uploaded Files

You can automatically scan any file uploaded to the portal for viruses. When you enable the antivirus scanner, it checks files on upload to the portal applications, such as Documents and Media, Message Boards, and more. If a virus is found, it’s reported for users to reject.

![The scanner detects virus-infected files on upload to Documents and Media and other applications.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/enabling-antivirus-scanning-for-uploaded-files/images/01.png)

MWARE Portal integrates with the ClamAV Daemon (Clamd). For best performance, run Clamd on a separate server.

Here’s how to enable the antivirus scanning:

1.  On a separate server, configure and start Clamd.

    IMPORTANT

    Load your ClamAV database before starting Clamd.
2.  Enable antivirus for your File Store (Document Library) by setting the following portal property or Docker environment variable.

    Portal property:

    ```properties
    dl.store.antivirus.enabled=true
    ```

    Docker environment variable:

    ```properties
    -e LIFERAY_DL_PERIOD_STORE_PERIOD_ANTIVIRUS_PERIOD_ENABLED=true
    ```
3. Start your portal server.
4. Go to _Control Panel_ → _System Settings_ and select _Antivirus_ in the Security category.
5.  Select _Antivirus Clamd Scanner_ in the menu.

    ![Antivirus Clamd Scanner configuration](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/enabling-antivirus-scanning-for-uploaded-files/images/02.png)
6. Enter the Clamd server’s hostname or IP address, port, and a connection timeout time (milliseconds).
7. Click _Save_.

Now files are scanned on upload to the portal applications. If a virus is detected in a file you’re uploading, the scanner reports the infected file, and you should decline saving the file.

![Here's the virus detection message.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/enabling-antivirus-scanning-for-uploaded-files/images/03.png)

IMPORTANT

Never save a virus-infected file. Reject the file by canceling the current operation.

\
