# Document Types

Document types are made of metadata fields and help users define the purpose of Document Library files. For example, a _Contract_ document type may need metadata fields for the effective date, expiration date, contract type, legal reviewer, and more. When users create Document Library files of the Contract document type, they can then populate those metadata fields. Document types also help you integrate files with other features like search and workflow. Search works on file metadata so users can find files faster. You can also apply workflows to specific document types. And you can more cleanly organize document libraries by designating folders to hold particular document types exclusively.

### Managing Document Types <a href="#managing-document-types" id="managing-document-types"></a>

To see the available document types, open the _Menu_ (![Product Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360040164131/icon-menu.png)), expand your site’s menu, and navigate to _Content & Data_ → _Documents and Media_. Then click the _Document Types_ tab. A searchable table lists any existing document types. The following actions are available for each document type via its Actions button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360040158932/icon-actions.png)):

**Edit**: Edit the document type.

**Permissions**: Set the document type’s permissions.

**Delete**: Delete the document type. Note that document types don’t support the Recycle Bin. Once you delete a document type, it’s gone forever.

![Figure 1: The Document Types management window lets you view existing document types and create new ones.](https://liferay-support.zendesk.com/hc/article\_attachments/360040158892/dm-doc-types-list.png)

### Creating Document Types <a href="#creating-document-types" id="creating-document-types"></a>

Follow these steps to create a document type:

1. From the _Menu_ (![Product Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360040164131/icon-menu.png)), expand your site’s menu and navigate to _Content & Data_ → _Documents and Media_. Then click the _Document Types_ tab.
2. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360040158912/icon-add.png)). The _New Document Type_ form appears.
3.  Give your document type a name and a description.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360040164151/dm-doc-types-new.png" alt="Figure 2: Create your new document type." data-size="original">

    Figure 2: Create your new document type.
4.  Define the metadata to use with your document type. You do this via these sections in the form:\


    **Main Metadata Fields:** These are tied directly to the document type. They can be created only via the form and can’t be used with other document types. You create and edit these metadata fields in the form the same way that you do when creating metadata sets.\


    **Additional Metadata Fields:** Select a metadata set to associate with the document type. Each document type must be associated with one or more metadata set. To differentiate document types that use the same metadata sets, define different main metadata fields.\

5. Define your document type’s permissions via the form’s _Permissions_ section. By default, anyone can view the document type, including site guests. You can restrict its view, update, delete, and permissions configuration to site members or the document type’s owner.
6. Click _Save_ when you’re finished specifying your new document type.

Your document type is now available when adding a document via the Documents and Media’s _Add_ menu. When users create new files of the document type, they’re presented with metadata fields to describe the document.
