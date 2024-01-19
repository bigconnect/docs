# Editing Images

Editing and re-uploading images when you only need to apply simple edits is tedious. Docs & Media contains a simple built-in image editor for exactly this reason. To access the image editor, locate the image you want to edit. Click the Actions icon (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029924132/icon-actions.png)) and select _Edit With Image Editor_.

You can also access the image editor when selecting an image to insert in content (i.e., via an item selector). Anywhere an image is, you can edit it. For example, you can access the image editor via item selector preview windows in blog entries and web content articles. To do this, click the pencil icon (![Pencil](https://liferay-support.zendesk.com/hc/article\_attachments/360029908771/icon-edit-pencil.png)) in the bottom-right corner of the preview window.

![Figure 1: You can access the image editor through the Documents and Media repository.](https://liferay-support.zendesk.com/hc/article\_attachments/360029908691/image-editor-docs-and-media.png)

![Figure 2: You can also access the image editor through the item selector preview window.](https://liferay-support.zendesk.com/hc/article\_attachments/360029924152/image-editor-preview-window.png)

If you edit and save the image via the Documents and Media repository, the file version is incremented a minor version (e.g., from version 1.0 to version 1.1). You can view the image’s version history (and previous versions) by clicking the image, clicking its _Info_ button (![i](https://liferay-support.zendesk.com/hc/article\_attachments/360029908751/icon-information-dm.png)), and then selecting the _Versions_ tab. In contrast, if you edit and save an image via an item selector, a copy of the image is created and saved to the Document Library.

We designed the image editor with quick editing in mind. It offers a minimal, user-friendly UI. The main toolbar consists of three buttons, each of which contains a subset of options:

![Figure 3: The image editors UI is clear and to the point, offering only what you need.](https://liferay-support.zendesk.com/hc/article\_attachments/360029908731/image-editor-tools.png)

**Transform** (![Transform](https://liferay-support.zendesk.com/hc/article\_attachments/360029924092/icon-transform.png))

* **Rotate**: Rotate the image to the left or right, in 90 degree increments.
* **Resize**: Resize the image. If the lock is closed, the aspect ratio is locked and changing width or height automatically adjusts the other dimension to maintain the aspect ratio. When the lock is opened, the width and height can be changed individually, letting the aspect ratio change (this isn’t recommended because the image can become distorted).
* **Crop**: Crop the image.

**Adjustment** (![Adjustment](https://liferay-support.zendesk.com/hc/article\_attachments/360029908711/icon-adjustment.png))

* **Saturation**: Adjust the color saturation. The default value is 50. Values range from 0 (completely desaturated) to 100 (completely saturated).
* **Contrast**: Adjust the contrast. The default value is 50. Values range from 0 (no contrast) to 100 (full contrast).
* **Brightness**: Adjust the brightness. The default value is 50. Values range from 0 (completely black) to 100 (completely white).

**Filter** (![Wand](https://liferay-support.zendesk.com/hc/article\_attachments/360029908671/icon-wand.png)): Apply a filter to the image.

![Figure 4: Select from a set of preset image filters.](https://liferay-support.zendesk.com/hc/article\_attachments/360029924112/image-editor-filters.png)

Upon editing the image in the editor, you can click the _Cancel_ button to cancel the changes, or the _Apply_ button to apply them. Upon applying the changes, the history bar appears. It lets you undo, redo, or reset the changes. Use the Reset button with caution; it resets the image to its original state, reverting all changes made in the editor.

![Figure 5: The history bar lets you undo, redo, and reset changes.](https://liferay-support.zendesk.com/hc/article\_attachments/360029924172/image-editor-history-bar.png)

