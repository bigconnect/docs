# Enabling DocuSign Digital Signatures

You can now integrate DocuSign digital signatures into your portal documents. DocuSign is a service that manages documents to be signed electronically. With this integration, you can manage and collect signatures on your documents.

Before you enable digital signatures in the portal, make sure you’ve generated and retrieved your User ID key, API Account key, Account Base URI, Integration key, and RSA Private key. Instructions for doing this can be found on DocuSign’s website.

### Enabling Digital Signatures <a href="#enabling-digital-signatures" id="enabling-digital-signatures"></a>

1. Open the Global menu in the top right corner. ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)
2.  Click _Control Panel_ → _Instance Settings_ → _Digital Signature_.

    ![The Digital Signature link appears in either Instance Settings or Site Settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/enabling-docusign-digital-signatures/images/01.png)
3.  Switch the toggle to _Enabled_.

    ![Switch the toggle.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/enabling-docusign-digital-signatures/images/02.png)
4.  Choose a _Site Settings Strategy_ (see below).

    ![Your Site Settings Strategy defines the scope of your digital signatures.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/enabling-docusign-digital-signatures/images/03.png)
5. Click _Save_.

You have three options for your Site settings strategy:

**Always Inherit:** All sites are linked to these settings.

**Always Override:** Every site must provide its own configuration.

**Inherit or Override:** Can be defined in both Instance Settings and Site Settings. If defined in both, Site settings take precedence over Instance settings.

You must now add your digital keys from DocuSign at the appropriate scope in the portal.

### Adding Digital Keys <a href="#adding-digital-keys" id="adding-digital-keys"></a>

Depending on what you chose for your Site Settings strategy, add your digital keys at the appropriate scope:

* If you chose Always Inherit, add the keys in Instance Settings.
* If you chose Always Override, add the keys in Site Settings.
* If you chose Inherit or Override, add the keys in either place

1. Navigate to _Control Panel_ → _Instance Settings_ → _Digital Signature_ or for Site Settings, the _Site Menu_ → _Configuration_ → _Site Settings_ → _Digital Signature_.
2. If it’s not switched already, switch the toggle to _Enabled_.
3. Enter the User ID, API Account ID, Account’s Base URI, Integration Key, and the RSA Private Key you previously retrieved from the DocuSign website.
4. Click _Save_ to enable digital signatures.

### Collecting Document Signatures in Documents and Media <a href="#collecting-document-signatures-in-documents-and-media" id="collecting-document-signatures-in-documents-and-media"></a>

1.  Find the document where you want to collect the digital signatures and click _Actions_ → _Collect Digital Signature_.

    ![You can see the Actions and Collect Digital Signatures buttons.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/enabling-docusign-digital-signatures/images/04.png)
2.  For multiple documents, select the documents where you want to collect the signatures and click _Collect Digital Signature_ (![Collect Digital Signature](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-digital-signature.png)).

    ![You can see multiple selected files and the Collect Digital Signature button.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/enabling-docusign-digital-signatures/images/05.png)
3.  Fill in the _Envelope_’s information and click _Send_.

    ![Fill in the envelope's information.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/enabling-docusign-digital-signatures/images/06.png)
4. When the envelope is sent, Recipients must go through DocuSign’s process to sign the document.

### Tracking an Envelope’s Status <a href="#tracking-an-envelopes-status" id="tracking-an-envelopes-status"></a>

DocuSign uses the term _envelope_ to denote a document or collection of documents to be signed. Once sent, you can track your envelope status from within the portal.

TIP

You can check the different status labels on the DocuSign website.

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)) → _Content & Data_ → _Digital Signature_ to see a list of created envelopes.
2. You can also create an envelope directly from this screen using the ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png). You’re shown the envelope’s screen to enter its information.
3.  Use _Filter and Order_ or type keywords in the Search bar to filter and sort the list of available envelopes.

    ![Organize documents through Filter and Order, or the Search Bar.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/enabling-docusign-digital-signatures/images/07.png)

### Downloading Documents <a href="#downloading-documents" id="downloading-documents"></a>

1.  Click the envelope’s name to see its details. You can download the document by clicking the _Download_ button or by clicking the _Actions Button_ ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) from the Digital Signature screen.

    IMPORTANT

    Clicking _Download_ fetches the document from DocuSign not from the portal, since the portal does not store signed documents. Whether a document is partially or fully signed, the download button retrieves the latest state.

    ![You can view the details of your envelopes from inside the portal.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/enabling-docusign-digital-signatures/images/08.png)

WARNING

Disabling the Digital Signature Configuration removes all reference to DocuSign, including the Digital Signature module in the Control Panel. If you disable the feature in the portal, all envelopes are still visible in DocuSign. If you re-enable digital signatures, the list appears again.

\
