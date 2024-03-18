# Result Rankings

Result Rankings are for changing the list of returned results for specific keywords.

With Results Rankings you can

* Designate results to pin to the top of the results list.
* Hide certain results for a search.
* Add results that normally wouldn’t be returned for a certain keyword.
* Re-order pinned results with a drag-and-drop interface.

NOTE

* Result Rankings affect only searches using the newer search widgets (Search Bar, Search Results, etc.). The rankings you customize do not apply to the legacy Search portlet results or to the individual application search bars.
* For the ranking to be applied, the user’s search phrase must match the result ranking’s keyword or one of its defined aliases exactly.

### [Requirements and Limitations](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/result-rankings#requirements-and-limitations) <a href="#requirements-and-limitations" id="requirements-and-limitations"></a>

#### [Elasticsearch is Required for Result Rankings](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/result-rankings#elasticsearch-is-required-for-result-rankings) <a href="#elasticsearch-is-required-for-result-rankings" id="elasticsearch-is-required-for-result-rankings"></a>

Result Rankings are only supported when using Elasticsearch as your search engine. Learn about installing Elasticsearch in [Getting Started with Elasticsearch](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/elasticsearch/getting-started-with-elasticsearch).

#### [Result Rankings Cannot Be Renamed](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/result-rankings#result-rankings-cannot-be-renamed) <a href="#result-rankings-cannot-be-renamed" id="result-rankings-cannot-be-renamed"></a>

An existing ranking entry cannot be renamed. Renaming requires recreating the ranking under a different name. See [LPS-96357](https://issues.liferay.com/browse/LPS-96357) for more information.

#### [Result Rankings and Blueprints](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/result-rankings#result-rankings-and-blueprints) <a href="#result-rankings-and-blueprints" id="result-rankings-and-blueprints"></a>

Result Rankings does not work well with [Search Blueprints](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/search-experiences/search-blueprints). If a result pinned by Result Rankings is filtered out in the blueprint, it still appears at the top of the results. If possible, avoid using these features together.

### [Creating and Managing Result Rankings](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/result-rankings#creating-and-managing-result-rankings) <a href="#creating-and-managing-result-rankings" id="creating-and-managing-result-rankings"></a>

To create a new Result Rankings,

1.  From the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) navigate to _Applications_ → _Search Tuning_ → _Result Rankings_.

    ![Navigate to the Result Rankings section in the Applications menu](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/result-rankings/images/01.png)
2. Click the Add icon (![Click on the add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to add a new Result Rankings.
3. Enter the keyword or phrase you want to use. Click _Customize Results_.
4.  A search query is executed and the results appear.

    The Result Rankings can be applied to additional keywords by adding words or phrases into the Aliases field. Click _Enter_ or use a comma to input additional aliases.

    ![Add additional keywords into the Aliases field.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/result-rankings/images/02.png)

    Click the _X_ next to a keyword to remove an alias.
5.  To pin a specific result, click _Options_ (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) and click _Pin Result_. You can also hover your mouse over a result and click on the pin icon.

    ![Click on the pin result link.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/result-rankings/images/03.png)

    Once more than one result is pinned, you can re-order your pinned results. Move your cursor to the left edge of the item you wish to re-order. Click and hold the item and drag it up or down your results list.

    ![Grab the left edge of a pinned result and drag it up or down.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/result-rankings/images/04.png)

    As you drag, your selected item appears as a rectangular box that you can move. The highlighted blue line indicates the item insertion point.
6.  To hide a specific result, click _Options_ (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) → _Hide Result_. You can also hover your mouse over a result and click on the hide result icon.

    ![Click on the hide result link.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/result-rankings/images/03.png)

    The list of hidden results can be found by clicking the _Hidden_ tab of the results. To make a hidden result visible again click _Options_ (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) → _Show Result_. You can also hover your mouse over a result and click on the show result icon.
7.  To add additional results, click _Add Result_. In the new window, use other search terms to locate additional results. Mark the results you want to add with a check and click the _Add_ button.

    ![Search for additional results and check mark ones to add.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/result-rankings/images/05.png)

    In the list of results you now see the additional results you just added.
8. Finish configuring your Result Rankings and click _Save_.
9. If you like, you can choose to deactivate specific Result Rankings. Click _Options_ (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) → _Deactivate_.

![Click on deactivate to deactivate a search query.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-administration-and-tuning/result-rankings/images/06.png)

To make a Result Rankings active again, click _Options_ (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) → _Activate_.

If you wish to permanently delete a Result Rankings, click _Options_ (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) → _Delete_.

### [Result Rankings Scope and Permissions](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/result-rankings#result-rankings-scope-and-permissions) <a href="#result-rankings-scope-and-permissions" id="result-rankings-scope-and-permissions"></a>

Because configuration of Result Rankings happens at the virtual instance scope, there are scoping and permissions behaviors to note.

Scope is disregarded for pinned results. Pinned results existing in Site A always appear in searches from Site B, even if the Search Bar Scope is set to _This Site_ as opposed to searching _Everything_.

Search from Result Rankings is global. When searching for results in Result Rankings admin, relevant results from all Sites are returned.

Permissions are applied as usual. If a User doesn’t have permission to see an asset, pinning it does not make it appear in the search results for that User.

### [Result Rankings Aliases versus Synonyms Sets](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/result-rankings#result-rankings-aliases-versus-synonyms-sets) <a href="#result-rankings-aliases-versus-synonyms-sets" id="result-rankings-aliases-versus-synonyms-sets"></a>

The similarities between result rankings and [synonym sets](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/synonym-sets) might cause some confusion. Synonym sets expand the search to include results matched by additional (synonymous) keywords, so more results are returned if there are matches to the synonyms.

Result rankings are keywords with manual ranking interventions applied. If the user searches for the result ranking’s configured keyword or its aliases, then the pins, hides, re-ordering, and additional results take effect.

These features don’t interact in a predictable way. If you need synonym-like behavior in results rankings, define aliases for the keyword.
