# Importing Knowledge Base Articles

You can draft Knowledge Base articles as [Markdown](https://www.markdownguide.org/) files and import them to the portal. To do this, you must ensure each Markdown file is formatted properly and packaged in a ZIP archive. Imported articles are converted to HTML automatically and appear in the Knowledge Base UI.

This way you can use external tools to draft, host, and collaborate on articles before publishing them to the portal.

NOTE

You must have the _Knowledge Base > Knowledge Base: Import Articles_ permission to import Markdown articles.

### Preparing Markdown Files for Import <a href="#preparing-markdown-files-for-import" id="preparing-markdown-files-for-import"></a>

Knowledge Base supports the [Multi-Markdown](https://fletcherpenney.net/multimarkdown) dialect, which extends basic Markdown with features like table formatting, image captions, and footnotes. As a language, Markdown is readable, intuitive, and versatile. Using services like GitHub, you and others can review and collaborate on documentation.

#### File Requirements <a href="#file-requirements" id="file-requirements"></a>

To import Markdown files, they must

* Be packaged in a ZIP file.
* Use the `.md` or `.markdown` file extensions.
* Begin with an H1 (e.g., `# Some Heading`) to set the article’s title.
* Include a unique ID after the H1 (e.g., `# Some Heading [](id=some-heading)`) to set the article’s friendly URL.

For example, an article named `modern-pentathlon.md` could include this content:

```markdown
# Modern Pentathlon [](id=modern-pentathlon)

The modern pentathlon is a competition across five different sport disciplines.

Each athlete must compete in fencing, shooting, swimming, horseback riding, and running.
```

Each ZIP file can include any number of new or modified files.

#### Optional Configurations <a href="#optional-configurations" id="optional-configurations"></a>

You can determine article order when imported by adding numerical prefixes to file names (e.g., `00-foo.md`, `01-bar.md`). Additionally, you can add the `-intro` suffix to files with the `.markdown` extension to create parent/child hierarchies for the imported articles.

For example, consider a scenario where these files are in the same folder: `00-foo-intro.markdown`, `01-bar.markdown`, and `02-goo.markdown`. When imported, this is resulting article order and hierarchy:

```bash
└── Foo
    ├── Bar
    └── Goo
```

If your articles reference images, you can include them in your import by adding them to an `/images` folder in the root of the ZIP file. Supported attachment extensions are `.bmp`, `.gif`, `.jpeg`, `.jpg`, and `.png`.

NOTE

Image resources are only imported if they are referenced in an article. Otherwise, they are ignored.

You can view and configure Knowledge Base import settings via system setting. See [Knowledge Base System Settings](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/knowledge-base-system-settings) for more information.

#### Setting a Source URL <a href="#setting-a-source-url" id="setting-a-source-url"></a>

If you’re hosting your Markdown files on a service like [GitHub](https://github.com/), you can set a source URL that specifies the repository location. To use this feature, first enable source URLs in the [Knowledge Base System Settings](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/knowledge-base-system-settings). Then, add a `.METADATA` file to the root of the ZIP file with the base URL for the desired repository. The importer appends each file’s path in the ZIP archive to this base URL to form the complete source URL (`[base URL]/[article file path]`).

For example, consider a scenario with this base URL:

```log
https://github.com/clarity/clarity-guide/blob/master/docs/latest/en
```

If you import a ZIP archive with `folder-01/file.md`, then this is the resulting source URL:

```log
https://github.com/clarity/clarity-guide/blob/master/docs/latest/en/folder-01/file.md
```

### Importing Markdown Files <a href="#importing-markdown-files" id="importing-markdown-files"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)) and go to _Content and Data_ → _Knowledge Base_.
2.  In the Articles and Folders (![Articles and Folders](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)) section, go to the desired folder, click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select _Import_.

    ![Go to the desired folder, click Add, and select Import.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/importing-knowledge-base-articles/images/01.png)

    Alternatively, click the _Actions_ button (![Action Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired folder and select _Import_.

    ![Click the Actions button for the desired folder and select Import.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/importing-knowledge-base-articles/images/02.png)
3.  Click _Choose File_ and select the desired ZIP file.

    ![Upload your ZIP file.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/importing-knowledge-base-articles/images/03.png)
4. Enable/Disable _Apply numerical prefixes of article files as priorities_. This determines whether Knowledge Base uses each file’s numerical prefixes to order them.
5. Click _Save_.

Once saved, the importer converts each file’s Markdown to HTML and the resulting articles are published.

IMPORTANT

Imported articles are independent of the workflow settings. This means that imported articles are approved automatically.

### Understanding the Import Process <a href="#understanding-the-import-process" id="understanding-the-import-process"></a>

When initiated, the Markdown importer checks whether each file’s H1 and ID match any existing Knowledge Base articles. If a match is found, the importer replaces the existing article’s content. If no match is found, a new article is created.

The importer only creates and updates articles. It does not delete any existing articles. You must manually delete articles.

TIP

You can update a file’s numerical prefix to update its order in the portal (e.g., changing `00-file.md` to `01-file.md`).

Updating an article’s header ID creates a new article with a new friendly URL and does not update or remove the existing article.
