# Data Sources

In this section, we introduce how to use data source in ML to load data. Beside some general data sources such as Parquet, CSV, JSON and JDBC, we also provide some specific data sources for ML.

## Image data source

This image data source is used to load image files from a directory, it can load compressed image \(jpeg, png, etc.\) into raw image representation via `ImageIO` in Java library. The loaded DataFrame has one `StructType` column: “image”, containing image data stored as image schema. The schema of the `image` column is:

* origin: `StringType` \(represents the file path of the image\)
* height: `IntegerType` \(height of the image\)
* width: `IntegerType` \(width of the image\)
* nChannels: `IntegerType` \(number of image channels\)
* mode: `IntegerType` \(OpenCV-compatible type\)
* data: `BinaryType` \(Image bytes in OpenCV-compatible order: row-wise BGR in most cases\)

{% tabs %}
{% tab title="Scala" %}
```scala
val df = spark.read.format("image").option("dropInvalid", true).load("data/mllib/images/origin/kittens")
df: org.apache.spark.sql.DataFrame = [image: struct<origin: string, height: int ... 4 more fields>]

df.select("image.origin", "image.width", "image.height").show(truncate=false)
+-----------------------------------------------------------------------+-----+------+
|origin                                                                 |width|height|
+-----------------------------------------------------------------------+-----+------+
|file:///spark/data/mllib/images/origin/kittens/54893.jpg               |300  |311   |
|file:///spark/data/mllib/images/origin/kittens/DP802813.jpg            |199  |313   |
|file:///spark/data/mllib/images/origin/kittens/29.5.a_b_EGDP022204.jpg |300  |200   |
|file:///spark/data/mllib/images/origin/kittens/DP153539.jpg            |300  |296   |
+-----------------------------------------------------------------------+-----+------+
```
{% endtab %}

{% tab title="Java" %}
```java
Dataset<Row> imagesDF = spark.read().format("image").option("dropInvalid", true).load("data/mllib/images/origin/kittens");
imageDF.select("image.origin", "image.width", "image.height").show(false);
/*
Will output:
+-----------------------------------------------------------------------+-----+------+
|origin                                                                 |width|height|
+-----------------------------------------------------------------------+-----+------+
|file:///spark/data/mllib/images/origin/kittens/54893.jpg               |300  |311   |
|file:///spark/data/mllib/images/origin/kittens/DP802813.jpg            |199  |313   |
|file:///spark/data/mllib/images/origin/kittens/29.5.a_b_EGDP022204.jpg |300  |200   |
|file:///spark/data/mllib/images/origin/kittens/DP153539.jpg            |300  |296   |
+-----------------------------------------------------------------------+-----+------+
*/
```
{% endtab %}

{% tab title="Python" %}
```python
df = spark.read.format("image").option("dropInvalid", true).load("data/mllib/images/origin/kittens")
df.select("image.origin", "image.width", "image.height").show(truncate=False)
+-----------------------------------------------------------------------+-----+------+
|origin                                                                 |width|height|
+-----------------------------------------------------------------------+-----+------+
|file:///spark/data/mllib/images/origin/kittens/54893.jpg               |300  |311   |
|file:///spark/data/mllib/images/origin/kittens/DP802813.jpg            |199  |313   |
|file:///spark/data/mllib/images/origin/kittens/29.5.a_b_EGDP022204.jpg |300  |200   |
|file:///spark/data/mllib/images/origin/kittens/DP153539.jpg            |300  |296   |
+-----------------------------------------------------------------------+-----+------+
```
{% endtab %}

{% tab title="R" %}
```r
df = read.df("data/mllib/images/origin/kittens", "image")
head(select(df, df$image.origin, df$image.width, df$image.height))

1               file:///spark/data/mllib/images/origin/kittens/54893.jpg
2            file:///spark/data/mllib/images/origin/kittens/DP802813.jpg
3 file:///spark/data/mllib/images/origin/kittens/29.5.a_b_EGDP022204.jpg
4            file:///spark/data/mllib/images/origin/kittens/DP153539.jpg
  width height
1   300    311
2   199    313
3   300    200
4   300    296
```
{% endtab %}
{% endtabs %}

