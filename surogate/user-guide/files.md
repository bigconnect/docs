# Files

Knowledge base, hallucination-free chat, and file management are the three pillars of Surogate. Surogate's file management allows you to upload files individually or in bulk. You can then link an uploaded file to multiple target knowledge bases. This guide showcases some basic usages of the file management feature.

### Create folder[​](https://ragflow.io/docs/dev/manage_files#create-folder) <a href="#create-folder" id="create-folder"></a>

Surogate's file management allows you to establish your file system with nested folder structures. To create a folder in the root directory of Surogate:

<figure><img src="../../.gitbook/assets/3a37a5f4-43a6-426d-a62a-e5cd2ff7a533.jpg" alt=""><figcaption></figcaption></figure>

Each knowledge base in Surogate has a corresponding folder under the **root/.knowledgebase** directory. You are not allowed to create a subfolder within it.

### Upload file[​](https://ragflow.io/docs/dev/manage_files#upload-file) <a href="#upload-file" id="upload-file"></a>

Surogate's file management supports file uploads from your local machine, allowing both individual and bulk uploads:

<figure><img src="../../.gitbook/assets/def0db55-824c-4236-b809-a98d8c8674e3.jpg" alt=""><figcaption></figcaption></figure>

### Preview file[​](https://ragflow.io/docs/dev/manage_files#preview-file) <a href="#preview-file" id="preview-file"></a>

Surogate's file management supports previewing files in the following formats:

* Documents (PDF, DOCS)
* Tables (XLSX)
* Pictures (JPEG, JPG, PNG, TIF, GIF)

<figure><img src="../../.gitbook/assets/2e931362-8bbf-482c-ac86-b68b09d331bc.jpg" alt=""><figcaption></figcaption></figure>

### Link file to knowledge base[​](https://ragflow.io/docs/dev/manage_files#link-file-to-knowledge-bases) <a href="#link-file-to-knowledge-bases" id="link-file-to-knowledge-bases"></a>

Surogate's file management allows you to _link_ an uploaded file to multiple knowledge bases, creating a file reference in each target knowledge base. Therefore, deleting a file in your file management will AUTOMATICALLY REMOVE all related file references across the knowledge bases.

<figure><img src="../../.gitbook/assets/6c508803-fb1f-435d-b688-683066fd7fff.jpg" alt=""><figcaption></figcaption></figure>

