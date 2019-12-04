# Frequent Pattern Mining

Mining frequent items, itemsets, subsequences, or other substructures is usually among the first steps to analyze a large-scale dataset, which has been an active research topic in data mining for years. We refer users to Wikipedia’s [association rule learning](http://en.wikipedia.org/wiki/Association_rule_learning) for more information.

## FP-Growth

The FP-growth algorithm is described in the paper [Han et al., Mining frequent patterns without candidate generation](http://dx.doi.org/10.1145/335191.335372), where “FP” stands for frequent pattern. Given a dataset of transactions, the first step of FP-growth is to calculate item frequencies and identify frequent items. Different from [Apriori-like](http://en.wikipedia.org/wiki/Apriori_algorithm) algorithms designed for the same purpose, the second step of FP-growth uses a suffix tree \(FP-tree\) structure to encode transactions without generating candidate sets explicitly, which are usually expensive to generate. After the second step, the frequent itemsets can be extracted from the FP-tree. In `spark.mllib`, we implemented a parallel version of FP-growth called PFP, as described in [Li et al., PFP: Parallel FP-growth for query recommendation](http://dx.doi.org/10.1145/1454008.1454027). PFP distributes the work of growing FP-trees based on the suffixes of transactions, and hence is more scalable than a single-machine implementation. We refer users to the papers for more details.

`spark.ml`’s FP-growth implementation takes the following \(hyper-\)parameters:

* `minSupport`: the minimum support for an itemset to be identified as frequent. For example, if an item appears 3 out of 5 transactions, it has a support of 3/5=0.6.
* `minConfidence`: minimum confidence for generating Association Rule. Confidence is an indication of how often an association rule has been found to be true. For example, if in the transactions itemset `X` appears 4 times, `X` and `Y` co-occur only 2 times, the confidence for the rule `X => Y` is then 2/4 = 0.5. The parameter will not affect the mining for frequent itemsets, but specify the minimum confidence for generating association rules from frequent itemsets.
* `numPartitions`: the number of partitions used to distribute the work. By default the param is not set, and number of partitions of the input dataset is used.

The `FPGrowthModel` provides:

* `freqItemsets`: frequent itemsets in the format of DataFrame\(“items”\[Array\], “freq”\[Long\]\)
* `associationRules`: association rules generated with confidence above `minConfidence`, in the format of DataFrame\(“antecedent”\[Array\], “consequent”\[Array\], “confidence”\[Double\]\).
* `transform`: For each transaction in `itemsCol`, the `transform` method will compare its items against the antecedents of each association rule. If the record contains all the antecedents of a specific association rule, the rule will be considered as applicable and its consequents will be added to the prediction result. The transform method will summarize the consequents from all the applicable rules as prediction. The prediction column has the same data type as `itemsCol` and does not contain existing items in the `itemsCol`.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.fpm.FPGrowth

val dataset = spark.createDataset(Seq(
  "1 2 5",
  "1 2 3 5",
  "1 2")
).map(t => t.split(" ")).toDF("items")

val fpgrowth = new FPGrowth().setItemsCol("items").setMinSupport(0.5).setMinConfidence(0.6)
val model = fpgrowth.fit(dataset)

// Display frequent itemsets.
model.freqItemsets.show()

// Display generated association rules.
model.associationRules.show()

// transform examines the input items against all the association rules and summarize the
// consequents as prediction
model.transform(dataset).show()
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.fpm import FPGrowth

df = spark.createDataFrame([
    (0, [1, 2, 5]),
    (1, [1, 2, 3, 5]),
    (2, [1, 2])
], ["id", "items"])

fpGrowth = FPGrowth(itemsCol="items", minSupport=0.5, minConfidence=0.6)
model = fpGrowth.fit(df)

# Display frequent itemsets.
model.freqItemsets.show()

# Display generated association rules.
model.associationRules.show()

# transform examines the input items against all the association rules and summarize the
# consequents as prediction
model.transform(df).show()
```
{% endtab %}

{% tab title="R" %}
```r
# Load training data

df <- selectExpr(createDataFrame(data.frame(rawItems = c(
  "1,2,5", "1,2,3,5", "1,2"
))), "split(rawItems, ',') AS items")

fpm <- spark.fpGrowth(df, itemsCol="items", minSupport=0.5, minConfidence=0.6)

# Extracting frequent itemsets

spark.freqItemsets(fpm)

# Extracting association rules

spark.associationRules(fpm)

# Predict uses association rules to and combines possible consequents

predict(fpm, df)
```
{% endtab %}
{% endtabs %}

## PrefixSpan

PrefixSpan is a sequential pattern mining algorithm described in [Pei et al., Mining Sequential Patterns by Pattern-Growth: The PrefixSpan Approach](http://dx.doi.org/10.1109%2FTKDE.2004.77). We refer the reader to the referenced paper for formalizing the sequential pattern mining problem.

`spark.ml`’s PrefixSpan implementation takes the following parameters:

* `minSupport`: the minimum support required to be considered a frequent sequential pattern.
* `maxPatternLength`: the maximum length of a frequent sequential pattern. Any frequent pattern exceeding this length will not be included in the results.
* `maxLocalProjDBSize`: the maximum number of items allowed in a prefix-projected database before local iterative processing of the projected database begins. This parameter should be tuned with respect to the size of your executors.
* `sequenceCol`: the name of the sequence column in dataset \(default “sequence”\), rows with nulls in this column are ignored.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.fpm.PrefixSpan

val smallTestData = Seq(
  Seq(Seq(1, 2), Seq(3)),
  Seq(Seq(1), Seq(3, 2), Seq(1, 2)),
  Seq(Seq(1, 2), Seq(5)),
  Seq(Seq(6)))

val df = smallTestData.toDF("sequence")
val result = new PrefixSpan()
  .setMinSupport(0.5)
  .setMaxPatternLength(5)
  .setMaxLocalProjDBSize(32000000)
  .findFrequentSequentialPatterns(df)
  .show()
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.fpm import PrefixSpan

df = sc.parallelize([Row(sequence=[[1, 2], [3]]),
                     Row(sequence=[[1], [3, 2], [1, 2]]),
                     Row(sequence=[[1, 2], [5]]),
                     Row(sequence=[[6]])]).toDF()

prefixSpan = PrefixSpan(minSupport=0.5, maxPatternLength=5,
                        maxLocalProjDBSize=32000000)

# Find frequent sequential patterns.
prefixSpan.findFrequentSequentialPatterns(df).show()
```
{% endtab %}
{% endtabs %}



