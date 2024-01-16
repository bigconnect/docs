# Elasticsearch

The Elasticsearch origin is a multithreaded origin that reads data from an Elasticsearch cluster, including Elastic Cloud clusters (formerly Found clusters) and Amazon Elasticsearch Service clusters.&#x20;

The origin generates a record for each Elasticsearch document.

When you configure the Elasticsearch origin, you configure the HTTP URLs used to connect to the Elasticsearch cluster and specify whether security is enabled on the cluster. When Data Pipelines shares the same network as the Elasticsearch cluster, you can enter one or more node URLs and automatically detect additional Elasticsearch nodes on the cluster.

You configure the origin to run in batch or incremental mode.

The origin uses the Elasticsearch scroll API to run a query that you define. A query can retrieve large numbers of documents from Elasticsearch. This allows the origin to run a single query and then read multiple batches of data from the scroll until no results are left. You configure a scroll timeout that defines the amount of time that the search context remains valid.

When the pipeline stops, the Elasticsearch origin notes where it stops reading. When the pipeline starts again, the origin continues processing from where it stopped - as long as the scroll timeout has not been exceeded or the origin is not configured to delete the scroll when the pipeline stops. You can reset the origin to process all requested documents.

When you configure the Elasticsearch origin, you specify the maximum number of slices to split the scroll into. The number of slices determines how many threads the origin uses to read the data.

## Security <a href="#ariaid-title2" id="ariaid-title2"></a>

When security is enabled for the Elasticsearch cluster, you must specify the authentication method:

**Basic**

Use Basic authentication for Elasticsearch clusters outside of Amazon Elasticsearch Service. With Basic authentication, the stage passes the Elasticsearch user name and password.

**AWS Signature V4**

Use AWS Signature V4 authentication for Elasticsearch clusters within Amazon Elasticsearch Service. The stage must sign HTTP requests with Amazon Web Services credentials. For details, see the [Amazon Elasticsearch Service documentation](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-request-signing.html). Use one of the following methods to sign with AWS credentials:

**Instance profile**

When the execution engine - Data Pipelines - runs on an Amazon EC2 instance that has an associated instance profile, the engine uses the instance profile credentials to automatically authenticate with AWS.To use an instance profile, do not configure the Access Key ID and Secret Access Key properties.For more information about associating an instance profile with an EC2 instance, see the Amazon EC2 documentation.

**AWS access key pair**

When the execution engine does not run on an Amazon EC2 instance or when the EC2 instance doesn’t have an instance profile, you must specify the Access Key ID and Secret Access Key properties.

## Batch and Incremental Mode <a href="#ariaid-title3" id="ariaid-title3"></a>

The Elasticsearch origin can read data in the following modes:

**Batch mode**

In batch mode, the origin reads all data returned from the Elasticsearch query, and then the pipeline stops. By default, the origin reads in batch mode.

**Incremental mode**

In incremental mode, the origin performs an initial read of all the data returned from the Elasticsearch query, then the origin periodically runs the query again to check for new data.&#x20;

In incremental mode, the pipeline runs continuously so it can repeat the query at regular intervals.

To use incremental mode, you define the amount of time that the origin waits between queries, and you configure the offset field and initial offset value.

The origin uses an offset field and initial offset value to determine where to start reading data. By default, the offset field is defined as a field named timestamp. You can define any Elasticsearch field as the offset field, though you'll most likely want to use a date field. The initial offset value is a value within the offset field where you want the Elasticsearch origin to start reading.

Include both the offset field and the offset value in the Elasticsearch query.

After the initial read, the origin saves the last read offset value. When the origin runs the next incremental query, the origin uses the last read offset value to continue processing from where it stopped.

## Query <a href="#ariaid-title4" id="ariaid-title4"></a>

Define the query that the origin uses to return data from Elasticsearch. You can define any valid Elasticsearch query.

The default query is valid for batch mode and returns all documents from the Elasticsearch cluster:

```
{
  "query": {
    "match_all": {}
  }
}
```

You can optionally specify an Elasticsearch index or mapping type to define the scope of the query in either batch or incremental mode.

For example, let's say that you specify the `customer` index, do not specify a mapping type, configure the origin to use batch mode, and use the default query. The query properties are configured like so:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Elasticsearch\_DefaultQueryBatchWithIndex.png)

When you run the pipeline, the query returns all documents within the `customer` index.

### Incremental Mode Query <a href="#ariaid-title5" id="ariaid-title5"></a>

If you configure the origin to use incremental mode, you must include both the offset field and the offset value in the Elasticsearch query. Use `${OFFSET}` to represent the offset value.

For example, let's say that you do not define an index or mapping type, configure the origin to use incremental mode, and configure the `timestamp` field as the offset field. You use the Elasticsearch date math expression `now-1d/d` to set the initial offset value to one day before the current time. You include the offset field and offset value in the query to determine where to start reading data. The query properties are configured like so:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Elasticsearch\_IncrementalQuery.png)

When you run the pipeline, the query returns all documents that have a timestamp field with a value greater than one day before the current time. The query sorts the results by timestamp.

## Scroll Timeout <a href="#ariaid-title6" id="ariaid-title6"></a>

An Elasticsearch query can retrieve large numbers of documents from a single search request. This allows the Elasticsearch origin to run a single query, and then read multiple batches of data from the scroll until no results are left. An Elasticsearch scroll functions like a cursor in a traditional database.

To run an Elasticsearch query using the scroll API, you must set a scroll timeout. The scroll timeout tells Elasticsearch how long it should keep the search context alive. The scroll timeout must be long enough for a single batch to be fully read. When the origin reads another batch from the results, the scroll timeout is reset.

If the scroll timeout value is exceeded, Elasticsearch clears the scroll and the last saved offset, and the origin must run the query again. In batch mode, the origin reads all data returned from the query. In incremental mode, the origin begins reading from the initial offset configured for the origin.

Enter the scroll timeout using [Elasticsearch duration time units](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/common-options.html#time-units). For example, enter `1m` to set a one minute timeout, or enter `1h` to set a one hour timeout.

Optionally, you can configure the origin to delete the scroll when the pipeline stops. When configured, Data Pipelines clears the scroll as soon as the pipeline stops, rather than waiting for the scroll timeout value to be exceeded. Keeping a scroll window open consumes Elasticsearch resources. To free up those resources, configure the origin to clear the scroll when it is no longer being used.

Clearing the scroll also clears the last saved offset, and so is essentially like resetting the origin. When the pipeline continues, the origin processes all available data instead of processing data from the last saved offset.

### Multithreaded Processing <a href="#ariaid-title7" id="ariaid-title7"></a>

The Elasticsearch origin performs parallel processing and enables the creation of a multithreaded pipeline.

The Elasticsearch origin uses multiple concurrent threads based on the Number of Slices property. Each thread connects to the origin system, creates a batch of data, and passes the batch to an available pipeline runner.

A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin. Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

For example, the origin runs an Elasticsearch query that returns a large number of documents. To decrease processing time, you want to split the scroll into multiple slices and process each slice in parallel. You set the Number of Slices property to 5. When you start the pipeline, the origin splits the scroll into five slices, then the origin creates five threads, and Data Pipelines creates a matching number of pipeline runners. Upon receiving data, the origin passes a batch to each of the pipeline runners for processing.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

For more information about multithreaded pipelines, see Multithreaded Pipeline Overview.
