# Re-Indexing Modes

_Re-indexing_ is refreshing all or part of the search index that’s used for searching, and in some cases displaying, Liferay’s content. Execute a re-index from the [search administration panel](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/search-administration) in Global Menu → Control Panel → Search → Index Actions. New re-indexing modes are included in the 2023.Q4/GA102 release: _Concurrent_ and _Sync_. These provide high availability re-indexing, so that the re-index process can run while your users continue to search the site’s content, eliminating downtime. Choose from the 3 available re-indexing modes:

| Re-Index Mode                                                                                                 | Description                                                                                                                                                                                                                            | High Availability | Supported Actions                                                                                |
| ------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------ |
| ![Full](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-globe-meridians.png) Full        | Deletes the index first, then regenerates it.                                                                                                                                                                                          | ✘                 | <p>✔ Global: all indexes<br>✔ Global: spell check dictionaries<br>✔ Single model re-indexing</p> |
| ![Concurrent](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-concurrent.png) Concurrent | <p><img src="https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-beta-feature.png" alt="Beta"><br>Creates a new index, leaving the old index in place (i.e., a blue/green re-index) until the new index is ready.</p> | ✔                 | <p>✔ Global: all indexes<br>✘ Global: spell check dictionaries<br>✘ Single model re-indexing</p> |
| ![Sync](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore2.png) Sync               | <p><img src="https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-beta-feature.png" alt="Beta"><br>Updates existing search index documents, and deletes stale documents when finished.</p>                             | ✔                 | <p>✔ Global: all indexes<br>✘ Global: spell check dictionaries<br>✔ Single model re-indexing</p> |

NOTE

Concurrent and Sync mode are only available [with Elasticsearch](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/solr/solr-limitations).

### [Choosing the Re-Index Mode](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/re-indexing-modes#choosing-the-re-index-mode) <a href="#choosing-the-re-index-mode" id="choosing-the-re-index-mode"></a>

A full re-index works in any scenario, whether for all content and indexes or a single model. However, it’s not always the best choice. To avoid downtime due to a long running full re-index process, use concurrent or sync mode whenever possible.

| Re-Index when:                                                                                                                                                                                         | Full | Concurrent (![Beta](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-beta-feature.png)) | Sync (![Beta](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-beta-feature.png)) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---- | ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| [Installing a new Elasticsearch cluster](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/elasticsearch/installing-elasticsearch)                       | ✔    | -                                                                                                           | -                                                                                                     |
| [Updating field mappings](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/elasticsearch/advanced-configuration-of-the-liferay-elasticsearch-connector) | ✓    | ✔                                                                                                           | -                                                                                                     |
| [Updating index settings](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/elasticsearch/advanced-configuration-of-the-liferay-elasticsearch-connector) | ✓    | ✔                                                                                                           | -                                                                                                     |
| [Upgrading Liferay](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/upgrading-liferay)                                                                                             | ✓    | ✔                                                                                                           | ✓                                                                                                     |
| [Upgrading Elasticsearch](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/elasticsearch/upgrading-elasticsearch)                                       | ✓    | ✔                                                                                                           | ✓                                                                                                     |
| Re-indexing after a connection outage                                                                                                                                                                  | ✓    | ✔                                                                                                           | ✓                                                                                                     |
| Re-indexing a single model[1](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/re-indexing-modes#fn-1)                                                          | ✓    | -                                                                                                           | ✔                                                                                                     |

✔ The mode is recommended for the scenario

✓ The mode is available for the scenario

\- The mode is not available for the scenario

In earlier Liferay versions, re-indexing always uses full mode, deleting content from the live production index before regenerating it. This can result in significant search index downtime. With concurrent and sync re-index modes, re-indexing is accomplished without deleting the index first. Sync mode updates the index in place, while concurrent mode builds a new index in the background, preserving the old one until it’s ready. High availability of the search index means minimal downtime, which means your site’s users can browse and search your site while you re-index the site’s content.

![Choose from Full, Concurrent, or Sync re-index modes, depending on your needs.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/re-indexing-modes/images/01.png)

Whenever possible, choose concurrent or sync mode to avoid downtime during a re-index. However, there are considerations to account for when deciding which mode is right in your situation.

### [![Full](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-globe-meridians.png) Full Re-Index Mode](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/re-indexing-modes#full-full-re-index-mode) <a href="#full-full-re-index-mode" id="full-full-re-index-mode"></a>

You must use _full_ mode to index all content when connecting to a new, empty Elasticsearch cluster. In this scenario, high availability is not a consideration. You can also use full mode for single model re-indexing when sync mode will not suffice, but sync mode is recommended. For high availability re-indexing of all indexes, use concurrent mode instead.

A full re-index proceeds this way:

1. The original index is deleted.
2. A new index is created with the latest index settings and field mappings.
3. Documents are regenerated in the index.

### [![Concurrent](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-concurrent.png) Concurrent Re-Index Mode](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/re-indexing-modes#concurrent-concurrent-re-index-mode) <a href="#concurrent-concurrent-re-index-mode" id="concurrent-concurrent-re-index-mode"></a>

Use _concurrent_ mode for high availability re-indexing of all content using a blue/green re-indexing strategy. Concurrent mode cannot be used for single model re-indexing: instead use sync mode. Concurrent mode serves as a replacement for full re-indexing mode when re-indexing all content.

A concurrent re-index proceeds this way:

1. A new index is created with the latest index settings and field mappings.
2. Documents are regenerated in the index.
3. The original index is deleted.

Because concurrent re-indexing creates a new index alongside the old one, it requires more disk space in the Elasticsearch system’s infrastructure than the other modes. To avoid errors due to insufficient disk space, Liferay estimates whether the available space is sufficient and warns you if it isn’t:

![You're warned if Liferay estimates insufficient disk space for a concurrent re-index.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/re-indexing-modes/images/03.png)

### [![Sync](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore2.png) Sync Re-Index Mode](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/re-indexing-modes#sync-sync-re-index-mode) <a href="#sync-sync-re-index-mode" id="sync-sync-re-index-mode"></a>

Use _sync_ mode for high availability re-indexing when primary disk space for Elasticsearch is a concern, or for all high availability single model re-indexing needs. There are scenarios when sync mode can’t properly account for system changes, and one of the other modes must be used.

A sync re-index proceeds this way:

1. Index documents are updated, but not deleted.
2. Stale documents are deleted.

A sync re-index does not take into account updated index settings or mappings, and is therefore more limited than a concurrent re-index.

### [Setting the Default Re-Indexing Mode](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/re-indexing-modes#setting-the-default-re-indexing-mode) <a href="#setting-the-default-re-indexing-mode" id="setting-the-default-re-indexing-mode"></a>

Full re-index mode is the default. To change it,

1. Go to _Global Menu_ → _Control Panel_ → _System Settings_.
2. Open the _Reindex_ entry under Platform → Search.
3. Set the _Default Reindex Execution Mode_ and click _Update_.

![The default re-indexing mode is configurable.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/re-indexing-modes/images/02.png)

***

1. Re-indexing a single model is occasionally required, and sync mode is recommended in these cases. For example, using certain elements while [creating a search blueprint](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/search-experiences/search-blueprints/creating-and-managing-search-blueprints#adding-elements-to-the-blueprint) and [enabling semantic search](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/search-experiences/semantic-search) require re-indexing individual model types.
