# Creating and Editing Documents with Google Drive

You can use Google Docs™ to create and edit text documents, spreadsheets, or presentations for storage in the Docs and Media library. When you finish your Google Docs™ editing session, your changes are automatically saved in the Documents and Media Library.

### Authentication <a href="#authentication" id="authentication"></a>

The first time you create or edit a Documents and Media file via Google Docs™, you must authenticate to your Google account. This links Google Drive™ to your portal account, so you only need to do this once.

To unlink your account,

1. Click on _your profile_ → _Account Settings_.
2. On the _General_ tab, click _Apps_.
3. Click _Revoke_ next to Google Drive™.

![You can unlink your Google account from the portal.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/creating-documents/creating-and-editing-documents-with-google-drive/images/01.png)

Access to Google Drive™ has been unlinked.

### Creating Files <a href="#creating-files" id="creating-files"></a>

Follow these steps to create a new Documents and Media file via Google Docs™:

1. Open the _Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)), click your site’s name, and navigate to _Content & Data_ → _Documents and Media_.
2. Click the _Add_ icon (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select the type of Google document to add to the Document Library.
   * Google Docs™
   * Google Slides™
   * Google Sheets™
3.  When you select one of these options, the portal creates a temporary Documents and Media file and links it to a new Google file. Your browser then redirects you to that Google file so you can create its content. Note that some actions on the temporary Documents and Media file can affect its linked Google file. For more information, see multiple editing sessions.

    ![Select the type of Google document you want to create.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/creating-documents/creating-and-editing-documents-with-google-drive/images/02.png)
4. Use the Google Docs™ editor to create your document’s content. All Google Docs™ features are available except for sharing.
5.  Save or discard your changes by clicking one of these toolbar buttons in the Google Docs™ editor:

    **Save and Return to Portal:** Saves your document as a new file in the Documents and Media Library, deletes the Google file, and returns you to the portal. The saved file’s format depends on the type of Google document you selected in step two above:

    * Google Docs™: Microsoft Word (`.docx`)
    * Google Slides™: Microsoft PowerPoint (`.pptx`)
    * Google Sheets™: Microsoft Excel (`.xlsx`)

    **Discard Changes:** Returns you to the portal without saving your file in the Documents and Media Library or Google Docs™.

    Note that it’s also possible to close the Google Docs™ window without clicking either button. In this case, the editing session remains open even though the window that displayed it is closed. For more information, see the section below on [multiple editing sessions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/creating-documents/creating-and-editing-documents-with-google-drive#multiple-editing-sessions).

    ![Save or discard your changes by using the toolbar in the editor.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/creating-documents/creating-and-editing-documents-with-google-drive/images/03.png)

### Editing Files <a href="#editing-files" id="editing-files"></a>

You can use Google Docs™ to edit the following types of Documents and Media files:

* Text files (`.docx`, `.html`, `.txt`, `.rtf`, `.odt`)
* Presentation files (`.pptx`, `.odp`)
* Spreadsheet files (`.xlsx`, `.ods`, `.csv`, `.tsv`)
* PDF files

**Note:** Google Docs™ doesn’t support older, non-XML-based Microsoft Office file types (`.doc`, `.ppt`, `.xls`).

Follow these steps to edit a Documents and Media file in Google Docs™:

1. Navigate to the file in the Documents and Media Library.
2.  Click the file’s Actions icon (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Edit in Google Docs_. This automatically checks out the file, transfers its content to a new Google Docs™ file, and redirects you to that Google Docs™ file.

    ![Select Edit in Google Docs from the file's Actions menu.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/creating-documents/creating-and-editing-documents-with-google-drive/images/04.png)
3. Edit the file in Google Docs™. The editing process is exactly the same as described above for creating files.

### Multiple Editing Sessions <a href="#multiple-editing-sessions" id="multiple-editing-sessions"></a>

When you create or edit a Documents and Media file in Google Docs™, you can save or discard your changes by clicking _Save and Return to Portal_ or _Discard Changes_, respectively. If you instead close the window without clicking either, the editing session still exists. You can access it via the original file in Documents and Media. If the file didn’t exist before (e.g., you were creating a new file), it appears in Documents and Media as a temporary file.

When an editing session already exists for a Documents and Media file, the following actions are available via that file’s Actions icon (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)):

**Edit in Google Docs:** Resume editing the file in Google Docs™.

**Check in:** Saves the Google file (including any changes) to the Documents and Media file, then deletes the Google file. This is equivalent to clicking _Save and Return to Portal_ in a Google Docs™ editing window.

**Cancel Checkout:** Deletes the Google file, discarding any changes. This is equivalent to clicking _Discard Changes_ in a Google Docs™ editing window.

\
