# Functions

Spark SQL provides two function features to meet a wide range of user needs: built-in functions and user-defined functions (UDFs). Built-in functions are commonly used routines that Spark SQL predefines and a complete list of the functions can be found in the [Built-in Functions](https://spark.apache.org/docs/latest/api/sql/) API document. UDFs allow users to define their own functions when the system’s built-in functions are not enough to perform the desired task.

#### Built-in Functions

Spark SQL has some categories of frequently-used built-in functions for aggregation, arrays/maps, date/timestamp, and JSON data. This subsection presents the usages and descriptions of these functions.

**Scalar Functions**

* Array Functions
* Map Functions
* Date and Timestamp Functions
* JSON Functions
* Mathematical Functions
* String Functions
* Bitwise Functions
* Conversion Functions
* Conditional Functions
* Predicate Functions
* Csv Functions
* Misc Functions

**Aggregate-like Functions**

* Aggregate Functions
* Window Functions

**Generator Functions**

* Generator Functions

#### UDFs (User-Defined Functions)

User-Defined Functions (UDFs) are a feature of Spark SQL that allows users to define their own functions when the system’s built-in functions are not enough to perform the desired task. To use UDFs in Spark SQL, users must first define the function, then register the function with Spark, and finally call the registered function. The User-Defined Functions can act on a single row or act on multiple rows at once. Spark SQL also supports integration of existing Hive implementations of UDFs, UDAFs and UDTFs.

* Scalar User-Defined Functions (UDFs)
* User-Defined Aggregate Functions (UDAFs)
* Integration with Hive UDFs/UDAFs/UDTFs
