# Splitters

A Splitter splits a document into multiple ones \(e.g. comments in a news article, products on a eCommerce website\).

## DOMSplitter

Splits HTML, XHTML, or XML document on a specific element. This splitter constructs a DOM tree from the document content. That DOM tree is loaded entirely into memory. Use this splitter with caution if you know you'll need to parse huge files. It may be preferable to use a stream-based approach if this is a concern.

The [jsoup](http://jsoup.org/) parser library is used to load a document content into a DOM tree. Elements are referenced using a [CSS or JQuery-like syntax](http://jsoup.org/cookbook/extracting-data/selector-syntax).

![](../../../.gitbook/assets/image%20%28658%29.png)

* **selector**: [CSS or JQuery-like syntax](http://jsoup.org/cookbook/extracting-data/selector-syntax) to select the DOM elements that will be splitted.



