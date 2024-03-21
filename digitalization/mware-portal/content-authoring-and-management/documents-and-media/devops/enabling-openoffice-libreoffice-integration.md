# Enabling OpenOffice / LibreOffice Integration

OpenOffice and LibreOffice are open source productivity suites that you can integrate with the portal. By default the portal includes PDFBox, which generates automatic previews for limited file types. However, you can integrate either of these suites for additional functionality, such as document conversion. The desired service must be installed on the server before it the portal integration can be enabled.

IMPORTANT

While both OpenOffice and LibreOffice are capable of integration, some of their features may not be compatible with all portal products. View the portal’s Integrated Technologies Compatibility Matrix for a list of supported technologies.

Once installed, follow these steps to enable the portal’s OpenOffice/LibreOffice integration via the Control Panel:

1.  Open the _Global Menu_, and go to _Control Panel_ → _System Settings_ → _Connectors_ → _OpenOffice Integration_.

    NOTE

    Alternatively, you can adjust these settings by deploying a `com.liferay.document.library.document.conversion.internal.configuration.OpenOfficeConfiguration.config` file to your `[Liferay Home]/osgi/configs` folder.

    ![Go to OpenOffice Integration under Connectors in System Settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/enabling-openoffice-libreoffice-integration/images/01.png)
2. Check the _Server Enabled_ box.
3.  Enter the desired _Server Host_ and _Server Port_.

    IMPORTANT

    It is recommended that you have OpenOffice on the same machine. Using a remote host for the instance is not fully supported and can lead to unexpected issues.
4. Click on _Update_.

You can now use OpenOffice/LibreOffice compatible features, which include converting documents, viewing supported document types in your browser, and generating document thumbnails and previews.

\
