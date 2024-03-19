# Creating Knowledge Base Articles

Knowledge Base provides tools for creating and managing a repository of articles. You can create articles using the application’s WYSIWYG editor or by importing them as Markdown files (`.markdown`, `.md`) in a ZIP archive.

![Create and manage a repository of articles.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/creating-knowledge-base-articles/images/01.png)

### [Authoring Articles in the Editor](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/creating-knowledge-base-articles#authoring-articles-in-the-editor) <a href="#authoring-articles-in-the-editor" id="authoring-articles-in-the-editor"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)) and go to _Content and Data_ → _Knowledge Base_.
2.  In the Folders and Articles section (![Folders and Articles](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)), click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select _Basic Article_.

    If you created an [article template](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/knowledge-base-templates), you can select it instead.

    ![Click Add and select the desired article template.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/creating-knowledge-base-articles/images/02.png)
3. Enter an article title.
4.  Enter content for the article. To view or edit the article’s HTML source, click the _Source_ button (![Source](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-code.png)) in the editor.

    ![Enter content for the article.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/creating-knowledge-base-articles/images/03.png)
5. (Optional) Add attachments to the article.
6. (Optional) Select a [display page template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates) for the article. This configuration overrides the default display page template for Knowledge Base articles.
7. (Optional) Add [tags](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/tagging-content-and-managing-tags) or [categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content) to the article to improve visibility and organization.
8. (Optional) Set expiration and review dates. By default, articles are set never to expire or require review. See [Managing the Knowledge Base](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base) for more information.
9. (Optional) Relate assets to the article.
10. Set the article’s friendly URL. By default, the URL matches the title field, but you can change it manually.

    IMPORTANT

    After saving or publishing an article, you cannot change its friendly URL.
11. (Optional) Configure the article’s permissions. By default, the View permission is assigned to the Guest role, meaning anyone can view the article.
12. Click _Publish_ to submit the article for publication or _Save as Draft_ to work on it later.

    NOTE

    With the LPS-188058 [release feature flag](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags) enabled, you can schedule articles for publishing at a specific date and time.

Once saved, the article appears in the Folders and Articles section. If you’ve enabled workflow for Knowledge Base, the article must go through the approval process before being published.

### [Importing Markdown Files](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/creating-knowledge-base-articles#importing-markdown-files) <a href="#importing-markdown-files" id="importing-markdown-files"></a>

If desired, you can import Knowledge Base articles as Markdown files (`.markdown`, `.md`). To do this, you must ensure each Markdown file is properly formatted and packaged in a ZIP archive. Imported articles are automatically converted to HTML and appear in the Articles and Folders section (![Folders and Articles](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)). See [Importing Knowledge Base Articles](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/importing-knowledge-base-articles) for detailed instructions.

\
