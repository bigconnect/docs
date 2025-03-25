# Run retrieval test

After your files are uploaded and parsed, it is recommended that you run a retrieval test before proceeding with the chat assistant configuration. Running a retrieval test is _not_ an unnecessary or superfluous step at all! Just like fine-tuning a precision instrument, Surogate requires careful tuning to deliver optimal question answering performance. Your knowledge base settings, chat assistant configurations, and the specified large and small models can all significantly impact the final results. Running a retrieval test verifies whether the intended chunks can be recovered, allowing you to quickly identify areas for improvement or pinpoint any issue that needs addressing. For instance, when debugging your question answering system, if you know that the correct chunks can be retrieved, you can focus your efforts elsewhere.&#x20;

During a retrieval test, chunks created from your specified chunk method are retrieved using a hybrid search. This search combines weighted keyword similarity with either weighted vector cosine similarity or a weighted reranking score, depending on your settings:

* If no rerank model is selected, weighted keyword similarity will be combined with weighted vector cosine similarity.
* If a rerank model is selected, weighted keyword similarity will be combined with weighted vector reranking score.

In contrast, chunks created from [knowledge graph](knowledge-graph.md) construction are retrieved solely using vector cosine similarity.

### Prerequisites[​](https://ragflow.io/docs/dev/run_retrieval_test#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

* Your files are uploaded and successfully parsed before running a retrieval test.
* A knowledge graph must be successfully built before enabling **Use knowledge graph**.

### Configurations[​](https://ragflow.io/docs/dev/run_retrieval_test#configurations) <a href="#configurations" id="configurations"></a>

#### Similarity threshold[​](https://ragflow.io/docs/dev/run_retrieval_test#similarity-threshold) <a href="#similarity-threshold" id="similarity-threshold"></a>

This sets the bar for retrieving chunks: chunks with similarities below the threshold will be filtered out. By default, the threshold is set to 0.2. That means that only chunks with hybrid similarity score of 20 or higher will be retrieved.

#### Keyword similarity weight[​](https://ragflow.io/docs/dev/run_retrieval_test#keyword-similarity-weight) <a href="#keyword-similarity-weight" id="keyword-similarity-weight"></a>

This sets the weight of keyword similarity in the combined similarity score, whether used with vector cosine similarity or a reranking score. By default, it is set to 0.7, making the weight of the other component 0.3 (1 - 0.7).

#### Rerank model[​](https://ragflow.io/docs/dev/run_retrieval_test#rerank-model) <a href="#rerank-model" id="rerank-model"></a>

* If left empty, Surogate will use a combination of weighted keyword similarity and weighted vector cosine similarity.
* If a rerank model is selected, weighted keyword similarity will be combined with weighted vector reranking score.

{% hint style="info" %}
Using a rerank model will significantly increase the time to receive a response.
{% endhint %}

#### Use knowledge graph[​](https://ragflow.io/docs/dev/run_retrieval_test#use-knowledge-graph) <a href="#use-knowledge-graph" id="use-knowledge-graph"></a>

In a knowledge graph, an entity description, a relationship description, or a community report each exists as an independent chunk. This switch indicates whether to add these chunks to the retrieval.

The switch is disabled by default. When enabled, Surogate performs the following during a retrieval test:

1. Extract entities and entity types from your query using the LLM.
2. Retrieve top N entities from the graph based on their PageRank values, using the extracted entity types.
3. Find similar entities and their N-hop relationships from the graph using the embeddings of the extracted query entities.
4. Retrieve similar relationships from the graph using the query embedding.
5. Rank these retrieved entities and relationships by multiplying each one's PageRank value with its similarity score to the query, returning the top n as the final retrieval.
6. Retrieve the report for the community involving the most entities in the final retrieval.\
   &#xNAN;_&#x54;he retrieved entity descriptions, relationship descriptions, and the top 1 community report are sent to the LLM for content generation._

{% hint style="info" %}
Using a knowledge graph in a retrieval test will significantly increase the time to receive a response.
{% endhint %}

#### Test text[​](https://ragflow.io/docs/dev/run_retrieval_test#test-text) <a href="#test-text" id="test-text"></a>

This field is where you put in your testing query.

\
