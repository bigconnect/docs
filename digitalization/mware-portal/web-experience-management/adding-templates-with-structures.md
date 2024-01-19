# Adding Templates with Structures

To better understand templates, now you’ll create a structure and an associated template. First create the structure:

1. Go to _Content & Data_ → _Web Content_ from Site Administration page and open the _Structures_ tab.
2. Click the _Add_ button (![Add Structure](https://liferay-support.zendesk.com/hc/article\_attachments/360030159851/icon-add.png)).
3.  Name the structure _News Article_ and add the following fields:

    | Field Type | Field Label | Name        |
    | ---------- | ----------- | ----------- |
    | Text       |  Title      |  `title`    |
    | Text Box   |  Abstract   |  `abstract` |
    | Image      |  Image      |  `image`    |
    | HTML       |  Body       |  `body`     |
4. Click _Save_.

Now create the template and connect it to the structure.

1. From the _Web Content_ page, go to the _Templates_ tab.
2. Click the _Add_ button (![Add Template](https://liferay-support.zendesk.com/hc/article\_attachments/360030159851/icon-add.png)).
3. Enter the name _News Article_.
4. Open _Details_ and make sure FreeMarker is selected as the script language (it’s the default).
5. Click _Select_ under _Structure_.
6. Choose the _News Article_ structure.
7.  In the _Script_ area, find the _Fields_ label on the left and click on _Title_, _Abstract_, _Image_ and _Body_ into the editor area. It should look like this:

    ```markup
    ${title.getData()}
    ${abstract.getData()}
    <#if image.getData()?? && image.getData() != ""> <img alt="${image.getAttribute("alt")}" data-fileentryid="${image.getAttribute("fileEntryId")}" src="${image.getData()}" /> </#if>
    ${body.getData()}
    ```
8.  Next, add heading and `<p>` tags and align the image to center to style your elements like this:

    ```markup
    <h1>${title.getData()}</h1>
    <p>${abstract.getData()}</p>
    <#if image.getData()?? && image.getData() != ""> <img alt="${image.getAttribute("alt")}" data-fileentryid="${image.getAttribute("fileEntryId")}" src="${image.getData()}" align="center" /> </#if>
    <p>${body.getData()}</p>
    ```
9. Click _Save_.

To finish it up, add some content:

1. Go to the _Web Content_ tab.
2. Click on the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030159851/icon-add.png)) and select _News Article_.
3. Insert some content and publish!

![Figure 1: The Lunar Resort News Article is shaping up!](https://liferay-support.zendesk.com/hc/article\_attachments/360030190752/web-content-structures-templates-completed.png)

Awesome! You created your own web content template!
