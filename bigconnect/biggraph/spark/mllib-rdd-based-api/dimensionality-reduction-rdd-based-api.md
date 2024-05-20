# Dimensionality Reduction - RDD-based API

* Singular value decomposition (SVD)
  * Performance
  * SVD Example
* Principal component analysis (PCA)

Dimensionality reduction is the process of reducing the number of variables under consideration. It can be used to extract latent features from raw and noisy features or compress data while maintaining the structure. `spark.mllib` provides support for dimensionality reduction on the RowMatrix class.

### Singular value decomposition (SVD)

Singular value decomposition (SVD) factorizes a matrix into three matrices: $$U$$, $$Σ$$Σ, and $$V$$ such that

$$A=UΣVT,$$

where

* $$U$$ is an orthonormal matrix, whose columns are called left singular vectors,
* $$Σ$$Σ is a diagonal matrix with non-negative diagonals in descending order, whose diagonals are called singular values,
* $$V$$ is an orthonormal matrix, whose columns are called right singular vectors.

For large matrices, usually we don’t need the complete factorization but only the top singular values and its associated singular vectors. This can save storage, de-noise and recover the low-rank structure of the matrix.

If we keep the top $$k$$ singular values, then the dimensions of the resulting low-rank matrix will be:

* $$U$$: $$m×k$$,
* $$Σ$$: $$k×k$$,
* $$V$$: $$n×k$$.

#### Performance

We assume $$n$$ is smaller than $$m$$. The singular values and the right singular vectors are derived from the eigenvalues and the eigenvectors of the Gramian matrix $$ATA$$ The matrix storing the left singular vectors $$U$$, is computed via matrix multiplication as $$U=A(VS−1)$$, if requested by the user via the computeU parameter. The actual method to use is determined automatically based on the computational cost:

* If $$n$$ is small ($$n<100$$) or $$k$$ is large compared with $$n$$ ($$k>n/2$$), we compute the Gramian matrix first and then compute its top eigenvalues and eigenvectors locally on the driver. This requires a single pass with $$O(n2)$$ storage on each executor and on the driver, and $$O(n2k)$$ time on the driver.
* Otherwise, we compute $$(ATA)v$$ in a distributive way and send it to [ARPACK](http://www.caam.rice.edu/software/ARPACK/) to compute $$(ATA)$$’s top eigenvalues and eigenvectors on the driver node. This requires $$O(k)$$ passes, $$O(n)$$storage on each executor, and $$O(nk)$$ storage on the driver.

#### SVD Example

`spark.mllib` provides SVD functionality to row-oriented matrices, provided in the [RowMatrix](https://spark.apache.org/docs/latest/mllib-data-types.html#rowmatrix) class.

* Python

Refer to the `SingularValueDecomposition` Python docs for details on the API.

```
from pyspark.mllib.linalg import Vectors
from pyspark.mllib.linalg.distributed import RowMatrix

rows = sc.parallelize([
    Vectors.sparse(5, {1: 1.0, 3: 7.0}),
    Vectors.dense(2.0, 0.0, 3.0, 4.0, 5.0),
    Vectors.dense(4.0, 0.0, 0.0, 6.0, 7.0)
])

mat = RowMatrix(rows)

# Compute the top 5 singular values and corresponding singular vectors.
svd = mat.computeSVD(5, computeU=True)
U = svd.U       # The U factor is a RowMatrix.
s = svd.s       # The singular values are stored in a local dense vector.
V = svd.V       # The V factor is a local dense matrix.
```

Find full example code at "examples/src/main/python/mllib/svd\_example.py" in the Spark repo.

The same code applies to `IndexedRowMatrix` if `U` is defined as an `IndexedRowMatrix`.

* Scala

Refer to the `SingularValueDecomposition` Scala docs for details on the API.

```
import org.apache.spark.mllib.linalg.Matrix
import org.apache.spark.mllib.linalg.SingularValueDecomposition
import org.apache.spark.mllib.linalg.Vector
import org.apache.spark.mllib.linalg.Vectors
import org.apache.spark.mllib.linalg.distributed.RowMatrix

val data = Array(
  Vectors.sparse(5, Seq((1, 1.0), (3, 7.0))),
  Vectors.dense(2.0, 0.0, 3.0, 4.0, 5.0),
  Vectors.dense(4.0, 0.0, 0.0, 6.0, 7.0))

val rows = sc.parallelize(data)

val mat: RowMatrix = new RowMatrix(rows)

// Compute the top 5 singular values and corresponding singular vectors.
val svd: SingularValueDecomposition[RowMatrix, Matrix] = mat.computeSVD(5, computeU = true)
val U: RowMatrix = svd.U  // The U factor is a RowMatrix.
val s: Vector = svd.s     // The singular values are stored in a local dense vector.
val V: Matrix = svd.V     // The V factor is a local dense matrix.
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/SVDExample.scala" in the Spark repo.

The same code applies to `IndexedRowMatrix` if `U` is defined as an `IndexedRowMatrix`.

* Java

Refer to the `SingularValueDecomposition` Java docs for details on the API.

```
import java.util.Arrays;
import java.util.List;

import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.linalg.Matrix;
import org.apache.spark.mllib.linalg.SingularValueDecomposition;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;
import org.apache.spark.mllib.linalg.distributed.RowMatrix;

List<Vector> data = Arrays.asList(
        Vectors.sparse(5, new int[] {1, 3}, new double[] {1.0, 7.0}),
        Vectors.dense(2.0, 0.0, 3.0, 4.0, 5.0),
        Vectors.dense(4.0, 0.0, 0.0, 6.0, 7.0)
);

JavaRDD<Vector> rows = jsc.parallelize(data);

// Create a RowMatrix from JavaRDD<Vector>.
RowMatrix mat = new RowMatrix(rows.rdd());

// Compute the top 5 singular values and corresponding singular vectors.
SingularValueDecomposition<RowMatrix, Matrix> svd = mat.computeSVD(5, true, 1.0E-9d);
RowMatrix U = svd.U();  // The U factor is a RowMatrix.
Vector s = svd.s();     // The singular values are stored in a local dense vector.
Matrix V = svd.V();     // The V factor is a local dense matrix.
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaSVDExample.java" in the Spark repo.

The same code applies to `IndexedRowMatrix` if `U` is defined as an `IndexedRowMatrix`.

### Principal component analysis (PCA)

[Principal component analysis (PCA)](http://en.wikipedia.org/wiki/Principal\_component\_analysis) is a statistical method to find a rotation such that the first coordinate has the largest variance possible, and each succeeding coordinate, in turn, has the largest variance possible. The columns of the rotation matrix are called principal components. PCA is used widely in dimensionality reduction.

`spark.mllib` supports PCA for tall-and-skinny matrices stored in row-oriented format and any Vectors.

* Python

The following code demonstrates how to compute principal components on a `RowMatrix` and use them to project the vectors into a low-dimensional space.

Refer to the `RowMatrix` Python docs for details on the API.

```
from pyspark.mllib.linalg import Vectors
from pyspark.mllib.linalg.distributed import RowMatrix

rows = sc.parallelize([
    Vectors.sparse(5, {1: 1.0, 3: 7.0}),
    Vectors.dense(2.0, 0.0, 3.0, 4.0, 5.0),
    Vectors.dense(4.0, 0.0, 0.0, 6.0, 7.0)
])

mat = RowMatrix(rows)
# Compute the top 4 principal components.
# Principal components are stored in a local dense matrix.
pc = mat.computePrincipalComponents(4)

# Project the rows to the linear space spanned by the top 4 principal components.
projected = mat.multiply(pc)
```

Find full example code at "examples/src/main/python/mllib/pca\_rowmatrix\_example.py" in the Spark repo.

* Scala

The following code demonstrates how to compute principal components on a `RowMatrix` and use them to project the vectors into a low-dimensional space.

Refer to the `RowMatrix` Scala docs for details on the API.

```
import org.apache.spark.mllib.linalg.Matrix
import org.apache.spark.mllib.linalg.Vectors
import org.apache.spark.mllib.linalg.distributed.RowMatrix

val data = Array(
  Vectors.sparse(5, Seq((1, 1.0), (3, 7.0))),
  Vectors.dense(2.0, 0.0, 3.0, 4.0, 5.0),
  Vectors.dense(4.0, 0.0, 0.0, 6.0, 7.0))

val rows = sc.parallelize(data)

val mat: RowMatrix = new RowMatrix(rows)

// Compute the top 4 principal components.
// Principal components are stored in a local dense matrix.
val pc: Matrix = mat.computePrincipalComponents(4)

// Project the rows to the linear space spanned by the top 4 principal components.
val projected: RowMatrix = mat.multiply(pc)
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/PCAOnRowMatrixExample.scala" in the Spark repo.

The following code demonstrates how to compute principal components on source vectors and use them to project the vectors into a low-dimensional space while keeping associated labels:

Refer to the `PCA` Scala docs for details on the API.

```
import org.apache.spark.mllib.feature.PCA
import org.apache.spark.mllib.linalg.Vectors
import org.apache.spark.mllib.regression.LabeledPoint
import org.apache.spark.rdd.RDD

val data: RDD[LabeledPoint] = sc.parallelize(Seq(
  new LabeledPoint(0, Vectors.dense(1, 0, 0, 0, 1)),
  new LabeledPoint(1, Vectors.dense(1, 1, 0, 1, 0)),
  new LabeledPoint(1, Vectors.dense(1, 1, 0, 0, 0)),
  new LabeledPoint(0, Vectors.dense(1, 0, 0, 0, 0)),
  new LabeledPoint(1, Vectors.dense(1, 1, 0, 0, 0))))

// Compute the top 5 principal components.
val pca = new PCA(5).fit(data.map(_.features))

// Project vectors to the linear space spanned by the top 5 principal
// components, keeping the label
val projected = data.map(p => p.copy(features = pca.transform(p.features)))
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/PCAOnSourceVectorExample.scala" in the Spark repo.

* Java

The following code demonstrates how to compute principal components on a `RowMatrix` and use them to project the vectors into a low-dimensional space.

Refer to the `RowMatrix` Java docs for details on the API.

```
import java.util.Arrays;
import java.util.List;

import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.linalg.Matrix;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;
import org.apache.spark.mllib.linalg.distributed.RowMatrix;

List<Vector> data = Arrays.asList(
        Vectors.sparse(5, new int[] {1, 3}, new double[] {1.0, 7.0}),
        Vectors.dense(2.0, 0.0, 3.0, 4.0, 5.0),
        Vectors.dense(4.0, 0.0, 0.0, 6.0, 7.0)
);

JavaRDD<Vector> rows = jsc.parallelize(data);

// Create a RowMatrix from JavaRDD<Vector>.
RowMatrix mat = new RowMatrix(rows.rdd());

// Compute the top 4 principal components.
// Principal components are stored in a local dense matrix.
Matrix pc = mat.computePrincipalComponents(4);

// Project the rows to the linear space spanned by the top 4 principal components.
RowMatrix projected = mat.multiply(pc);
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaPCAExample.java" in the Spark repo.
