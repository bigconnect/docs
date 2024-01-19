# Caching query results

If your question results don’t change frequently, you may want to store the results in Metabase so that the next time you visit the question, Metabase can retrieve the stored results rather than query the database again.

For example, if your data only updates once a day, there’s no point in querying the database more than once a day, as they data won’t have changed. Returning cached results can be significantly faster, as the database won’t have to recompute the results to load your question.

Metabase gives you the ability to automatically cache question results that meet a [minimum query duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#minimum-query-duration).

If your questions share a common model, you can enable [model caching](https://www.metabase.com/docs/latest/data-modeling/models#model-caching) instead.

[Caching doesn’t work with data sandboxing](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-doesnt-work-with-data-sandboxing)

Just something to keep in mind: if someone in a [sandboxed](https://www.metabase.com/docs/latest/permissions/data-sandboxes) group views a table, Metabase will skip the cached results and query the table directly, returning only the results the person is allowed to see.

[Enabling global caching](https://www.metabase.com/docs/latest/configuring-metabase/caching#enabling-global-caching)

1. Go to **Admin settings** > **Caching** (in the sidebar).
2. Click the toggle under **Saved Questions**.

Once you’ve enabled caching, you can choose when and what to cache from your [caching settings](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-settings).

By default, questions will get cached once their [average execution time](https://www.metabase.com/docs/latest/configuring-metabase/caching#average-query-execution-time) meets a [minimum query duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#minimum-query-duration) of 60 seconds.

[Caching location](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-location)

If you’re self-hosting Metabase, cached question results will be saved to your [application database](https://www.metabase.com/docs/latest/installation-and-operation/configuring-application-database).

If you’re using Metabase Cloud, cached question results will be saved to Metabase’s servers in the United States.

[Last updated at](https://www.metabase.com/docs/latest/configuring-metabase/caching#last-updated-at)

Questions that use the cache will display a “last cached at” timestamp in the question’s **info** panel.

[Getting fresh results](https://www.metabase.com/docs/latest/configuring-metabase/caching#getting-fresh-results)

To override a cached question result, re-run the question using the **refresh** button (counterclockwise arrow).

[Average query execution time](https://www.metabase.com/docs/latest/configuring-metabase/caching#average-query-execution-time)

Your Metabase instance keeps track of how long it takes each question to run. The average query execution time is used in your [caching settings](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-settings).

On [paid plans](https://www.metabase.com/pricing), you can view statistics about query execution time from your [auditing tools](https://www.metabase.com/docs/latest/usage-and-performance-tools/audit).

[Caching settings](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-settings)

You can tell Metabase when and what to cache from **Admin settings** > **Caching**:

* [Minimum query duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#minimum-query-duration)
* [Cache time-to-live (TTL) multiplier](https://www.metabase.com/docs/latest/configuring-metabase/caching#cache-time-to-live-ttl-multiplier)
* [Max cache entry size](https://www.metabase.com/docs/latest/configuring-metabase/caching#max-cache-entry-size)

[Minimum query duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#minimum-query-duration)

Metabase uses this number to decide whether a question will be cached or not.

Choose a duration (in seconds) that will trigger the cache. For example, you’d enter “60” if you want to cache all questions that take longer than 1 minute to load ([on average](https://www.metabase.com/docs/latest/configuring-metabase/caching#average-query-execution-time)).

[Cache time-to-live (TTL) multiplier](https://www.metabase.com/docs/latest/configuring-metabase/caching#cache-time-to-live-ttl-multiplier)

The TTL multiplier tells Metabase how long to persist a cached question result:

> Cache lifetime per question = TTL multiplier x [average execution time](https://www.metabase.com/docs/latest/configuring-metabase/caching#average-query-execution-time) per question

For example, if you enter a TTL multiplier of 10, a question that takes 5 seconds on average will be cached for 50 seconds. A question that takes 10 minutes will be cached for 100 minutes. This way, each question’s cache lifetime is proportional to that question’s execution time.

[Max cache entry size](https://www.metabase.com/docs/latest/configuring-metabase/caching#max-cache-entry-size)

To prevent cached results from taking up too much space on your server, you can set the maximum size of the cache (per question) in kilobytes.

[Advanced caching controls](https://www.metabase.com/docs/latest/configuring-metabase/caching#advanced-caching-controls)

Advanced caching controls is only available on [Pro](https://www.metabase.com/product/pro) and [Enterprise](https://www.metabase.com/product/enterprise) plans (both self-hosted and on Metabase Cloud).

All Metabase editions include global caching controls. On [paid plans](https://www.metabase.com/pricing), you can override your global [time-to-live (TTL) setting](https://www.metabase.com/docs/latest/configuring-metabase/caching#cache-time-to-live-ttl-multiplier) to set different cache lifetimes for specific databases, questions, or dashboards.

[Caching per database](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-per-database)

This setting tells Metabase how long to keep the cached results from a specific database.

1. Make sure [caching is enabled](https://www.metabase.com/docs/latest/configuring-metabase/caching#enabling-global-caching).
2. Go to **Admin settings** > **Databases** and select your database.
3. Open **Advanced options** and find the **Default result cache duration**.
4. Click **Custom** and enter a cache duration in hours.

The cache duration setting is useful for databases that take longer to query, or databases that are kept up to date on a special cadence.

This setting will override your [global cache duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#cache-time-to-live-ttl-multiplier).

[Caching per question](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-per-question)

You can tell Metabase how long to keep the cached results for specific questions. You’ll only find these cache settings on questions that exceed the [minimum query duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#minimum-query-duration).

1. Make sure [caching is enabled](https://www.metabase.com/docs/latest/configuring-metabase/caching#enabling-global-caching).
2. Go to your question.
3. Click on the **info** icon.
4. Click **Cache configuration**.
5. Enter a cache duration in hours.
6. Click **Save changes**.

You can use this setting to update questions on the same cadence as your data. For example, if your data gets updated daily, you can set the **Cache configuration** to 24 hours.

If set, your question cache duration will override the:

* [global cache duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#cache-time-to-live-ttl-multiplier)
* [database cache duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-per-database)

[Caching per dashboard](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-per-dashboard)

You can tell Metabase how long to keep the cached results for each of the questions on a dashboard.

1. Make sure [caching is enabled](https://www.metabase.com/docs/latest/configuring-metabase/caching#enabling-global-caching).
2. Go to your dashboard.
3. Click on the **info** icon.
4. Click **Cache configuration**.
5. Enter a cache duration in hours.
6. Click **Save changes**.

> This setting won’t cache the entire dashboard at once. The dashboard cache duration will only apply to questions that exceed the [minimum query duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#minimum-query-duration).

If set, your dashboard cache duration will override the:

* [global cache duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#cache-time-to-live-ttl-multiplier)
* [database cache duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-per-database)
* [question cache duration](https://www.metabase.com/docs/latest/configuring-metabase/caching#caching-per-question)
