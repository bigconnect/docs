---
description: Custom HTML embed component
---

# Embed

The Embed component is used to embed content into your app. This content is provided by an external application or other sources of interactive content. Types of content can range from SVGs, PDFs, Maps, to HTML web pages.

Another use of the embed component is for Global Styling.

> ### 👍Embedded app
>
> Apps can also be embedded in other webpages. Read more: Embedded app.

#### Security considerations

You are unable to use `<script>` tags inside an Embed component. If you require more advanced functionality inside your app, consider building a custom component plugin.

When using an embed to pull code from an external source, make sure that the source is trusted. Using embedded code can introduce risks, for example Clickjacking is one kind of common iframe attack where hackers embed an invisible iframe into your document (or embed your document into their own malicious website) and use it to capture users' interactions. This is a common way to mislead users or steal sensitive data.
