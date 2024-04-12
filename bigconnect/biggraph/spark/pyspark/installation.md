# Installation

PySpark is included in the official releases of Spark available in the [Apache Spark website](https://spark.apache.org/downloads.html). For Python users, PySpark also provides `pip` installation from PyPI. This is usually for local usage or as a client to connect to a cluster instead of setting up a cluster itself.

This page includes instructions for installing PySpark by using pip, Conda, downloading manually, and building from the source.

### Python Versions Supported

Python 3.8 and above.

### Using PyPI

PySpark installation using [PyPI](https://pypi.org/project/pyspark/) is as follows:

```
pip install pyspark
```

If you want to install extra dependencies for a specific component, you can install it as below:

```
# Spark SQL
pip install pyspark[sql]
# pandas API on Spark
pip install pyspark[pandas_on_spark] plotly  # to plot your data, you can install plotly together.
# Spark Connect
pip install pyspark[connect]
```

For PySpark with/without a specific Hadoop version, you can install it by using `PYSPARK_HADOOP_VERSION` environment variables as below:

```
PYSPARK_HADOOP_VERSION=3 pip install pyspark
```

The default distribution uses Hadoop 3.3 and Hive 2.3. If users specify different versions of Hadoop, the pip installation automatically downloads a different version and uses it in PySpark. Downloading it can take a while depending on the network and the mirror chosen. `PYSPARK_RELEASE_MIRROR` can be set to manually choose the mirror for faster downloading.

```
PYSPARK_RELEASE_MIRROR=http://mirror.apache-kr.org PYSPARK_HADOOP_VERSION=3 pip install
```

It is recommended to use `-v` option in `pip` to track the installation and download status.

```
PYSPARK_HADOOP_VERSION=3 pip install pyspark -v
```

Supported values in `PYSPARK_HADOOP_VERSION` are:

* `without`: Spark pre-built with user-provided Apache Hadoop
* `3`: Spark pre-built for Apache Hadoop 3.3 and later (default)

Note that this installation of PySpark with/without a specific Hadoop version is experimental. It can change or be removed between minor releases.

### Using Conda

Conda is an open-source package management and environment management system (developed by [Anaconda](https://www.anaconda.com/)), which is best installed through [Miniconda](https://docs.conda.io/en/latest/miniconda.html/) or [Miniforge](https://github.com/conda-forge/miniforge/). The tool is both cross-platform and language agnostic, and in practice, conda can replace both [pip](https://pip.pypa.io/en/latest/) and [virtualenv](https://virtualenv.pypa.io/en/latest/).

Conda uses so-called channels to distribute packages, and together with the default channels by Anaconda itself, the most important channel is [conda-forge](https://conda-forge.org/), which is the community-driven packaging effort that is the most extensive & the most current (and also serves as the upstream for the Anaconda channels in most cases).

To create a new conda environment from your terminal and activate it, proceed as shown below:

```
conda create -n pyspark_env
conda activate pyspark_env
```

After activating the environment, use the following command to install pyspark, a python version of your choice, as well as other packages you want to use in the same session as pyspark (you can install in several steps too).

```
conda install -c conda-forge pyspark  # can also add "python=3.8 some_package [etc.]" here
```

Note that [PySpark for conda](https://anaconda.org/conda-forge/pyspark) is maintained separately by the community; while new versions generally get packaged quickly, the availability through conda(-forge) is not directly in sync with the PySpark release cycle.

While using pip in a conda environment is technically feasible (with the same command as above), this approach is [discouraged](https://www.anaconda.com/blog/using-pip-in-a-conda-environment/), because pip does not interoperate with conda.

For a short summary about useful conda commands, see their [cheat sheet](https://docs.conda.io/projects/conda/en/latest/user-guide/cheatsheet.html/).

### Manually Downloading

PySpark is included in the distributions available at the Apache Spark website. You can download a distribution you want from the site. After that, uncompress the tar file into the directory where you want to install Spark, for example, as below:

```
tar xzvf spark-master-bin-hadoop3.tgz
```

Ensure the `SPARK_HOME` environment variable points to the directory where the tar file has been extracted. Update `PYTHONPATH` environment variable such that it can find the PySpark and Py4J under `SPARK_HOME/python/lib`. One example of doing this is shown below:

```
cd spark-master-bin-hadoop3
export SPARK_HOME=`pwd`
export PYTHONPATH=$(ZIPS=("$SPARK_HOME"/python/lib/.zip); IFS=:; echo "${ZIPS[]}"):$PYTHONPATH
```

### Installing from Source

To install PySpark from source, refer to Building Spark.

### Dependencies

| Package                    | Supported version Note |                                                                                        |
| -------------------------- | ---------------------- | -------------------------------------------------------------------------------------- |
| _py4j_                     | >=0.10.9.7             | Required                                                                               |
| _pandas_                   | >=1.0.5                | Required for pandas API on Spark and Spark Connect; Optional for Spark SQL             |
| _pyarrow_                  | >=4.0.0                | Required for pandas API on Spark and Spark Connect; Optional for Spark SQL             |
| _numpy_                    | >=1.15                 | Required for pandas API on Spark and MLLib DataFrame-based API; Optional for Spark SQL |
| _grpcio_                   | >=1.48,<1.57           | Required for Spark Connect                                                             |
| _grpcio-status_            | >=1.48,<1.57           | Required for Spark Connect                                                             |
| _googleapis-common-protos_ | ==1.56.4               | Required for Spark Connect                                                             |

Note that PySpark requires Java 8 or later with `JAVA_HOME` properly set. If using JDK 11, set `-Dio.netty.tryReflectionSetAccessible=true` for Arrow related features and refer to Downloading.
