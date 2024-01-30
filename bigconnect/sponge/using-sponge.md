# Using Sponge

Sponge has two main components. A web UI for managing scraping projects and a Chrome browser extension for annotating web pages.

There are a few terms used in Sponge that users need to get used to at first:

| Term              | Description                                                                                        |
| ----------------- | -------------------------------------------------------------------------------------------------- |
| Project           | A scraping project for one website                                                                 |
| Source Field      | A field created by the Annotator when extracting an element from a web page using a CSS selector   |
| Destination Field | A field in a target system where a Source Field will be stored (eg. a database table)              |
| Agent             | A job runner that executes scraping jobs on local or remote machines                               |
| Committer         | A connection to a target system for storing scraped data (eg. Database, Object Storage, Zion etc.) |

{% content-ref url="sponge-manager/" %}
[sponge-manager](sponge-manager/)
{% endcontent-ref %}

{% content-ref url="sponge-annotator.md" %}
[sponge-annotator.md](sponge-annotator.md)
{% endcontent-ref %}

