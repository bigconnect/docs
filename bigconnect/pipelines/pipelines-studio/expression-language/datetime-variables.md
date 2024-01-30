# Datetime Variables

The expression language provides datetime variables for use in expressions.

You can use datetime variables to configure a Hadoop FS, Local FS, or MapR FS destination to write records to time-based directories.

You can use datetime variables to configure an Amazon S3 or Elasticsearch destination to write records to a time-based partition prefix or to a time-based index. And you can use them in the partition value expression in the Hive Metadata processor.

The expression language provides the following datetime variables:

* ${YYYY()} - four digit year
* ${YY()} - two digit year
* ${MM()} - two digit month
* ${DD()} - two digit date
* ${hh()} - two digit hour
* ${mm()} - two digit minute
* ${ss()} - two digit second

When you use datetime variables in most stages, use all of the datetime variables between one of the year variables and the smallest variable that you want to use. For example, to create directories on a daily basis for a Hadoop FS destination, use a year variable, a month variable, and then a day variable. You might use one of the following datetime variable progressions:

```
${YYYY()}-${MM()}-${DD()}
${YY()}_${MM()}_${DD()}
```

In the Hive Metadata processor, you can use the datetime variables as needed.
