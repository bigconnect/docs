# Siddhi APIs

### Core <a href="#core" id="core"></a>

#### and [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#and-aggregate-function" id="and-aggregate-function"></a>

Returns the results of AND operation for all the events.

_Origin: siddhi-core:5.0.0_

Syntax

```
<BOOL> and(<BOOL> arg)
```

QUERY PARAMETERS

| Name | Description                               | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ----------------------------------------- | ------------- | ------------------- | -------- | ------- |
| arg  | The value that needs to be AND operation. |               | BOOL                | No       | No      |

ExamplesEXAMPLE 1

```
from cscStream#window.lengthBatch(10)
select and(isFraud) as isFraudTransaction
insert into alertStream;
```

This will returns the result for AND operation of isFraud values as a boolean value for event chunk expiry by window length batch.

#### avg [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#avg-aggregate-function" id="avg-aggregate-function"></a>

Calculates the average for all the events.

_Origin: siddhi-core:5.0.0_

Syntax

```
<DOUBLE> avg(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                         | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ----------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | The value that need to be averaged. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

Examples

```
from fooStream#window.timeBatch
 select avg(temp) as avgTemp
 insert into barStream;
```

avg(temp) returns the average temp value for all the events based on their arrival and expiry.

#### count [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#count-aggregate-function" id="count-aggregate-function"></a>

Returns the count of all the events.

_Origin: siddhi-core:5.0.0_

Syntax

```
<LONG> count()
```

Examples

```
from fooStream#window.timeBatch(10 sec)
select count() as count
insert into barStream;
```

This will return the count of all the events for time batch in 10 seconds.

#### distinctCount [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#distinctcount-aggregate-function" id="distinctcount-aggregate-function"></a>

This returns the count of distinct occurrences for a given arg.

_Origin: siddhi-core:5.0.0_

Syntax

```
<LONG> distinctCount(<INT|LONG|DOUBLE|FLOAT|STRING> arg)
```

QUERY PARAMETERS

| Name | Description                                                                 | Default Value | Possible Data Types                             | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------- | ------------- | ----------------------------------------------- | -------- | ------- |
| arg  | The object for which the number of distinct occurences needs to be counted. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING</p> | No       | No      |

Examples

```
from fooStream
select distinctcount(pageID) as count
insert into barStream;
```

distinctcount(pageID) for the following output returns '3' when the available values are as follows.\
&#x20;"WEB\_PAGE\_1"\
&#x20;"WEB\_PAGE\_1"\
&#x20;"WEB\_PAGE\_2"\
&#x20;"WEB\_PAGE\_3"\
&#x20;"WEB\_PAGE\_1"\
&#x20;"WEB\_PAGE\_2"\
&#x20;The three distinct occurences identified are 'WEB\_PAGE\_1', 'WEB\_PAGE\_2', and 'WEB\_PAGE\_3'.

#### max [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#max-aggregate-function" id="max-aggregate-function"></a>

Returns the maximum value for all the events.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT> max(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                                                    | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | -------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | The value that needs to be compared to find the maximum value. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

Examples

```
from fooStream#window.timeBatch(10 sec)
select max(temp) as maxTemp
insert into barStream;
```

max(temp) returns the maximum temp value recorded for all the events based on their arrival and expiry.

#### maxForever [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#maxforever-aggregate-function" id="maxforever-aggregate-function"></a>

This is the attribute aggregator to store the maximum value for a given attribute throughout the lifetime of the query regardless of any windows in-front.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT> maxForever(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                                                    | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | -------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | The value that needs to be compared to find the maximum value. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

Examples

```
from inputStream
select maxForever(temp) as max
insert into outputStream;
```

maxForever(temp) returns the maximum temp value recorded for all the events throughout the lifetime of the query.

#### min [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#min-aggregate-function" id="min-aggregate-function"></a>

Returns the minimum value for all the events.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT> min(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                                                    | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | -------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | The value that needs to be compared to find the minimum value. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

Examples

```
from inputStream
select min(temp) as minTemp
insert into outputStream;
```

min(temp) returns the minimum temp value recorded for all the events based on their arrival and expiry.

#### minForever [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#minforever-aggregate-function" id="minforever-aggregate-function"></a>

This is the attribute aggregator to store the minimum value for a given attribute throughout the lifetime of the query regardless of any windows in-front.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT> minForever(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                                                    | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | -------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | The value that needs to be compared to find the minimum value. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

Examples

```
from inputStream
select minForever(temp) as max
insert into outputStream;
```

minForever(temp) returns the minimum temp value recorded for all the events throughoutthe lifetime of the query.

#### or [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#or-aggregate-function" id="or-aggregate-function"></a>

Returns the results of OR operation for all the events.

_Origin: siddhi-core:5.0.0_

Syntax

```
<BOOL> or(<BOOL> arg)
```

QUERY PARAMETERS

| Name | Description                              | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ---------------------------------------- | ------------- | ------------------- | -------- | ------- |
| arg  | The value that needs to be OR operation. |               | BOOL                | No       | No      |

Examples

```
from cscStream#window.lengthBatch(10)
select or(isFraud) as isFraudTransaction
insert into alertStream;
```

This will returns the result for OR operation of isFraud values as a boolean value for event chunk expiry by window length batch.

#### stdDev [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#stddev-aggregate-function" id="stddev-aggregate-function"></a>

Returns the calculated standard deviation for all the events.

_Origin: siddhi-core:5.0.0_

Syntax

```
<DOUBLE> stdDev(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                                                        | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------------ | ------------- | ------------------------------------- | -------- | ------- |
| arg  | The value that should be used to calculate the standard deviation. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

Examples

```
from inputStream
select stddev(temp) as stdTemp
insert into outputStream;
```

stddev(temp) returns the calculated standard deviation of temp for all the events based on their arrival and expiry.

#### sum [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#sum-aggregate-function" id="sum-aggregate-function"></a>

Returns the sum for all the events.

_Origin: siddhi-core:5.0.0_

Syntax

```
<LONG|DOUBLE> sum(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                        | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ---------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | The value that needs to be summed. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

Examples

```
from inputStream
select sum(volume) as sumOfVolume
insert into outputStream;
```

This will returns the sum of volume values as a long value for each event arrival and expiry.

#### unionSet [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#unionset-aggregate-function" id="unionset-aggregate-function"></a>

Union multiple sets.\
&#x20;This attribute aggregator maintains a union of sets. The given input set is put into the union set and the union set is returned.

_Origin: siddhi-core:5.0.0_

Syntax

```
<OBJECT> unionSet(<OBJECT> set)
```

QUERY PARAMETERS

| Name | Description                                                         | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| set  | The java.util.Set object that needs to be added into the union set. |               | OBJECT              | No       | No      |

Examples

```
from stockStream 
select createSet(symbol) as initialSet 
insert into initStream 

from initStream#window.timeBatch(10 sec) 
select unionSet(initialSet) as distinctSymbols 
insert into distinctStockStream;
```

distinctStockStream will return the set object which contains the distinct set of stock symbols received during a sliding window of 10 seconds.

#### UUID [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#uuid-function" id="uuid-function"></a>

Generates a UUID (Universally Unique Identifier).

_Origin: siddhi-core:5.0.0_

Syntax

```
<STRING> UUID()
```

Examples

```
from TempStream
select convert(roomNo, 'string') as roomNo, temp, UUID() as messageID
insert into RoomTempStream;
```

This will converts a room number to string, introducing a message ID to each event asUUID() returns a34eec40-32c2-44fe-8075-7f4fde2e2dd8\
\
from TempStream\
select convert(roomNo, 'string') as roomNo, temp, UUID() as messageID\
insert into RoomTempStream;

#### cast [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#cast-function" id="cast-function"></a>

Converts the first parameter according to the cast.to parameter. Incompatible arguments cause Class Cast exceptions if further processed. This function is used with map extension that returns attributes of the object type. You can use this function to cast the object to an accurate and concrete type.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> cast(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> to.be.caster, <STRING> cast.to)
```

QUERY PARAMETERS

| Name         | Description                                                                                                                                | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| to.be.caster | This specifies the attribute to be casted.                                                                                                 |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |
| cast.to      | A string constant parameter expressing the cast to type using one of the following strings values: int, long, float, double, string, bool. |               | STRING                                                            | No       | No      |

Examples

```
from fooStream
select symbol as name, cast(temp, 'double') as temp
insert into barStream;
```

This will cast the fooStream temp field value into 'double' format.

#### coalesce [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#coalesce-function" id="coalesce-function"></a>

Returns the value of the first input parameter that is not null, and all input parameters have to be on the same type.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> coalesce(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> args)
```

QUERY PARAMETERS

| Name | Description                                                                                                                                               | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| args | This function accepts one or more parameters. They can belong to any one of the available types. All the specified parameters should be of the same type. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

Examples

```
from fooStream
select coalesce('123', null, '789') as value
insert into barStream;
```

This will returns first null value 123.

EXAMPLE 2

```
from fooStream
select coalesce(null, 76, 567) as value
insert into barStream;
```

This will returns first null value 76.

EXAMPLE 3

```
from fooStream
select coalesce(null, null, null) as value
insert into barStream;
```

This will returns null as there are no notnull values.

#### convert [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#convert-function" id="convert-function"></a>

Converts the first input parameter according to the convertedTo parameter.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT|STRING|BOOL> convert(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL> to.be.converted, <STRING> converted.to)
```

QUERY PARAMETERS

| Name            | Description                                                                                                                                                                 | Default Value | Possible Data Types                                     | Optional | Dynamic |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------------------------- | -------- | ------- |
| to.be.converted | This specifies the value to be converted.                                                                                                                                   |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL</p> | No       | No      |
| converted.to    | A string constant parameter to which type the attribute need to be converted using one of the following strings values: 'int', 'long', 'float', 'double', 'string', 'bool'. |               | STRING                                                  | No       | No      |

Examples

```
from fooStream
select convert(temp, 'double') as temp
insert into barStream;
```

This will convert fooStream temp value into 'double'.

EXAMPLE 2

```
from fooStream
select convert(temp, 'int') as temp
insert into barStream;
```

This will convert fooStream temp value into 'int' (value = "convert(45.9, 'int') returns 46").

#### createSet [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#createset-function" id="createset-function"></a>

Includes the given input parameter in a java.util.HashSet and returns the set.

_Origin: siddhi-core:5.0.0_

Syntax

```
<OBJECT> createSet(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL> input)
```

QUERY PARAMETERS

| Name  | Description                                    | Default Value | Possible Data Types                                     | Optional | Dynamic |
| ----- | ---------------------------------------------- | ------------- | ------------------------------------------------------- | -------- | ------- |
| input | The input that needs to be added into the set. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL</p> | No       | No      |

Examples

```
from stockStream 
select createSet(symbol) as initialSet 
insert into initStream;
```

For every incoming stockStream event, the initStream stream will produce a set object having only one element: the symbol in the incoming stockStream.

#### currentTimeMillis [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#currenttimemillis-function" id="currenttimemillis-function"></a>

Returns the current timestamp of siddhi application in milliseconds.

_Origin: siddhi-core:5.0.0_

Syntax

```
<LONG> currentTimeMillis()
```

Examples

```
from fooStream
select symbol as name, currentTimeMillis() as eventTimestamp 
insert into barStream;
```

This will extract current siddhi application timestamp.

#### default [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#default-function" id="default-function"></a>

Checks if the 'attribute' parameter is null and if so returns the value of the 'default' parameter

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> default(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> attribute, <INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> default)
```

QUERY PARAMETERS

| Name      | Description                                                            | Default Value | Possible Data Types                                               | Optional | Dynamic |
| --------- | ---------------------------------------------------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| attribute | The attribute that could be null.                                      |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |
| default   | The default value that will be used when 'attribute' parameter is null |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

Examples

```
from TempStream
select default(temp, 0.0) as temp, roomNum
insert into StandardTempStream;
```

This will replace TempStream's temp attribute with default value if the temp is null.

#### eventTimestamp [_(Function_](https://siddhi.io/en/v5.0/docs/query-guide/#function)_)_ <a href="#eventtimestamp-function" id="eventtimestamp-function"></a>

Returns the timestamp of the processed event.

_Origin: siddhi-core:5.0.0_

Syntax

```
<LONG> eventTimestamp()
```

Examples

```
from fooStream
select symbol as name, eventTimestamp() as eventTimestamp 
insert into barStream;
```

This will extract current events timestamp.

#### ifThenElse [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#ifthenelse-function" id="ifthenelse-function"></a>

Evaluates the 'condition' parameter and returns value of the 'if.expression' parameter if the condition is true, or returns value of the 'else.expression' parameter if the condition is false. Here both 'if.expression' and 'else.expression' should be of the same type.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> ifThenElse(<BOOL> condition, <INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> if.expression, <INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> else.expression)
```

QUERY PARAMETERS

| Name            | Description                                                                               | Default Value | Possible Data Types                                               | Optional | Dynamic |
| --------------- | ----------------------------------------------------------------------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| condition       | This specifies the if then else condition value.                                          |               | BOOL                                                              | No       | No      |
| if.expression   | This specifies the value to be returned if the value of the condition parameter is true.  |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |
| else.expression | This specifies the value to be returned if the value of the condition parameter is false. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

Examples

```
@info(name = 'query1')
from sensorEventStream
select sensorValue, ifThenElse(sensorValue>35,'High','Low') as status
insert into outputStream;
```

This will returns High if sensorValue = 50.

EXAMPLE 2

```
@info(name = 'query1')
from sensorEventStream
select sensorValue, ifThenElse(voltage < 5, 0, 1) as status
insert into outputStream;
```

This will returns 1 if voltage= 12.

EXAMPLE 3

```
@info(name = 'query1')
from userEventStream
select userName, ifThenElse(password == 'admin', true, false) as passwordState
insert into outputStream;
```

This will returns passwordState as true if password = admin.

#### instanceOfBoolean [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#instanceofboolean-function" id="instanceofboolean-function"></a>

Checks whether the parameter is an instance of Boolean or not.

_Origin: siddhi-core:5.0.0_

Syntax

```
<BOOL> instanceOfBoolean(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> arg)
```

QUERY PARAMETERS

| Name | Description                  | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ---- | ---------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| arg  | The parameter to be checked. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

Examples

```
from fooStream
select instanceOfBoolean(switchState) as state
insert into barStream;
```

This will return true if the value of switchState is true.

EXAMPLE 2

```
from fooStream
select instanceOfBoolean(value) as state
insert into barStream;
```

if the value = 32 then this will returns false as the value is not an instance of the boolean.

#### instanceOfDouble [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#instanceofdouble-function" id="instanceofdouble-function"></a>

Checks whether the parameter is an instance of Double or not.

_Origin: siddhi-core:5.0.0_

Syntax

```
<BOOL> instanceOfDouble(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> arg)
```

QUERY PARAMETERS

| Name | Description                  | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ---- | ---------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| arg  | The parameter to be checked. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

Examples

```
from fooStream
select instanceOfDouble(value) as state
insert into barStream;
```

This will return true if the value field format is double ex : 56.45.

EXAMPLE 2

```
from fooStream
select instanceOfDouble(switchState) as state
insert into barStream;
```

if the switchState = true then this will returns false as the value is not an instance of the double.

#### instanceOfFloat [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#instanceoffloat-function" id="instanceoffloat-function"></a>

Checks whether the parameter is an instance of Float or not.

_Origin: siddhi-core:5.0.0_

Syntax

```
<BOOL> instanceOfFloat(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> arg)
```

QUERY PARAMETERS

| Name | Description                  | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ---- | ---------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| arg  | The parameter to be checked. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

EXAMPLE 1

```
from fooStream
select instanceOfFloat(value) as state
insert into barStream;
```

This will return true if the value field format is float ex : 56.45f.

EXAMPLE 2

```
from fooStream
select instanceOfFloat(switchState) as state
insert into barStream;
```

if the switchState = true then this will returns false as the value is an instance of the boolean not a float.

#### instanceOfInteger [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#instanceofinteger-function" id="instanceofinteger-function"></a>

Checks whether the parameter is an instance of Integer or not.

_Origin: siddhi-core:5.0.0_

Syntax

```
<BOOL> instanceOfInteger(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> arg)
```

QUERY PARAMETERS

| Name | Description                  | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ---- | ---------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| arg  | The parameter to be checked. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

EXAMPLE 1

```
from fooStream
select instanceOfInteger(value) as state
insert into barStream;
```

This will return true if the value field format is integer.

EXAMPLE 2

```
from fooStream
select instanceOfInteger(switchState) as state
insert into barStream;
```

if the switchState = true then this will returns false as the value is an instance of the boolean not a long.

#### instanceOfLong [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#instanceoflong-function" id="instanceoflong-function"></a>

Checks whether the parameter is an instance of Long or not.

_Origin: siddhi-core:5.0.0_

Syntax

```
<BOOL> instanceOfLong(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> arg)
```

QUERY PARAMETERS

| Name | Description                  | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ---- | ---------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| arg  | The parameter to be checked. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

EXAMPLE 1

```
from fooStream
select instanceOfLong(value) as state
insert into barStream;
```

This will return true if the value field format is long ex : 56456l.

EXAMPLE 2

```
from fooStream
select instanceOfLong(switchState) as state
insert into barStream;
```

if the switchState = true then this will returns false as the value is an instance of the boolean not a long.

#### instanceOfString [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#instanceofstring-function" id="instanceofstring-function"></a>

Checks whether the parameter is an instance of String or not.

_Origin: siddhi-core:5.0.0_

Syntax

```
<BOOL> instanceOfString(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> arg)
```

QUERY PARAMETERS

| Name | Description                  | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ---- | ---------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| arg  | The parameter to be checked. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

EXAMPLE 1

```
from fooStream
select instanceOfString(value) as state
insert into barStream;
```

This will return true if the value field format is string ex : 'test'.

EXAMPLE 2

```
from fooStream
select instanceOfString(switchState) as state
insert into barStream;
```

if the switchState = true then this will returns false as the value is an instance of the boolean not a string.

#### maximum [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#maximum-function" id="maximum-function"></a>

Returns the maximum value of the input parameters.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT> maximum(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                                                                                                                                               | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | This function accepts one or more parameters. They can belong to any one of the available types. All the specified parameters should be of the same type. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

EXAMPLE 1

```
@info(name = 'query1') from inputStream
select maximum(price1, price2, price3) as max
insert into outputStream;
```

This will returns the maximum value of the input parameters price1, price2, price3.

#### minimum [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#minimum-function" id="minimum-function"></a>

Returns the minimum value of the input parameters.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT|LONG|DOUBLE|FLOAT> minimum(<INT|LONG|DOUBLE|FLOAT> arg)
```

QUERY PARAMETERS

| Name | Description                                                                                                                                               | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | This function accepts one or more parameters. They can belong to any one of the available types. All the specified parameters should be of the same type. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT</p> | No       | No      |

EXAMPLE 1

```
@info(name = 'query1') from inputStream
select maximum(price1, price2, price3) as max
insert into outputStream;
```

This will returns the minimum value of the input parameters price1, price2, price3.

#### sizeOfSet [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#sizeofset-function" id="sizeofset-function"></a>

Returns the size of an object of type java.util.Set.

_Origin: siddhi-core:5.0.0_

Syntax

```
<INT> sizeOfSet(<OBJECT> set)
```

QUERY PARAMETERS

| Name | Description                                                                                                                              | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| set  | The set object. This parameter should be of type java.util.Set. A set object may be created by the 'set' attribute aggregator in Siddhi. |               | OBJECT              | No       | No      |

EXAMPLE 1

```
from stockStream 
select initSet(symbol) as initialSet 
insert into initStream; 

;from initStream#window.timeBatch(10 sec) 
select union(initialSet) as distinctSymbols 
insert into distinctStockStream; 

from distinctStockStream 
select sizeOfSet(distinctSymbols) sizeOfSymbolSet 
insert into sizeStream;
```

The sizeStream stream will output the number of distinct stock symbols received during a sliding window of 10 seconds.

#### pol2Cart [_(Stream Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#stream-function) <a href="#pol2cart-stream-function" id="pol2cart-stream-function"></a>

The pol2Cart function calculating the cartesian coordinates x & y for the given theta, rho coordinates and adding them as new attributes to the existing events.

_Origin: siddhi-core:5.0.0_

Syntax

```
pol2Cart(<DOUBLE> theta, <DOUBLE> rho, <DOUBLE> z)
```

QUERY PARAMETERS

| Name  | Description                           | Default Value                                                    | Possible Data Types | Optional | Dynamic |
| ----- | ------------------------------------- | ---------------------------------------------------------------- | ------------------- | -------- | ------- |
| theta | The theta value of the coordinates.   |                                                                  | DOUBLE              | No       | No      |
| rho   | The rho value of the coordinates.     |                                                                  | DOUBLE              | No       | No      |
| z     | z value of the cartesian coordinates. | If z value is not given, drop the third parameter of the output. | DOUBLE              | Yes      | No      |

ExamplesEXAMPLE 1

```
from PolarStream#pol2Cart(theta, rho)
select x, y 
insert into outputStream ;
```

This will return cartesian coordinates (4.99953024681082, 0.06853693328228748) for theta: 0.7854 and rho: 5.

EXAMPLE 2

```
from PolarStream#pol2Cart(theta, rho, 3.4)
select x, y, z 
insert into outputStream ;
```

This will return cartesian coordinates (4.99953024681082, 0.06853693328228748, 3.4)for theta: 0.7854 and rho: 5 and z: 3.4.

#### log [_(Stream Processor)_](https://siddhi.io/en/v5.0/docs/query-guide/#stream-processor) <a href="#log-stream-processor" id="log-stream-processor"></a>

The logger logs the message on the given priority with or without processed event.

_Origin: siddhi-core:5.0.0_

Syntax

```
log(<STRING> priority, <STRING> log.message, <BOOL> is.event.logged)
```

QUERY PARAMETERS

| Name            | Description                                                                          | Default Value | Possible Data Types | Optional | Dynamic |
| --------------- | ------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| priority        | The priority/type of this log message (INFO, DEBUG, WARN, FATAL, ERROR, OFF, TRACE). | INFO          | STRING              | Yes      | No      |
| log.message     | This message will be logged.                                                         |               | STRING              | No       | No      |
| is.event.logged | To log the processed event.                                                          | true          | BOOL                | Yes      | No      |

ExamplesEXAMPLE 1

```
from fooStream#log("INFO", "Sample Event :", true)
select *
insert into barStream;
```

This will log as INFO with the message "Sample Event :" + fooStream:events.

EXAMPLE 2

```
from fooStream#log("Sample Event :", true)
select *
insert into barStream;
```

This will logs with default log level as INFO.

EXAMPLE 3

```
from fooStream#log("Sample Event :", fasle)
select *
insert into barStream;
```

This will only log message.

EXAMPLE 4

```
from fooStream#log(true)
select *
insert into barStream;
```

This will only log fooStream:events.

EXAMPLE 5

```
from fooStream#log("Sample Event :")
select *
insert into barStream;
```

This will log message and fooStream:events.

#### batch [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#batch-window" id="batch-window"></a>

A window that holds an incoming events batch. When a new set of events arrives, the previously arrived old events will be expired. Batch window can be used to aggregate events that comes in batches. If it has the parameter length specified, then batch window process the batch as several chunks.

_Origin: siddhi-core:5.0.0_

Syntax

```
batch(<INT> window.length)
```

QUERY PARAMETERS

| Name          | Description           | Default Value                                                                           | Possible Data Types | Optional | Dynamic |
| ------------- | --------------------- | --------------------------------------------------------------------------------------- | ------------------- | -------- | ------- |
| window.length | The length of a chunk | If length value was not given it assign 0 as length and process the whole batch as once | INT                 | Yes      | No      |

ExamplesEXAMPLE 1

```
define stream consumerItemStream (itemId string, price float)

from consumerItemStream#window.batch()
select price, str:groupConcat(itemId) as itemIds
group by price
insert into outputStream;
```

This will output comma separated items IDs that have the same price for each incoming batch of events.

#### cron [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#cron-window" id="cron-window"></a>

This window outputs the arriving events as and when they arrive, and resets (expires) the window periodically based on the given cron expression.

_Origin: siddhi-core:5.0.0_

Syntax

```
cron(<STRING> cron.expression)
```

QUERY PARAMETERS

| Name            | Description                                 | Default Value | Possible Data Types | Optional | Dynamic |
| --------------- | ------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| cron.expression | The cron expression that resets the window. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputEventStream (symbol string, price float, volume int);

@info(name = 'query1')
from InputEventStream#cron('*/5 * * * * ?')
select symbol, sum(price) as totalPrice 
insert into OutputStream;
```

This let the totalPrice to gradually increase and resets to zero as a batch every 5 seconds.

EXAMPLE 2

```
define stream StockEventStream (symbol string, price float, volume int)
define window StockEventWindow (symbol string, price float, volume int) cron('*/5 * * * * ?');

@info(name = 'query0')
from StockEventStream
insert into StockEventWindow;

@info(name = 'query1')
from StockEventWindow 
select symbol, sum(price) as totalPrice
insert into OutputStream ;
```

The defined window will let the totalPrice to gradually increase and resets to zero as a batch every 5 seconds.

#### delay [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#delay-window" id="delay-window"></a>

A delay window holds events for a specific time period that is regarded as a delay period before processing them.

_Origin: siddhi-core:5.0.0_

Syntax

```
delay(<INT|LONG|TIME> window.delay)
```

QUERY PARAMETERS

| Name         | Description                                                                               | Default Value | Possible Data Types        | Optional | Dynamic |
| ------------ | ----------------------------------------------------------------------------------------- | ------------- | -------------------------- | -------- | ------- |
| window.delay | The time period (specified in sec, min, ms) for which the window should delay the events. |               | <p>INT<br>LONG<br>TIME</p> | No       | No      |

ExamplesEXAMPLE 1

```
define window delayWindow(symbol string, volume int) delay(1 hour);
define stream PurchaseStream(symbol string, volume int);
define stream DeliveryStream(symbol string);
define stream OutputStream(symbol string);

@info(name='query1') 
from PurchaseStream
select symbol, volume
insert into delayWindow;

@info(name='query2') 
from delayWindow join DeliveryStream
on delayWindow.symbol == DeliveryStream.symbol
select delayWindow.symbol
insert into OutputStream;
```

In this example, purchase events that arrive in the 'PurchaseStream' stream are directed to a delay window. At any given time, this delay window holds purchase events that have arrived within the last hour. These purchase events in the window are matched by the 'symbol' attribute, with delivery events that arrive in the 'DeliveryStream' stream. This monitors whether the delivery of products is done with a minimum delay of one hour after the purchase.

#### externalTime [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#externaltime-window" id="externaltime-window"></a>

A sliding time window based on external time. It holds events that arrived during the last windowTime period from the external timestamp, and gets updated on every monotonically increasing timestamp.

_Origin: siddhi-core:5.0.0_

Syntax

```
externalTime(<LONG> timestamp, <INT|LONG|TIME> window.time)
```

QUERY PARAMETERS

| Name        | Description                                                                                                                             | Default Value | Possible Data Types        | Optional | Dynamic |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------- | -------------------------- | -------- | ------- |
| timestamp   | The time which the window determines as current time and will act upon. The value of this parameter should be monotonically increasing. |               | LONG                       | No       | No      |
| window.time | The sliding time period for which the window should hold events.                                                                        |               | <p>INT<br>LONG<br>TIME</p> | No       | No      |

ExamplesEXAMPLE 1

```
define window cseEventWindow (symbol string, price float, volume int) externalTime(eventTime, 20 sec) output expired events;

@info(name = 'query0')
from cseEventStream
insert into cseEventWindow;

@info(name = 'query1')
from cseEventWindow
select symbol, sum(price) as price
insert expired events into outputStream ;
```

processing events arrived within the last 20 seconds from the eventTime and output expired events.

#### externalTimeBatch [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#externaltimebatch-window" id="externaltimebatch-window"></a>

A batch (tumbling) time window based on external time, that holds events arrived during windowTime periods, and gets updated for every windowTime.

_Origin: siddhi-core:5.0.0_

Syntax

```
externalTimeBatch(<LONG> timestamp, <INT|LONG|TIME> window.time, <INT|LONG|TIME> start.time, <INT|LONG|TIME> timeout)
```

QUERY PARAMETERS

| Name        | Description                                                                                                                                                                                                                                | Default Value                                                             | Possible Data Types        | Optional | Dynamic |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------- | -------------------------- | -------- | ------- |
| timestamp   | The time which the window determines as current time and will act upon. The value of this parameter should be monotonically increasing.                                                                                                    |                                                                           | LONG                       | No       | No      |
| window.time | The batch time period for which the window should hold events.                                                                                                                                                                             |                                                                           | <p>INT<br>LONG<br>TIME</p> | No       | No      |
| start.time  | User defined start time. This could either be a constant (of type int, long or time) or an attribute of the corresponding stream (of type long). If an attribute is provided, initial value of attribute would be considered as startTime. | Timestamp of first event                                                  | <p>INT<br>LONG<br>TIME</p> | Yes      | No      |
| timeout     | Time to wait for arrival of new event, before flushing and giving output for events belonging to a specific batch.                                                                                                                         | System waits till an event from next batch arrives to flush current batch | <p>INT<br>LONG<br>TIME</p> | Yes      | No      |

ExamplesEXAMPLE 1

```
define window cseEventWindow (symbol string, price float, volume int) externalTimeBatch(eventTime, 1 sec) output expired events;
@info(name = 'query0')
from cseEventStream
insert into cseEventWindow;
@info(name = 'query1')
from cseEventWindow
select symbol, sum(price) as price
insert expired events into outputStream ;
```

This will processing events that arrive every 1 seconds from the eventTime.

EXAMPLE 2

```
define window cseEventWindow (symbol string, price float, volume int) externalTimeBatch(eventTime, 20 sec, 0) output expired events;
```

This will processing events that arrive every 1 seconds from the eventTime. Starts on 0th millisecond of an hour.

EXAMPLE 3

```
define window cseEventWindow (symbol string, price float, volume int) externalTimeBatch(eventTime, 2 sec, eventTimestamp, 100) output expired events;
```

This will processing events that arrive every 2 seconds from the eventTim. Considers the first event's eventTimestamp value as startTime. Waits 100 milliseconds for the arrival of a new event before flushing current batch.

#### frequent [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#frequent-window" id="frequent-window"></a>

This window returns the latest events with the most frequently occurred value for a given attribute(s). Frequency calculation for this window processor is based on Misra-Gries counting algorithm.

_Origin: siddhi-core:5.0.0_

Syntax

```
frequent(<INT> event.count, <STRING> attribute)
```

QUERY PARAMETERS

| Name        | Description                                                                                                                         | Default Value                                                       | Possible Data Types | Optional | Dynamic |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------- | -------- | ------- |
| event.count | The number of most frequent events to be emitted to the stream.                                                                     |                                                                     | INT                 | No       | No      |
| attribute   | The attributes to group the events. If no attributes are given, the concatenation of all the attributes of the event is considered. | The concatenation of all the attributes of the event is considered. | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@info(name = 'query1')
from purchase[price >= 30]#window.frequent(2)
select cardNo, price
insert all events into PotentialFraud;
```

This will returns the 2 most frequent events.

EXAMPLE 2

```
@info(name = 'query1')
from purchase[price >= 30]#window.frequent(2, cardNo)
select cardNo, price
insert all events into PotentialFraud;
```

This will returns the 2 latest events with the most frequently appeared card numbers.

#### length [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#length-window" id="length-window"></a>

A sliding length window that holds the last 'window.length' events at a given time, and gets updated for each arrival and expiry.

_Origin: siddhi-core:5.0.0_

Syntax

```
length(<INT> window.length)
```

QUERY PARAMETERS

| Name          | Description                                                              | Default Value | Possible Data Types | Optional | Dynamic |
| ------------- | ------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| window.length | The number of events that should be included in a sliding length window. |               | INT                 | No       | No      |

ExamplesEXAMPLE 1

```
define window StockEventWindow (symbol string, price float, volume int) length(10) output all events;

@info(name = 'query0')
from StockEventStream
insert into StockEventWindow;
@info(name = 'query1')

from StockEventWindow
select symbol, sum(price) as price
insert all events into outputStream ;
```

This will process last 10 events in a sliding manner.

#### lengthBatch [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#lengthbatch-window" id="lengthbatch-window"></a>

A batch (tumbling) length window that holds and process a number of events as specified in the window.length.

_Origin: siddhi-core:5.0.0_

Syntax

```
lengthBatch(<INT> window.length, <BOOL> stream.current.event)
```

QUERY PARAMETERS

| Name                 | Description                                                                                                        | Default Value | Possible Data Types | Optional | Dynamic |
| -------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| window.length        | The number of events the window should tumble.                                                                     |               | INT                 | No       | No      |
| stream.current.event | Let the window stream the current events out as and when they arrive to the window while expiring them in batches. | false         | BOOL                | Yes      | No      |

ExamplesEXAMPLE 1

```
define stream InputEventStream (symbol string, price float, volume int);

@info(name = 'query1')
from InputEventStream#lengthBatch(10)
select symbol, sum(price) as price 
insert into OutputStream;
```

This collect and process 10 events as a batch and output them.

EXAMPLE 2

```
define stream InputEventStream (symbol string, price float, volume int);

@info(name = 'query1')
from InputEventStream#lengthBatch(10, true)
select symbol, sum(price) as sumPrice 
insert into OutputStream;
```

This window sends the arriving events directly to the output letting the `sumPrice` to increase gradually, after every 10 events it clears the window as a batch and resets the `sumPrice` to zero.

EXAMPLE 3

```
define stream InputEventStream (symbol string, price float, volume int);
define window StockEventWindow (symbol string, price float, volume int) lengthBatch(10) output all events;

@info(name = 'query0')
from InputEventStream
insert into StockEventWindow;

@info(name = 'query1')
from StockEventWindow
select symbol, sum(price) as price
insert all events into OutputStream ;
```

This uses an defined window to process 10 events as a batch and output all events.

#### lossyFrequent [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#lossyfrequent-window" id="lossyfrequent-window"></a>

This window identifies and returns all the events of which the current frequency exceeds the value specified for the supportThreshold parameter.

_Origin: siddhi-core:5.0.0_

Syntax

```
lossyFrequent(<DOUBLE> support.threshold, <DOUBLE> error.bound, <STRING> attribute)
```

QUERY PARAMETERS

| Name              | Description                                                                                                                         | Default Value                                                       | Possible Data Types | Optional | Dynamic |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------- | -------- | ------- |
| support.threshold | The support threshold value.                                                                                                        |                                                                     | DOUBLE              | No       | No      |
| error.bound       | The error bound value.                                                                                                              |                                                                     | DOUBLE              | No       | No      |
| attribute         | The attributes to group the events. If no attributes are given, the concatenation of all the attributes of the event is considered. | The concatenation of all the attributes of the event is considered. | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
define stream purchase (cardNo string, price float);
define window purchaseWindow (cardNo string, price float) lossyFrequent(0.1, 0.01);
@info(name = 'query0')
from purchase[price >= 30]
insert into purchaseWindow;
@info(name = 'query1')
from purchaseWindow
select cardNo, price
insert all events into PotentialFraud;
```

lossyFrequent(0.1, 0.01) returns all the events of which the current frequency exceeds 0.1, with an error bound of 0.01.

EXAMPLE 2

```
define stream purchase (cardNo string, price float);
define window purchaseWindow (cardNo string, price float) lossyFrequent(0.3, 0.05, cardNo);
@info(name = 'query0')
from purchase[price >= 30]
insert into purchaseWindow;
@info(name = 'query1')
from purchaseWindow
select cardNo, price
insert all events into PotentialFraud;
```

lossyFrequent(0.3, 0.05, cardNo) returns all the events of which the cardNo attributes frequency exceeds 0.3, with an error bound of 0.05.

#### session [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#session-window" id="session-window"></a>

This is a session window that holds events that belong to a specific session. The events that belong to a specific session are identified by a grouping attribute (i.e., a session key). A session gap period is specified to determine the time period after which the session is considered to be expired. A new event that arrives with a specific value for the session key is matched with the session window with the same session key.\
&#x20;There can be out of order and late arrival of events, these events can arrive after the session is expired, to include those events to the matching session key specify a latency time period that is less than the session gap period.To have aggregate functions with session windows, the events need to be grouped by the session key via a 'group by' clause.

_Origin: siddhi-core:5.0.0_

Syntax

```
session(<INT|LONG|TIME> window.session, <STRING> window.key, <INT|LONG|TIME> window.allowedlatency)
```

QUERY PARAMETERS

| Name                  | Description                                                                                                                                                                                                                                | Default Value | Possible Data Types        | Optional | Dynamic |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | -------------------------- | -------- | ------- |
| window.session        | The time period for which the session considered is valid. This is specified in seconds, minutes, or milliseconds (i.e., 'min', 'sec', or 'ms'.                                                                                            |               | <p>INT<br>LONG<br>TIME</p> | No       | No      |
| window.key            | The grouping attribute for events.                                                                                                                                                                                                         | default-key   | STRING                     | Yes      | No      |
| window.allowedlatency | This specifies the time period for which the session window is valid after the expiration of the session. The time period specified here should be less than the session time gap (which is specified via the 'window.session' parameter). | 0             | <p>INT<br>LONG<br>TIME</p> | Yes      | No      |

ExamplesEXAMPLE 1

```
define stream PurchaseEventStream (user string, item_number int, price float, quantity int);

@info(name='query0) 
from PurchaseEventStream#window.session(5 sec, user, 2 sec) 
select * 
insert all events into OutputStream;
```

This query processes events that arrive at the PurchaseEvent input stream. The 'user' attribute is the session key, and the session gap is 5 seconds. '2 sec' is specified as the allowed latency. Therefore, events with the matching user name that arrive 2 seconds after the expiration of the session are also considered when performing aggregations for the session identified by the given user name.

#### sort [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#sort-window" id="sort-window"></a>

This window holds a batch of events that equal the number specified as the windowLength and sorts them in the given order.

_Origin: siddhi-core:5.0.0_

Syntax

```
sort(<INT> window.length, <STRING> attribute, <STRING> order)
```

QUERY PARAMETERS

| Name          | Description                                         | Default Value                                                       | Possible Data Types | Optional | Dynamic |
| ------------- | --------------------------------------------------- | ------------------------------------------------------------------- | ------------------- | -------- | ------- |
| window.length | The size of the window length.                      |                                                                     | INT                 | No       | No      |
| attribute     | The attribute that should be checked for the order. | The concatenation of all the attributes of the event is considered. | STRING              | Yes      | No      |
| order         | The order define as "asc" or "desc".                | asc                                                                 | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
define stream cseEventStream (symbol string, price float, volume long);
define window cseEventWindow (symbol string, price float, volume long) sort(2,volume, 'asc');
@info(name = 'query0')
from cseEventStream
insert into cseEventWindow;
@info(name = 'query1')
from cseEventWindow
select volume
insert all events into outputStream ;
```

sort(5, price, 'asc') keeps the events sorted by price in the ascending order. Therefore, at any given time, the window contains the 5 lowest prices.

#### time [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#time-window" id="time-window"></a>

A sliding time window that holds events that arrived during the last windowTime period at a given time, and gets updated for each event arrival and expiry.

_Origin: siddhi-core:5.0.0_

Syntax

```
time(<INT|LONG|TIME> window.time)
```

QUERY PARAMETERS

| Name        | Description                                                      | Default Value | Possible Data Types        | Optional | Dynamic |
| ----------- | ---------------------------------------------------------------- | ------------- | -------------------------- | -------- | ------- |
| window.time | The sliding time period for which the window should hold events. |               | <p>INT<br>LONG<br>TIME</p> | No       | No      |

ExamplesEXAMPLE 1

```
define window cseEventWindow (symbol string, price float, volume int) time(20) output all events;
@info(name = 'query0')
from cseEventStream
insert into cseEventWindow;
@info(name = 'query1')
from cseEventWindow
select symbol, sum(price) as price
insert all events into outputStream ;
```

This will processing events that arrived within the last 20 milliseconds.

#### timeBatch [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#timebatch-window" id="timebatch-window"></a>

A batch (tumbling) time window that holds and process events that arrive during 'window.time' period as a batch.

_Origin: siddhi-core:5.0.0_

Syntax

```
timeBatch(<INT|LONG|TIME> window.time, <INT> start.time, <BOOL> stream.current.event)
```

QUERY PARAMETERS

| Name                 | Description                                                                                                        | Default Value            | Possible Data Types        | Optional | Dynamic |
| -------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------ | -------------------------- | -------- | ------- |
| window.time          | The batch time period in which the window process the events.                                                      |                          | <p>INT<br>LONG<br>TIME</p> | No       | No      |
| start.time           | This specifies an offset in milliseconds in order to start the window at a time different to the standard time.    | Timestamp of first event | INT                        | Yes      | No      |
| stream.current.event | Let the window stream the current events out as and when they arrive to the window while expiring them in batches. | false                    | BOOL                       | Yes      | No      |

ExamplesEXAMPLE 1

```
define stream InputEventStream (symbol string, price float, volume int);

@info(name = 'query1')
from InputEventStream#timeBatch(20 sec)
select symbol, sum(price) as price 
insert into OutputStream;
```

This collect and process incoming events as a batch every 20 seconds and output them.

EXAMPLE 2

```
define stream InputEventStream (symbol string, price float, volume int);

@info(name = 'query1')
from InputEventStream#timeBatch(20 sec, true)
select symbol, sum(price) as sumPrice 
insert into OutputStream;
```

This window sends the arriving events directly to the output letting the `sumPrice` to increase gradually and on every 20 second interval it clears the window as a batch resetting the `sumPrice` to zero.

EXAMPLE 3

```
define stream InputEventStream (symbol string, price float, volume int);
define window StockEventWindow (symbol string, price float, volume int) timeBatch(20 sec) output all events;

@info(name = 'query0')
from InputEventStream
insert into StockEventWindow;

@info(name = 'query1')
from StockEventWindow
select symbol, sum(price) as price
insert all events into OutputStream ;
```

This uses an defined window to process events arrived every 20 seconds as a batch and output all events.

#### timeLength [_(Window)_](https://siddhi.io/en/v5.0/docs/query-guide/#window) <a href="#timelength-window" id="timelength-window"></a>

A sliding time window that, at a given time holds the last window.length events that arrived during last window.time period, and gets updated for every event arrival and expiry.

_Origin: siddhi-core:5.0.0_

Syntax

```
timeLength(<INT|LONG|TIME> window.time, <INT> window.length)
```

QUERY PARAMETERS

| Name          | Description                                                                  | Default Value | Possible Data Types        | Optional | Dynamic |
| ------------- | ---------------------------------------------------------------------------- | ------------- | -------------------------- | -------- | ------- |
| window.time   | The sliding time period for which the window should hold events.             |               | <p>INT<br>LONG<br>TIME</p> | No       | No      |
| window.length | The number of events that should be be included in a sliding length window.. |               | INT                        | No       | No      |

ExamplesEXAMPLE 1

```
define stream cseEventStream (symbol string, price float, volume int);
define window cseEventWindow (symbol string, price float, volume int) timeLength(2 sec, 10);
@info(name = 'query0')
from cseEventStream
insert into cseEventWindow;
@info(name = 'query1')
from cseEventWindow select symbol, price, volume
insert all events into outputStream;
```

window.timeLength(2 sec, 10) holds the last 10 events that arrived during last 2 seconds and gets updated for every event arrival and expiry.

### Json <a href="#json" id="json"></a>

#### getBool [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#getbool-function" id="getbool-function"></a>

This method returns a 'boolean' value, either 'true' or 'false', based on the valuespecified against the JSON element present in the given path.In case there is no valid boolean value found in the given path, the method still returns 'false'.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<BOOL> json:getBool(<STRING|OBJECT> json, <STRING> path)
```

QUERY PARAMETERS

| Name | Description                                                                            | Default Value | Possible Data Types     | Optional | Dynamic |
| ---- | -------------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json | The JSON input that holds the boolean value in the given path.                         |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path | The path of the input JSON from which the 'getBool' function fetches theboolean value. |               | STRING                  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:getBool(json,"$.name") as name
insert into OutputStream;
```

This returns the boolean value of the JSON input in the given path. The results are directed to the 'OutputStream' stream.

#### getDouble [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#getdouble-function" id="getdouble-function"></a>

This method returns the double value of the JSON element present in the given path. If there is no valid double value in the given path, the method returns 'null'.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<DOUBLE> json:getDouble(<STRING|OBJECT> json, <STRING> path)
```

QUERY PARAMETERS

| Name | Description                                                                             | Default Value | Possible Data Types     | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json | The JSON input that holds the value in the given path.                                  |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path | The path of the input JSON from which the 'getDouble' function fetches thedouble value. |               | STRING                  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:getDouble(json,"$.name") as name
insert into OutputStream;
```

This returns the double value of the given path. The results aredirected to the 'OutputStream' stream.

#### getFloat [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#getfloat-function" id="getfloat-function"></a>

This method returns the float value of the JSON element present in the given path.If there is no valid float value in the given path, the method returns 'null'.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<FLOAT> json:getFloat(<STRING|OBJECT> json, <STRING> path)
```

QUERY PARAMETERS

| Name | Description                                                                     | Default Value | Possible Data Types     | Optional | Dynamic |
| ---- | ------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json | The JSON input that holds the value in the given path.                          |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path | The path of the input JSON from which the 'getFloat' function fetches thevalue. |               | STRING                  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:getFloat(json,"$.name") as name
insert into OutputStream;
```

This returns the float value of the JSON input in the given path. The results aredirected to the 'OutputStream' stream.

#### getInt [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#getint-function" id="getint-function"></a>

This method returns the integer value of the JSON element present in the given path. If there is no valid integer value in the given path, the method returns 'null'.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<INT> json:getInt(<STRING|OBJECT> json, <STRING> path)
```

QUERY PARAMETERS

| Name | Description                                                                           | Default Value | Possible Data Types     | Optional | Dynamic |
| ---- | ------------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json | The JSON input that holds the value in the given path.                                |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path | The path of the input JSON from which the 'getInt' function fetches theinteger value. |               | STRING                  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:getInt(json,"$.name") as name
insert into OutputStream;
```

This returns the integer value of the JSON input in the given path. The resultsare directed to the 'OutputStream' stream.

#### getLong [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#getlong-function" id="getlong-function"></a>

This returns the long value of the JSON element present in the given path. Ifthere is no valid long value in the given path, the method returns 'null'.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<LONG> json:getLong(<STRING|OBJECT> json, <STRING> path)
```

QUERY PARAMETERS

| Name | Description                                                                           | Default Value | Possible Data Types     | Optional | Dynamic |
| ---- | ------------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json | The JSON input that holds the value in the given path.                                |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path | The path of the JSON element from which the 'getLong' functionfetches the long value. |               | STRING                  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:getLong(json,"$.name") as name
insert into OutputStream;
```

This returns the long value of the JSON input in the given path. The results aredirected to 'OutputStream' stream.

#### getObject [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#getobject-function" id="getobject-function"></a>

This returns the object of the JSON element present in the given path.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<OBJECT> json:getObject(<STRING|OBJECT> json, <STRING> path)
```

QUERY PARAMETERS

| Name | Description                                                                       | Default Value | Possible Data Types     | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json | The JSON input that holds the value in the given path.                            |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path | The path of the input JSON from which the 'getObject' function fetches theobject. |               | STRING                  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:getObject(json,"$.name") as name
insert into OutputStream;
```

This returns the object of the JSON input in the given path. The results are directed to the 'OutputStream' stream.

#### getString [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#getstring-function" id="getstring-function"></a>

This returns the string value of the JSON element present in the given path.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<STRING> json:getString(<STRING|OBJECT> json, <STRING> path)
```

QUERY PARAMETERS

| Name | Description                                                                              | Default Value | Possible Data Types     | Optional | Dynamic |
| ---- | ---------------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json | The JSON input that holds the value in the given path.                                   |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path | The path of the JSON input from which the 'getString' function fetches the string value. |               | STRING                  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:getString(json,"$.name") as name
insert into OutputStream;
```

This returns the string value of the JSON input in the given path. The results are directed to the 'OutputStream' stream.

#### isExists [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#isexists-function" id="isexists-function"></a>

This method checks whether there is a JSON element present in the given path or not.If there is a valid JSON element in the given path, it returns 'true'. If there is no valid JSON element, it returns 'false'

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<BOOL> json:isExists(<STRING|OBJECT> json, <STRING> path)
```

QUERY PARAMETERS

| Name | Description                                                                                 | Default Value | Possible Data Types     | Optional | Dynamic |
| ---- | ------------------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json | The JSON input in a given path, on which the function performs the search forJSON elements. |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path | The path that contains the input JSON on which the function performs the search.            |               | STRING                  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:isExists(json,"$.name") as name
insert into OutputStream;
```

This returns either true or false based on the existence of a JSON element in a given path. The results are directed to the 'OutputStream' stream.

#### setElement [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#setelement-function" id="setelement-function"></a>

This method allows to insert elements into a given JSON present in a specific path. If there is no valid path given, it returns the original JSON. Otherwise, it returns the new JSON.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<OBJECT> json:setElement(<STRING|OBJECT> json, <STRING> path, <STRING|BOOL|DOUBLE|FLOAT|INT|LONG|OBJECT> jsonelement, <STRING> key)
```

QUERY PARAMETERS

| Name        | Description                                                             | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ----------- | ----------------------------------------------------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| json        | The JSON input into which is this function inserts the new value.       |               | <p>STRING<br>OBJECT</p>                                           | No       | No      |
| path        | The path on the JSON input which is used to insert the given element.   |               | STRING                                                            | No       | No      |
| jsonelement | The JSON element which is inserted by the function into the input JSON. |               | <p>STRING<br>BOOL<br>DOUBLE<br>FLOAT<br>INT<br>LONG<br>OBJECT</p> | No       | No      |
| key         | The key which is used to insert the given element into the input JSON.  |               | STRING                                                            | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:setElement(json,"$.name") as name
insert into OutputStream;
```

This returns the JSON object present in the given path with the newly inserted JSONelement. The results are directed to the 'OutputStream' stream.

#### toObject [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#toobject-function" id="toobject-function"></a>

This method returns the JSON object related to a given JSON string.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<OBJECT> json:toObject(<STRING> json)
```

QUERY PARAMETERS

| Name | Description                                                            | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ---------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| json | A valid JSON string from which the function generates the JSON object. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:toJson(json) as jsonObject
insert into OutputStream;
```

This returns the JSON object corresponding to the given JSON string.The results aredirected to the 'OutputStream' stream.

#### toString [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#tostring-function" id="tostring-function"></a>

This method returns the JSON string corresponding to a given JSON object.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
<STRING> json:toString(<OBJECT> json)
```

QUERY PARAMETERS

| Name | Description                                                          | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | -------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| json | A valid JSON object from which the function generates a JSON string. |               | OBJECT              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream(json string);
from InputStream
select json:toString(json) as jsonString
insert into OutputStream;
```

This returns the JSON string corresponding to a given JSON object. The results are directed to the 'OutputStream' stream.

#### tokenize [_(Stream Processor)_](https://siddhi.io/en/v5.0/docs/query-guide/#stream-processor) <a href="#tokenize-stream-processor" id="tokenize-stream-processor"></a>

This tokenizes the given json according the path provided

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
json:tokenize(<STRING|OBJECT> json, <STRING> path, <BOOL> fail.on.missing.attribute)
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                | Default Value | Possible Data Types     | Optional | Dynamic |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json                      | The input json that should be tokenized using the given path.                                                                                                                                                                                                              |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path                      | The path that is used to tokenize the given json                                                                                                                                                                                                                           |               | STRING                  | No       | No      |
| fail.on.missing.attribute | If this parameter is set to 'true' and a json is not provided in the given path, the event is dropped. If the parameter is set to 'false', the unavailability of a json in the specified path results in the event being created with a 'null' value for the json element. | true          | BOOL                    | Yes      | No      |

Extra Return Attributes

| Name        | Description                                                      | Possible Types |
| ----------- | ---------------------------------------------------------------- | -------------- |
| jsonElement | The json element retrieved based on the given path and the json. | STRING         |

ExamplesEXAMPLE 1

```
define stream InputStream (json string,path string);
@info(name = 'query1')
from InputStream#json:tokenize(json, path)
select jsonElement
insert into OutputStream;
```

This query performs a tokenization for the given json using the path specified. If the specified path provides a json array, it generates events for each element in that array by adding an additional attributes as the 'jsonElement' to the stream.\
`e.g., jsonInput - {name:"John",enrolledSubjects:["Mathematics","Physics"]}, path - ".enrolledSubjects" </code><br> If we use the configuration in this example, it generates two events with the attributes "Mathematics", "Physics".<br>If the specified path provides a single json element, it adds the specified json element as an additional attribute named 'jsonElement' into the stream. <br><code> e.g., jsonInput - {name:"John",age:25}, path - ".age"`\


#### tokenizeAsObject [_(Stream Processor)_](https://siddhi.io/en/v5.0/docs/query-guide/#stream-processor) <a href="#tokenizeasobject-stream-processor" id="tokenizeasobject-stream-processor"></a>

This tokenizes the given JSON based on the path provided and returns the response as an object.

_Origin: siddhi-execution-json:2.0.0_

Syntax

```
json:tokenizeAsObject(<STRING|OBJECT> json, <STRING> path, <BOOL> fail.on.missing.attribute)
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                | Default Value | Possible Data Types     | Optional | Dynamic |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ----------------------- | -------- | ------- |
| json                      | The input json that is tokenized using the given path.                                                                                                                                                                                                                     |               | <p>STRING<br>OBJECT</p> | No       | No      |
| path                      | The path of the input JSON that the function tokenizes.                                                                                                                                                                                                                    |               | STRING                  | No       | No      |
| fail.on.missing.attribute | If this parameter is set to 'true' and a JSON is not provided in the given path, the event is dropped. If the parameter is set to 'false', the unavailability of a JSON in the specified path results in the event being created with a 'null' value for the json element. | true          | BOOL                    | Yes      | No      |

Extra Return Attributes

| Name        | Description                                                      | Possible Types |
| ----------- | ---------------------------------------------------------------- | -------------- |
| jsonElement | The JSON element retrieved based on the given path and the JSON. | OBJECT         |

ExamplesEXAMPLE 1

```
define stream InputStream (json string,path string);
@info(name = 'query1')
from InputStream#json:tokenizeAsObject(json, path)
select jsonElement
insert into OutputStream;
```

This query performs a tokenization for the given JSON using the path specified. If the specified path provides a JSON array, it generates events for each element in the specified json array by adding an additional attribute as the 'jsonElement' into the stream.\
`e.g., jsonInput - {name:"John",enrolledSubjects:["Mathematics","Physics"]}, path - ".enrolledSubjects" </code><br>If we use the configuration in the above example, it generates two events with the attributes "Mathematics" and "Physics".<br>If the specified path provides a single json element, it adds the specified json element as an additional attribute named 'jsonElement' into the stream <br><code> e.g., jsonInput - {name:"John",age:25}, path - ".age"`\


### Math <a href="#math" id="math"></a>

#### percentile [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#percentile-aggregate-function" id="percentile-aggregate-function"></a>

This functions returns the pth percentile value of a given argument.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:percentile(<INT|LONG|FLOAT|DOUBLE> arg, <DOUBLE> p)
```

QUERY PARAMETERS

| Name | Description                                                                                                                  | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ---------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| arg  | The value of the parameter whose percentile should be found.                                                                 |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |
| p    | Estimate of the percentile to be found (pth percentile) where p is any number greater than 0 or lesser than or equal to 100. |               | DOUBLE                                | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (sensorId int, temperature double); 
from InValueStream 
select math:percentile(temperature, 97.0) as percentile 
insert into OutMediationStream;
```

This function returns the percentile value based on the argument given. For example, math:percentile(temperature, 97.0) returns the 97th percentile value of all the temperature events.

#### abs [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#abs-function" id="abs-function"></a>

This function returns the absolute value of the given parameter. It wraps the `java.lang.Math.abs()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:abs(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                  | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | -------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The parameter whose absolute value is found. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:abs(inValue) as absValue 
insert into OutMediationStream;
```

Irrespective of whether the 'invalue' in the input stream holds a value of abs(3) or abs(-3),the function returns 3 since the absolute value of both 3 and -3 is 3. The result directed to OutMediationStream stream.

#### acos [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#acos-function" id="acos-function"></a>

If -1 <= p1 <= 1, this function returns the arc-cosine (inverse cosine) value of p1.If the domain is invalid, it returns NULL. The value returned is in radian scale. This function wraps the java.lang.Math.acos() function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:acos(<FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                  | Default Value | Possible Data Types    | Optional | Dynamic |
| ---- | ---------------------------------------------------------------------------- | ------------- | ---------------------- | -------- | ------- |
| p1   | The value of the parameter whose arc-cosine (inverse cosine) value is found. |               | <p>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:acos(inValue) as acosValue 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is given, the function calculates the arc-cosine value of it and returns the arc-cosine value to the output stream, OutMediationStream. For example, acos(0.5) returns 1.0471975511965979.

#### asin [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#asin-function" id="asin-function"></a>

If -1 <= p1 <= 1, this function returns the arc-sin (inverse sine) value of p1. If the domain is invalid, it returns NULL. The value returned is in radian scale. This function wraps the java.lang.Math.asin() function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:asin(<FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                             | Default Value | Possible Data Types    | Optional | Dynamic |
| ---- | ----------------------------------------------------------------------- | ------------- | ---------------------- | -------- | ------- |
| p1   | The value of the parameter whose arc-sin (inverse sine) value is found. |               | <p>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:asin(inValue) as asinValue 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is given, the function calculates the arc-sin value of it and returns the arc-sin value to the output stream, OutMediationStream. For example, asin(0.5) returns 0.5235987755982989.

#### atan [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#atan-function" id="atan-function"></a>

1\. If a single `p1` is received, this function returns the arc-tangent (inverse tangent) value of `p1`.\
2\. If `p1` is received along with an optional `p1`, it considers them as x and y coordinates and returns the arc-tangent (inverse tangent) value.\
The returned value is in radian scale. This function wraps the `java.lang.Math.atan()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:atan(<INT|LONG|FLOAT|DOUBLE> p1, <INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                                                                                                       | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose arc-tangent (inverse tangent) is found. If the optional second parameter is given this represents the x coordinate of the (x,y) coordinate pair. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |
| p1   | This optional parameter represents the y coordinate of the (x,y) coordinate pair.                                                                                                 | 0D            | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | Yes      | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue1 double, inValue2 double); 
from InValueStream 
select math:atan(inValue1, inValue2) as convertedValue 
insert into OutMediationStream;
```

If the 'inValue1' in the input stream is given, the function calculates the arc-tangent value of it and returns the arc-tangent value to the output stream, OutMediationStream. If both the 'inValue1' and 'inValue2' are given, then the function considers them to be x and y coordinates respectively and returns the calculated arc-tangent value to the output stream, OutMediationStream. For example, atan(12d, 5d) returns 1.1760052070951352.

#### bin [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#bin-function" id="bin-function"></a>

This function returns a string representation of the p1 argument, that is of either 'integer' or 'long' data type, as an unsigned integer in base 2. It wraps the `java.lang.Integer.toBinaryString` and java.lang.Long.toBinaryString\` methods.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<STRING> math:bin(<INT|LONG> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                           | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ----------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| p1   | The value in either 'integer' or 'long', that should be converted into an unsigned integer of base 2. |               | <p>INT<br>LONG</p>  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue long); 
from InValueStream 
select math:bin(inValue) as binValue 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is given, the function converts it into an unsigned integer in base 2 and directs the output to the output stream, OutMediationStream. For example, bin(9) returns '1001'.

#### cbrt [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#cbrt-function" id="cbrt-function"></a>

This function returns the cube-root of 'p1' which is in radians. It wraps the `java.lang.Math.cbrt()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:cbrt(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                     | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ----------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose cube-root should be found. Input is required to be in radians. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:cbrt(inValue) as cbrtValue 
insert into OutMediationStream;
```

If the 'inValue' is given, the function calculates the cube-root value for the same and directs the output to the output stream, OutMediationStream. For example, cbrt(17d) returns 2.5712815906582356.

#### ceil [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#ceil-function" id="ceil-function"></a>

This function returns the smallest double value, i.e., the closest to the negative infinity, that is greater than or equal to the `p1` argument, and is equal to a mathematical integer. It wraps the `java.lang.Math.ceil()` method.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:ceil(<FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                              | Default Value | Possible Data Types    | Optional | Dynamic |
| ---- | -------------------------------------------------------- | ------------- | ---------------------- | -------- | ------- |
| p1   | The value of the parameter whose ceiling value is found. |               | <p>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:ceil(inValue) as ceilingValue 
insert into OutMediationStream;
```

This function calculates the ceiling value of the given 'inValue' and directs the result to 'OutMediationStream' output stream. For example, ceil(423.187d) returns 424.0.

#### conv [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#conv-function" id="conv-function"></a>

This function converts `a` from the `fromBase` base to the `toBase` base.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<STRING> math:conv(<STRING> a, <INT> from.base, <INT> to.base)
```

QUERY PARAMETERS

| Name      | Description                                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| --------- | ---------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| a         | The value whose base should be changed. Input should be given as a 'String'. |               | STRING              | No       | No      |
| from.base | The source base of the input parameter 'a'.                                  |               | INT                 | No       | No      |
| to.base   | The target base that the input parameter 'a' should be converted into.       |               | INT                 | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue string,fromBase int,toBase int); 
from InValueStream 
select math:conv(inValue,fromBase,toBase) as convertedValue 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is given, and the base in which it currently resides in and the base to which it should be converted to is specified, the function converts it into a string in the target base and directs it to the output stream, OutMediationStream. For example, conv("7f", 16, 10) returns "127".

#### copySign [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#copysign-function" id="copysign-function"></a>

This function returns a value of an input with the received `magnitude` and `sign` of another input. It wraps the `java.lang.Math.copySign()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:copySign(<INT|LONG|FLOAT|DOUBLE> magnitude, <INT|LONG|FLOAT|DOUBLE> sign)
```

QUERY PARAMETERS

| Name      | Description                                                      | Default Value | Possible Data Types                   | Optional | Dynamic |
| --------- | ---------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| magnitude | The magnitude of this parameter is used in the output attribute. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |
| sign      | The sign of this parameter is used in the output attribute.      |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue1 double, inValue2 double); 
from InValueStream 
select math:copySign(inValue1,inValue2) as copysignValue 
insert into OutMediationStream;
```

If two values are provided as 'inValue1' and 'inValue2', the function copies the magnitude and sign of the second argument into the first one and directs the result to the output stream, OutMediatonStream. For example, copySign(5.6d, -3.0d) returns -5.6.

#### cos [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#cos-function" id="cos-function"></a>

This function returns the cosine of `p1` which is in radians. It wraps the `java.lang.Math.cos()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:cos(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                           | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ----------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose cosine value should be found.The input is required to be in radians. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:cos(inValue) as cosValue 
insert into OutMediationStream;
```

If the 'inValue' is given, the function calculates the cosine value for the same and directs the output to the output stream, OutMediationStream. For example, cos(6d) returns 0.9601702866503661.

#### cosh [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#cosh-function" id="cosh-function"></a>

This function returns the hyperbolic cosine of `p1` which is in radians. It wraps the `java.lang.Math.cosh()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:cosh(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                                 | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ----------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose hyperbolic cosine should be found. The input is required to be in radians. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:cosh(inValue) as cosValue 
insert into OutMediationStream;
```

If the 'inValue' is given, the function calculates the hyperbolic cosine value for the same and directs the output to the output stream, OutMediationStream. For example, cosh (6d) returns 201.7156361224559.

#### e [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#e-function" id="e-function"></a>

This function returns the `java.lang.Math.E` constant, which is the closest double value to e, where e is the base of the natural logarithms.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:e()
```

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:e() as eValue 
insert into OutMediationStream;
```

This function returns the constant, 2.7182818284590452354 which is the closest double value to e and directs the output to 'OutMediationStream' output stream.

#### exp [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#exp-function" id="exp-function"></a>

This function returns the Euler's number `e` raised to the power of `p1`. It wraps the `java.lang.Math.exp()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:exp(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                       | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The power that the Euler's number e is raised to. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:exp(inValue) as expValue 
insert into OutMediationStream;
```

If the 'inValue' in the inputstream holds a value, this function calculates the corresponding Euler's number 'e' and directs it to the output stream, OutMediationStream. For example, exp(10.23) returns 27722.51006805505.

#### floor [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#floor-function" id="floor-function"></a>

This function wraps the `java.lang.Math.floor()` function and returns the largest value, i.e., closest to the positive infinity, that is less than or equal to `p1`, and is equal to a mathematical integer.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:floor(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                   | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose floor value should be found. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:floor(inValue) as floorValue 
insert into OutMediationStream;
```

This function calculates the floor value of the given 'inValue' input and directs the output to the 'OutMediationStream' output stream. For example, (10.23) returns 10.0.

#### getExponent [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#getexponent-function" id="getexponent-function"></a>

This function returns the unbiased exponent that is used in the representation of `p1`. This function wraps the `java.lang.Math.getExponent()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<INT> math:getExponent(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                          | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | -------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of whose unbiased exponent representation should be found. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:getExponent(inValue) as expValue 
insert into OutMediationStream;
```

This function calculates the unbiased exponent of a given input, 'inValue' and directs the result to the 'OutMediationStream' output stream. For example, getExponent(60984.1) returns 15.

#### hex [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#hex-function" id="hex-function"></a>

This function wraps the `java.lang.Double.toHexString() function. It returns a hexadecimal string representation of the input,` p1\`.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<STRING> math:hex(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                         | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose hexadecimal value should be found. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue int); 
from InValueStream 
select math:hex(inValue) as hexString 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is provided, the function converts this into its corresponding hexadecimal format and directs the output to the output stream, OutMediationStream. For example, hex(200) returns "c8".

#### isInfinite [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#isinfinite-function" id="isinfinite-function"></a>

This function wraps the `java.lang.Float.isInfinite()` and `java.lang.Double.isInfinite()` and returns `true` if `p1` is infinitely large in magnitude and `false` if otherwise.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<BOOL> math:isInfinite(<FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                      | Default Value | Possible Data Types    | Optional | Dynamic |
| ---- | ------------------------------------------------------------------------------------------------ | ------------- | ---------------------- | -------- | ------- |
| p1   | This is the value of the parameter that the function determines to be either infinite or finite. |               | <p>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue1 double,inValue2 int); 
from InValueStream 
select math:isInfinite(inValue1) as isInfinite 
insert into OutMediationStream;
```

If the value given in the 'inValue' in the input stream is of infinitely large magnitude, the function returns the value, 'true' and directs the result to the output stream, OutMediationStream'. For example, isInfinite(java.lang.Double.POSITIVE\_INFINITY) returns true.

#### isNan [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#isnan-function" id="isnan-function"></a>

This function wraps the `java.lang.Float.isNaN()` and `java.lang.Double.isNaN()` functions and returns `true` if `p1` is NaN (Not-a-Number), and returns `false` if otherwise.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<BOOL> math:isNan(<FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                            | Default Value | Possible Data Types    | Optional | Dynamic |
| ---- | -------------------------------------------------------------------------------------- | ------------- | ---------------------- | -------- | ------- |
| p1   | The value of the parameter which the function determines to be either NaN or a number. |               | <p>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue1 double,inValue2 int); 
from InValueStream 
select math:isNan(inValue1) as isNaN 
insert into OutMediationStream;
```

If the 'inValue1' in the input stream has a value that is undefined, then the function considers it as an 'NaN' value and directs 'True' to the output stream, OutMediationStream. For example, isNan(java.lang.Math.log(-12d)) returns true.

#### ln [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#ln-function" id="ln-function"></a>

This function returns the natural logarithm (base e) of `p1`.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:ln(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                  | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ---------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose natural logarithm (base e) should be found. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:ln(inValue) as lnValue 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is given, the function calculates its natural logarithm (base e) and directs the results to the output stream, 'OutMeditionStream'. For example, ln(11.453) returns 2.438251704415579.

#### log [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#log-function" id="log-function"></a>

This function returns the logarithm of the received `number` as per the given `base`.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:log(<INT|LONG|FLOAT|DOUBLE> number, <INT|LONG|FLOAT|DOUBLE> base)
```

QUERY PARAMETERS

| Name   | Description                                              | Default Value | Possible Data Types                   | Optional | Dynamic |
| ------ | -------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| number | The value of the parameter whose base should be changed. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |
| base   | The base value of the ouput.                             |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (number double, base double); 
from InValueStream 
select math:log(number, base) as logValue 
insert into OutMediationStream;
```

If the number and the base to which it has to be converted into is given in the input stream, the function calculates the number to the base specified and directs the result to the output stream, OutMediationStream. For example, log(34, 2f) returns 5.08746284125034.

#### log10 [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#log10-function" id="log10-function"></a>

This function returns the base 10 logarithm of `p1`.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:log10(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                         | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose base 10 logarithm should be found. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:log10(inValue) as lnValue 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is given, the function calculates the base 10 logarithm of the same and directs the result to the output stream, OutMediatioStream. For example, log10(19.234) returns 1.2840696117100832.

#### log2 [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#log2-function" id="log2-function"></a>

This function returns the base 2 logarithm of `p1`.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:log2(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                        | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------------ | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose base 2 logarithm should be found. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:log2(inValue) as lnValue 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is given, the function calculates the base 2 logarithm of the same and returns the value to the output stream, OutMediationStream. For example log2(91d) returns 6.507794640198696.

#### max [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#max-function" id="max-function"></a>

This function returns the greater value of `p1` and `p2`.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:max(<INT|LONG|FLOAT|DOUBLE> p1, <INT|LONG|FLOAT|DOUBLE> p2)
```

QUERY PARAMETERS

| Name | Description                                                                            | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | -------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | One of the input values to be compared in order to find the larger value of the two    |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |
| p2   | The input value to be compared with 'p1' in order to find the larger value of the two. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue1 double,inValue2 int); 
from InValueStream 
select math:max(inValue1,inValue2) as maxValue 
insert into OutMediationStream;
```

If two input values 'inValue1, and 'inValue2' are given, the function compares them and directs the larger value to the output stream, OutMediationStream. For example, max(123.67d, 91) returns 123.67.

#### min [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#min-function" id="min-function"></a>

This function returns the smaller value of `p1` and `p2`.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:min(<INT|LONG|FLOAT|DOUBLE> p1, <INT|LONG|FLOAT|DOUBLE> p2)
```

QUERY PARAMETERS

| Name | Description                                                                          | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------------------------------ | ------------- | ------------------------------------- | -------- | ------- |
| p1   | One of the input values that are to be compared in order to find the smaller value.  |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |
| p2   | The input value that is to be compared with 'p1' in order to find the smaller value. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue1 double,inValue2 int); 
from InValueStream 
select math:min(inValue1,inValue2) as minValue 
insert into OutMediationStream;
```

If two input values, 'inValue1' and 'inValue2' are given, the function compares them and directs the smaller value of the two to the output stream, OutMediationStream. For example, min(123.67d, 91) returns 91.

#### oct [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#oct-function" id="oct-function"></a>

This function converts the input parameter `p1` to octal.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<STRING> math:oct(<INT|LONG> p1)
```

QUERY PARAMETERS

| Name | Description                                                            | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ---------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| p1   | The value of the parameter whose octal representation should be found. |               | <p>INT<br>LONG</p>  | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue long); 
from InValueStream 
select math:oct(inValue) as octValue 
insert into OutMediationStream;
```

If the 'inValue' in the input stream is given, this function calculates the octal value corresponding to the same and directs it to the output stream, OutMediationStream. For example, oct(99l) returns "143".

#### parseDouble [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#parsedouble-function" id="parsedouble-function"></a>

This function returns the double value of the string received.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:parseDouble(<STRING> p1)
```

QUERY PARAMETERS

| Name | Description                                             | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| p1   | The value that should be converted into a double value. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue string); 
from InValueStream 
select math:parseDouble(inValue) as output 
insert into OutMediationStream;
```

If the 'inValue' in the input stream holds a value, this function converts it into the corresponding double value and directs it to the output stream, OutMediationStream. For example, parseDouble("123") returns 123.0.

#### parseFloat [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#parsefloat-function" id="parsefloat-function"></a>

This function returns the float value of the received string.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<FLOAT> math:parseFloat(<STRING> p1)
```

QUERY PARAMETERS

| Name | Description                                            | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| p1   | The value that should be converted into a float value. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue string); 
from InValueStream 
select math:parseFloat(inValue) as output 
insert into OutMediationStream;
```

The function converts the input value given in 'inValue',into its corresponding float value and directs the result into the output stream, OutMediationStream. For example, parseFloat("123") returns 123.0.

#### parseInt [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#parseint-function" id="parseint-function"></a>

This function returns the integer value of the received string.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<INT> math:parseInt(<STRING> p1)
```

QUERY PARAMETERS

| Name | Description                                       | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | ------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| p1   | The value that should be converted to an integer. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue string); 
from InValueStream 
select math:parseInt(inValue) as output 
insert into OutMediationStream;
```

The function converts the 'inValue' into its corresponding integer value and directs the output to the output stream, OutMediationStream. For example, parseInt("123") returns 123.

#### parseLong [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#parselong-function" id="parselong-function"></a>

This function returns the long value of the string received.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<LONG> math:parseLong(<STRING> p1)
```

QUERY PARAMETERS

| Name | Description                                         | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | --------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| p1   | The value that should be converted to a long value. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue string); 
from InValueStream 
select math:parseLong(inValue) as output 
insert into OutMediationStream;
```

The function converts the 'inValue' to its corresponding long value and directs the result to the output stream, OutMediationStream. For example, parseLong("123") returns 123.

#### pi [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#pi-function" id="pi-function"></a>

This function returns the `java.lang.Math.PI` constant, which is the closest value to pi, i.e., the ratio of the circumference of a circle to its diameter.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:pi()
```

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:pi() as piValue 
insert into OutMediationStream;
```

pi() always returns 3.141592653589793.

#### power [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#power-function" id="power-function"></a>

This function raises the given value to a given power.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:power(<INT|LONG|FLOAT|DOUBLE> value, <INT|LONG|FLOAT|DOUBLE> to.power)
```

QUERY PARAMETERS

| Name     | Description                                                                 | Default Value | Possible Data Types                   | Optional | Dynamic |
| -------- | --------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| value    | The value that should be raised to the power of 'to.power' input parameter. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |
| to.power | The power to which the 'value' input parameter should be raised.            |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue1 double, inValue2 double); 
from InValueStream 
select math:power(inValue1,inValue2) as powerValue 
insert into OutMediationStream;
```

This function raises the 'inValue1' to the power of 'inValue2' and directs the output to the output stream, 'OutMediationStream. For example, (5.6d, 3.0d) returns 175.61599999999996.

#### rand [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#rand-function" id="rand-function"></a>

This returns a stream of pseudo-random numbers when a sequence of calls are sent to the `rand()`. Optionally, it is possible to define a seed, i.e., `rand(seed)` using which the pseudo-random numbers are generated. These functions internally use the `java.util.Random` class.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:rand(<INT|LONG> seed)
```

QUERY PARAMETERS

| Name | Description                                                                      | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | -------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| seed | An optional seed value that will be used to generate the random number sequence. | defaultSeed   | <p>INT<br>LONG</p>  | Yes      | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (symbol string, price long, volume long); 
from InValueStream select symbol, math:rand() as randNumber 
select math:oct(inValue) as octValue 
insert into OutMediationStream;
```

In the example given above, a random double value between 0 and 1 will be generated using math:rand().

#### round [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#round-function" id="round-function"></a>

This function returns the value of the input argument rounded off to the closest integer/long value.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<INT|LONG> math:round(<FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                             | Default Value | Possible Data Types    | Optional | Dynamic |
| ---- | ----------------------------------------------------------------------- | ------------- | ---------------------- | -------- | ------- |
| p1   | The value that should be rounded off to the closest integer/long value. |               | <p>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:round(inValue) as roundValue 
insert into OutMediationStream;
```

The function rounds off 'inValue1' to the closest int/long value and directs the output to the output stream, 'OutMediationStream'. For example, round(3252.353) returns 3252.

#### signum [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#signum-function" id="signum-function"></a>

This returns +1, 0, or -1 for the given positive, zero and negative values respectively. This function wraps the `java.lang.Math.signum()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<INT> math:signum(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                        | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------------ | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value that should be checked to be positive, negative or zero. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:signum(inValue) as sign 
insert into OutMediationStream;
```

The function evaluates the 'inValue' given to be positive, negative or zero and directs the result to the output stream, 'OutMediationStream'. For example, signum(-6.32d) returns -1.

#### sin [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#sin-function" id="sin-function"></a>

This returns the sine of the value given in radians. This function wraps the `java.lang.Math.sin()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:sin(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                      | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------------------------------------------ | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose sine value should be found. Input is required to be in radians. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:sin(inValue) as sinValue 
insert into OutMediationStream;
```

The function calculates the sine value of the given 'inValue' and directs the output to the output stream, 'OutMediationStream. For example, sin(6d) returns -0.27941549819892586.

#### sinh [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#sinh-function" id="sinh-function"></a>

This returns the hyperbolic sine of the value given in radians. This function wraps the `java.lang.Math.sinh()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:sinh(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                                 | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ----------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose hyperbolic sine value should be found. Input is required to be in radians. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:sinh(inValue) as sinhValue 
insert into OutMediationStream;
```

This function calculates the hyperbolic sine value of 'inValue' and directs the output to the output stream, 'OutMediationStream'. For example, sinh(6d) returns 201.71315737027922.

#### sqrt [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#sqrt-function" id="sqrt-function"></a>

This function returns the square-root of the given value. It wraps the `java.lang.Math.sqrt()`s function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:sqrt(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                         | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose square-root value should be found. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:sqrt(inValue) as sqrtValue 
insert into OutMediationStream;
```

The function calculates the square-root value of the 'inValue' and directs the output to the output stream, 'OutMediationStream'. For example, sqrt(4d) returns 2.

#### tan [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#tan-function" id="tan-function"></a>

This function returns the tan of the given value in radians. It wraps the `java.lang.Math.tan()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:tan(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                     | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | ----------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose tan value should be found. Input is required to be in radians. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:tan(inValue) as tanValue 
insert into OutMediationStream;
```

This function calculates the tan value of the 'inValue' given and directs the output to the output stream, 'OutMediationStream'. For example, tan(6d) returns -0.29100619138474915.

#### tanh [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#tanh-function" id="tanh-function"></a>

This function returns the hyperbolic tangent of the value given in radians. It wraps the `java.lang.Math.tanh()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:tanh(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                                                                    | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | -------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The value of the parameter whose hyperbolic tangent value should be found. Input is required to be in radians. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:tanh(inValue) as tanhValue 
insert into OutMediationStream;
```

If the 'inVaue' in the input stream is given, this function calculates the hyperbolic tangent value of the same and directs the output to 'OutMediationStream' stream. For example, tanh(6d) returns 0.9999877116507956.

#### toDegrees [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#todegrees-function" id="todegrees-function"></a>

This function converts the value given in radians to degrees. It wraps the `java.lang.Math.toDegrees()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:toDegrees(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                     | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | --------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The input value in radians that should be converted to degrees. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:toDegrees(inValue) as degreesValue 
insert into OutMediationStream;
```

The function converts the 'inValue' in the input stream from radians to degrees and directs the output to 'OutMediationStream' output stream. For example, toDegrees(6d) returns 343.77467707849394.

#### toRadians [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#toradians-function" id="toradians-function"></a>

This function converts the value given in degrees to radians. It wraps the `java.lang.Math.toRadians()` function.

_Origin: siddhi-execution-math:5.0.0_

Syntax

```
<DOUBLE> math:toRadians(<INT|LONG|FLOAT|DOUBLE> p1)
```

QUERY PARAMETERS

| Name | Description                                                     | Default Value | Possible Data Types                   | Optional | Dynamic |
| ---- | --------------------------------------------------------------- | ------------- | ------------------------------------- | -------- | ------- |
| p1   | The input value in degrees that should be converted to radians. |               | <p>INT<br>LONG<br>FLOAT<br>DOUBLE</p> | No       | No      |

ExamplesEXAMPLE 1

```
define stream InValueStream (inValue double); 
from InValueStream 
select math:toRadians(inValue) as radiansValue 
insert into OutMediationStream;
```

This function converts the input, from degrees to radians and directs the result to 'OutMediationStream' output stream. For example, toRadians(6d) returns 0.10471975511965977.

### Rdbms <a href="#rdbms" id="rdbms"></a>

#### cud [_(Stream Processor)_](https://siddhi.io/en/v5.0/docs/query-guide/#stream-processor) <a href="#cud-stream-processor" id="cud-stream-processor"></a>

This function performs SQL CUD (INSERT, UPDATE, DELETE) queries on WSO2 datasources.\
Note: This function is only available when running Siddhi with WSO2 SP.\


_Origin: siddhi-store-rdbms:6.0.0_

Syntax

```
rdbms:cud(<STRING> datasource.name, <STRING> query, <STRING> parameter.n)
```

QUERY PARAMETERS

| Name            | Description                                                                                                                   | Default Value | Possible Data Types | Optional | Dynamic |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| datasource.name | The name of the WSO2 datasource for which the query should be performed.                                                      |               | STRING              | No       | No      |
| query           | The update, delete, or insert query(formatted according to the relevant database type) that needs to be performed.            |               | STRING              | No       | No      |
| parameter.n     | If the second parameter is a parametrised SQL query, then siddhi attributes can be passed to set the values of the parameters |               | STRING              | No       | No      |

System Parameters

| Name                   | Description                                                                                      | Default Value | Possible Parameters  |
| ---------------------- | ------------------------------------------------------------------------------------------------ | ------------- | -------------------- |
| perform.CUD.operations | If this parameter is set to 'true', the RDBMS CUD function is enabled to perform CUD operations. | false         | <p>true<br>false</p> |

Extra Return Attributes

| Name       | Description                                     | Possible Types |
| ---------- | ----------------------------------------------- | -------------- |
| numRecords | The number of records manipulated by the query. | INT            |

ExamplesEXAMPLE 1

```
from TriggerStream#rdbms:cud("SAMPLE_DB", "UPDATE Customers_Table SET customerName='abc' where customerName='xyz'") 
select numRecords 
insert into  RecordStream;
```

This query updates the events from the input stream named 'TriggerStream' with an additional attribute named 'numRecords', of which the value indicates the number of records manipulated. The updated events are inserted into an output stream named 'RecordStream'.

EXAMPLE 2

```
from TriggerStream#rdbms:cud("SAMPLE_DB", "UPDATE Customers_Table SET customerName=? where customerName=?", changedName, previousName) 
select numRecords 
insert into  RecordStream;
```

This query updates the events from the input stream named 'TriggerStream' with an additional attribute named 'numRecords', of which the value indicates the number of records manipulated. The updated events are inserted into an output stream named 'RecordStream'. Here the values of attributes changedName and previousName in the event will be set to the query.

#### query [_(Stream Processor)_](https://siddhi.io/en/v5.0/docs/query-guide/#stream-processor) <a href="#query-stream-processor" id="query-stream-processor"></a>

This function performs SQL retrieval queries on WSO2 datasources.\
Note: This function is only available when running Siddhi with WSO2 SP.

_Origin: siddhi-store-rdbms:6.0.0_

Syntax

```
rdbms:query(<STRING> datasource.name, <STRING> query, <STRING> parameter.n, <STRING> attribute.definition.list)
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| datasource.name           | The name of the WSO2 datasource for which the query should be performed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |               | STRING              | No       | No      |
| query                     | The select query(formatted according to the relevant database type) that needs to be performed                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |               | STRING              | No       | No      |
| parameter.n               | If the second parameter is a parametrised SQL query, then siddhi attributes can be passed to set the values of the parameters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |               | STRING              | No       | No      |
| attribute.definition.list | <p>This is provided as a comma-separated list in the '&#x3C;AttributeName AttributeType>' format. The SQL query is expected to return the attributes in the given order. e.g., If one attribute is defined here, the SQL query should return one column result set. If more than one column is returned, then the first column is processed. The Siddhi data types supported are 'STRING', 'INT', 'LONG', 'DOUBLE', 'FLOAT', and 'BOOL'.<br> Mapping of the Siddhi data type to the database data type can be done as follows,<br><em>Siddhi Datatype</em>-><em>Datasource Datatype</em><br><code>STRING</code>-><code>CHAR</code>,<code>VARCHAR</code>,<code>LONGVARCHAR</code><br><code>INT</code>      -><code>INTEGER</code><br><code>LONG</code>    -><code>BIGINT</code><br><code>DOUBLE</code>-><code>DOUBLE</code><br><code>FLOAT</code>   -><code>REAL</code><br><code>BOOL</code>     -><code>BIT</code><br></p> |               | STRING              | No       | No      |

Extra Return Attributes

| Name          | Description                                                                                 | Possible Types                                          |
| ------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| attributeName | The return attributes will be the ones defined in the parameter`attribute.definition.list`. | <p>STRING<br>INT<br>LONG<br>DOUBLE<br>FLOAT<br>BOOL</p> |

ExamplesEXAMPLE 1

```
from TriggerStream#rdbms:query('SAMPLE_DB', 'select * from Transactions_Table', 'creditcardno string, country string, transaction string, amount int') 
select creditcardno, country, transaction, amount 
insert into recordStream;
```

Events inserted into recordStream includes all records matched for the query i.e an event will be generated for each record retrieved from the datasource. The event will include as additional attributes, the attributes defined in the `attribute.definition.list`(creditcardno, country, transaction, amount).

EXAMPLE 2

```
from TriggerStream#rdbms:query('SAMPLE_DB', 'select * from where country=? ', countrySearchWord, 'creditcardno string, country string, transaction string, amount int') 
select creditcardno, country, transaction, amount 
insert into recordStream;
```

Events inserted into recordStream includes all records matched for the query i.e an event will be generated for each record retrieved from the datasource. The event will include as additional attributes, the attributes defined in the `attribute.definition.list`(creditcardno, country, transaction, amount). countrySearchWord value from the event will be set in the query when querying the datasource.

### Regex <a href="#regex" id="regex"></a>

#### find [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#find-function" id="find-function"></a>

These methods attempt to find the subsequence of the 'inputSequence' that matches the given 'regex' pattern.

_Origin: siddhi-execution-regex:5.0.0_

Syntax

```
<BOOL> regex:find(<STRING> regex, <STRING> input.sequence, <INT> starting.index)
```

QUERY PARAMETERS

| Name           | Description                                                                                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| regex          | A regular expression that is matched to a sequence in order to find the subsequence of the same. For example, \d\d(.\*)WSO2. |               | STRING              | No       | No      |
| input.sequence | The input sequence to be matched with the regular expression. For example, 21 products are produced by WSO2.                 |               | STRING              | No       | No      |
| starting.index | The starting index of the input sequence from where the input sequence ismatched with the given regex pattern. eg: 1, 2.     |               | INT                 | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream (inputSequence string, price long, regex string);

from InputStream select inputSequence , regex:find(\d\d(.*)WSO2, 21 products are produced by WSO2 currently) as aboutWSO2 insert into OutputStream;
```

This method attempts to find the subsequence of the 'inputSequence' that matches the regex pattern, \d\d(.\*)WSO2. It returns true as a subsequence exists.

EXAMPLE 2

```
define stream InputStream (inputSequence string, price long, regex string);

from InputStream select inputSequence , regex:find(\d\d(.*)WSO2, 21 products are produced currently) as aboutWSO2 insert into OutputStream;
```

This method attempts to find the subsequence of the 'inputSequence' that matches the regex pattern, \d\d(.\*)WSO2 . It returns 'false' as a subsequence does not exist.

EXAMPLE 3

```
define stream InputStream (inputSequence string, price long, regex string);

from InputStream select inputSequence , regex:find(\d\d(.*)WSO2, 21 products are produced within 10 years by WSO2 currently by WSO2 employees, 30) as aboutWSO2 insert into OutputStream;
```

This method attempts to find the subsequence of the 'inputSequence' that matches the regex pattern, \d\d(.\*)WSO2 starting from index 30. It returns 'true' since a subsequence exists.

#### group [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#group-function" id="group-function"></a>

This method returns the input subsequence captured by the given group during the previous match operation.

_Origin: siddhi-execution-regex:5.0.0_

Syntax

```
<STRING> regex:group(<STRING> regex, <STRING> input.sequence, <INT> group.id)
```

QUERY PARAMETERS

| Name           | Description                                                                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| -------------- | ------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| regex          | A regular expression. For example, \d\d(.\*)WSO2.                                                            |               | STRING              | No       | No      |
| input.sequence | The input sequence to be matched with the regular expression. For example, 21 products are produced by WSO2. |               | STRING              | No       | No      |
| group.id       | The given group id of the regex expression. For example, 0, 1, 2, etc.                                       |               | INT                 | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream (inputSequence string, price long, regex string, group int);

from InputStream select inputSequence, regex:group(\d\d(.*)(WSO2.*), 21 products are produced within 10 years by WSO2 currently by WSO2 employees, 3) 
 insert into OutputStream;
```

This function returns 'WSO2 employees', the input subsequence captured within the given groupID, 3 after grouping the 'inputSequence' according to the regex pattern, \d\d(._)(WSO2._).

#### lookingAt [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#lookingat-function" id="lookingat-function"></a>

This method attempts to match the 'inputSequence', from the beginning, against the 'regex' pattern.

_Origin: siddhi-execution-regex:5.0.0_

Syntax

```
<BOOL> regex:lookingAt(<STRING> regex, <STRING> input.sequence)
```

QUERY PARAMETERS

| Name           | Description                                                                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| -------------- | ------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| regex          | A regular expression. For example, \d\d(.\*)WSO2.                                                            |               | STRING              | No       | No      |
| input.sequence | The input sequence to be matched with the regular expression. For example, 21 products are produced by WSO2. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream (inputSequence string, price long, regex string, group int);

from InputStream select inputSequence, regex:lookingAt(\d\d(.*)(WSO2.*), 21 products are produced by WSO2 currently in Sri Lanka)
```

This method attempts to match the 'inputSequence' against the regex pattern, \d\d(._)(WSO2._) from the beginning. Since it matches, the function returns 'true'.

EXAMPLE 2

```
define stream InputStream (inputSequence string, price long, regex string, group int);

from InputStream select inputSequence, regex:lookingAt(WSO2(.*)middleware(.*), sample test string and WSO2 is situated in trace and it's a middleware company)
```

This method attempts to match the 'inputSequence' against the regex pattern, WSO2(._)middleware(._) from the beginning. Since it does not match, the function returns false.

#### matches [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#matches-function" id="matches-function"></a>

This method attempts to match the entire 'inputSequence' against the 'regex' pattern.

_Origin: siddhi-execution-regex:5.0.0_

Syntax

```
<BOOL> regex:matches(<STRING> regex, <STRING> input.sequence)
```

QUERY PARAMETERS

| Name           | Description                                                                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| -------------- | ------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| regex          | A regular expression. For example, \d\d(.\*)WSO2.                                                            |               | STRING              | No       | No      |
| input.sequence | The input sequence to be matched with the regular expression. For example, 21 products are produced by WSO2. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
define stream InputStream (inputSequence string, price long, regex string, group int);

from InputStream select inputSequence, regex:matches(WSO2(.*)middleware(.*), WSO2 is situated in trace and its a middleware company)
```

This method attempts to match the entire 'inputSequence' against WSO2(._)middleware(._) regex pattern. Since it matches, it returns 'true'.

EXAMPLE 2

```
define stream inputStream (inputSequence string, price long, regex string, group int);

from inputStream select inputSequence, regex:matches(WSO2(.*)middleware, WSO2 is situated in trace and its a middleware company)
```

This method attempts to match the entire 'inputSequence' against WSO2(.\*)middleware regex pattern. Since it does not match, it returns 'false'.

### Sink <a href="#sink" id="sink"></a>

#### email [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#email-sink" id="email-sink"></a>

The email sink uses the 'smtp' server to publish events via emails. The events can be published in 'text', 'xml' or 'json' formats. The user can define email sink parameters in either the '\<SP\_HOME>/conf/\<PROFILE>/deployment yaml' file or in the stream definition. The email sink first checks the stream definition for parameters, and if they are no configured there, it checks the 'deployment.yaml' file. If the parameters are not configured in either place, default values are considered for optional parameters. If you need to configure server system parameters that are not provided as options in the stream definition, then those parameters need to be defined them in the 'deployment.yaml' file under 'email sink properties'.\
\
For more information about the SMTP server parameters, see https://javaee.github.io/javamail/SMTP-Transport.\
\
Further, some email accounts are required to enable the 'access to less secure apps' option. For gmail accounts, you can enable this option via https://myaccount.google.com/lesssecureapps.

_Origin: siddhi-io-email:2.0.1_

Syntax

```
@sink(type="email", username="<STRING>", address="<STRING>", password="<STRING>", host="<STRING>", port="<INT>", ssl.enable="<BOOL>", auth="<BOOL>", content.type="<STRING>", subject="<STRING>", to="<STRING>", cc="<STRING>", bcc="<STRING>", attachments="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name         | Description                                                                                                                                                                                                                                                                                   | Default Value                                            | Possible Data Types | Optional | Dynamic |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- | ------------------- | -------- | ------- |
| username     | <p>The username of the email account that is used to send emails.<br>e.g., 'abc' is the username of the 'abc@gmail.com' account.</p>                                                                                                                                                          |                                                          | STRING              | No       | No      |
| address      | The address of the email account that is used to send emails.                                                                                                                                                                                                                                 |                                                          | STRING              | No       | No      |
| password     | The password of the email account.                                                                                                                                                                                                                                                            |                                                          | STRING              | No       | No      |
| host         | The host name of the SMTP server. e.g., 'smtp.gmail.com' is a host name for a gmail account. The default value 'smtp.gmail.com' is only valid if the email account is a gmail account.                                                                                                        | smtp.gmail.com                                           | STRING              | Yes      | No      |
| port         | The port that is used to create the connection.                                                                                                                                                                                                                                               | '465' the default value is only valid is SSL is enabled. | INT                 | Yes      | No      |
| ssl.enable   | This parameter specifies whether the connection should be established via a secure connection or not. The value can be either 'true' or 'false'. If it is 'true', then the connection is establish via the 493 port which is a secure connection.                                             | true                                                     | BOOL                | Yes      | No      |
| auth         | This parameter specifies whether to use the 'AUTH' command when authenticating or not. If the parameter is set to 'true', an attempt is made to authenticate the user using the 'AUTH' command.                                                                                               | true                                                     | BOOL                | Yes      | No      |
| content.type | The content type can be either 'text/plain' or 'text/html'.                                                                                                                                                                                                                                   | text/plain                                               | STRING              | Yes      | No      |
| subject      | The subject of the mail to be send.                                                                                                                                                                                                                                                           |                                                          | STRING              | No       | Yes     |
| to           | The address of the 'to' recipient. If there are more than one 'to' recipients, then all the required addresses can be given as a comma-separated list.                                                                                                                                        |                                                          | STRING              | No       | Yes     |
| cc           | The address of the 'cc' recipient. If there are more than one 'cc' recipients, then all the required addresses can be given as a comma-separated list.                                                                                                                                        | None                                                     | STRING              | Yes      | No      |
| bcc          | The address of the 'bcc' recipient. If there are more than one 'bcc' recipients, then all the required addresses can be given as a comma-separated list.                                                                                                                                      | None                                                     | STRING              | Yes      | No      |
| attachments  | <p>File paths of the files that need to be attached to the email.<br>These paths should be absolute paths.<br>They can be either directories or files<br>. If the path is to a directory, all the files located at the first level (i.e., not within another sub directory) are attached.</p> | None                                                     | STRING              | Yes      | Yes     |

System Parameters

| Name                              | Description                                                                                                                                                                                                                                                                                                                                                                     | Default Value                                                   | Possible Parameters                                                                                                                             |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| mail.smtp.ssl.trust               | If this parameter is se, and a socket factory has not been specified, it enables the use of a MailSSLSocketFactory. If this parameter is set to "_", all the hosts are trusted. If it is set to a whitespace-separated list of hosts, only those specified hosts are trusted. If not, the hosts trusted depends on the certificate presented by the server._                    |                                                                 | String                                                                                                                                          |
| mail.smtp.connectiontimeout       | The socket connection timeout value in milliseconds.                                                                                                                                                                                                                                                                                                                            | infinite timeout                                                | Any Integer                                                                                                                                     |
| mail.smtp.timeout                 | The socket I/O timeout value in milliseconds.                                                                                                                                                                                                                                                                                                                                   | infinite timeout                                                | Any Integer                                                                                                                                     |
| mail.smtp.from                    | The email address to use for the SMTP MAIL command. This sets the envelope return address.                                                                                                                                                                                                                                                                                      | Defaults to msg.getFrom() or InternetAddress.getLocalAddress(). | Any valid email address                                                                                                                         |
| mail.smtp.localport               | The local port number to bind to when creating the SMTP socket.                                                                                                                                                                                                                                                                                                                 | Defaults to the port number picked by the Socket class.         | Any Integer                                                                                                                                     |
| mail.smtp.ehlo                    | If this parameter is set to 'false', you must not attempt to sign in with the EHLO command.                                                                                                                                                                                                                                                                                     | true                                                            | true or false                                                                                                                                   |
| mail.smtp.auth.login.disable      | If this is set to 'true', it is not allowed to use the 'AUTH LOGIN' command.                                                                                                                                                                                                                                                                                                    | false                                                           | true or false                                                                                                                                   |
| mail.smtp.auth.plain.disable      | If this parameter is set to 'true', it is not allowed to use the 'AUTH PLAIN' command.                                                                                                                                                                                                                                                                                          | false                                                           | true or false                                                                                                                                   |
| mail.smtp.auth.digest-md5.disable | If this parameter is set to 'true', it is not allowed to use the 'AUTH DIGEST-MD5' command.                                                                                                                                                                                                                                                                                     | false                                                           | true or false                                                                                                                                   |
| mail.smtp.auth.ntlm.disable       | If this parameter is set to 'true', it is not allowed to use the 'AUTH NTLM' command                                                                                                                                                                                                                                                                                            | false                                                           | true or false                                                                                                                                   |
| mail.smtp.auth.ntlm.domain        | The NTLM authentication domain.                                                                                                                                                                                                                                                                                                                                                 | None                                                            | The valid NTLM authentication domain name.                                                                                                      |
| mail.smtp.auth.ntlm.flags         | NTLM protocol-specific flags. For more details, see http://curl.haxx.se/rfc/ntlm.html#theNtlmFlags.                                                                                                                                                                                                                                                                             | None                                                            | Valid NTLM protocol-specific flags.                                                                                                             |
| mail.smtp.dsn.notify              | The NOTIFY option to the RCPT command.                                                                                                                                                                                                                                                                                                                                          | None                                                            | Either 'NEVER', or a combination of 'SUCCESS', 'FAILURE', and 'DELAY' (separated by commas).                                                    |
| mail.smtp.dsn.ret                 | The 'RET' option to the 'MAIL' command.                                                                                                                                                                                                                                                                                                                                         | None                                                            | Either 'FULL' or 'HDRS'.                                                                                                                        |
| mail.smtp.sendpartial             | If this parameter is set to 'true' and a message is addressed to both valid and invalid addresses, the message is sent with a log that reports the partial failure with a 'SendFailedException' error. If this parameter is set to 'false' (which is default), the message is not sent to any of the recipients when the recipient lists contain one or more invalid addresses. | false                                                           | true or false                                                                                                                                   |
| mail.smtp.sasl.enable             | If this parameter is set to 'true', the system attempts to use the 'javax.security.sasl' package to choose an authentication mechanism for the login.                                                                                                                                                                                                                           | false                                                           | true or false                                                                                                                                   |
| mail.smtp.sasl.mechanisms         | Enter a space or a comma-separated list of SASL mechanism names that the system shouldt try to use.                                                                                                                                                                                                                                                                             | None                                                            |                                                                                                                                                 |
| mail.smtp.sasl.authorizationid    | The authorization ID to be used in the SASL authentication. If no value is specified, the authentication ID (i.e., username) is used.                                                                                                                                                                                                                                           | username                                                        | Valid ID                                                                                                                                        |
| mail.smtp.sasl.realm              | The realm to be used with the 'DIGEST-MD5' authentication.                                                                                                                                                                                                                                                                                                                      | None                                                            |                                                                                                                                                 |
| mail.smtp.quitwait                | If this parameter is set to 'false', the 'QUIT' command is issued and the connection is immediately closed. If this parameter is set to 'true' (which is default), the transport waits for the response to the QUIT command.                                                                                                                                                    | false                                                           | true or false                                                                                                                                   |
| mail.smtp.reportsuccess           | If this parameter is set to 'true', the transport to includes an 'SMTPAddressSucceededException' for each address to which the message is successfully delivered.                                                                                                                                                                                                               | false                                                           | true or false                                                                                                                                   |
| mail.smtp.socketFactory           | If this parameter is set to a class that implements the 'javax.net.SocketFactory' interface, this class is used to create SMTP sockets.                                                                                                                                                                                                                                         | None                                                            | Socket Factory                                                                                                                                  |
| mail.smtp.socketFactory.class     | If this parameter is set, it specifies the name of a class that implements the 'javax.net.SocketFactory interface'. This class is used to create SMTP sockets.                                                                                                                                                                                                                  | None                                                            |                                                                                                                                                 |
| mail.smtp.socketFactory.fallback  | If this parameter is set to 'true', the failure to create a socket using the specified socket factory class causes the socket to be created using the 'java.net.Socket' class.                                                                                                                                                                                                  | true                                                            | true or false                                                                                                                                   |
| mail.smtp.socketFactory.port      | This specifies the port to connect to when using the specified socket factory.                                                                                                                                                                                                                                                                                                  | 25                                                              | Valid port number                                                                                                                               |
| mail.smtp.ssl.protocols           | This specifies the SSL protocols that need to be enabled for the SSL connections.                                                                                                                                                                                                                                                                                               | None                                                            | This parameter specifies a whitespace separated list of tokens that are acceptable to the 'javax.net.ssl.SSLSocket.setEnabledProtocols' method. |
| mail.smtp.starttls.enable         | If this parameter is set to 'true', it is possible to issue the 'STARTTLS' command (if supported by the server) to switch the connection to a TLS-protected connection before issuing any login commands.                                                                                                                                                                       | false                                                           | true or false                                                                                                                                   |
| mail.smtp.starttls.required       | If this parameter is set to 'true', it is required to use the 'STARTTLS' command. If the server does not support the 'STARTTLS' command, or if the command fails, the connection method will fail.                                                                                                                                                                              | false                                                           | true or false                                                                                                                                   |
| mail.smtp.socks.host              | This specifies the host name of a SOCKS5 proxy server to be used for the connections to the mail server.                                                                                                                                                                                                                                                                        | None                                                            |                                                                                                                                                 |
| mail.smtp.socks.port              | This specifies the port number for the SOCKS5 proxy server. This needs to be used only if the proxy server is not using the standard port number 1080.                                                                                                                                                                                                                          | 1080                                                            | valid port number                                                                                                                               |
| mail.smtp.auth.ntlm.disable       | If this parameter is set to 'true', the AUTH NTLM command cannot be issued.                                                                                                                                                                                                                                                                                                     | false                                                           | true or false                                                                                                                                   |
| mail.smtp.mailextension           | The extension string to be appended to the MAIL command.                                                                                                                                                                                                                                                                                                                        | None                                                            |                                                                                                                                                 |
| mail.smtp.userset                 | If this parameter is set to 'true', you should use the 'RSET' command instead of the 'NOOP' command in the 'isConnected' method. In some scenarios, 'sendmail' responds slowly after many 'NOOP' commands. This is avoided by using 'RSET' instead.                                                                                                                             | false                                                           | true or false                                                                                                                                   |

ExamplesEXAMPLE 1

```
@sink(type='email', @map(type ='json'), username='sender.account', address='sender.account@gmail.com',password='account.password',subject='Alerts from Wso2 Stream Processor',to='{{email}}',)define stream FooStream (email string, loginId int, name string);
```

This example illustrates how to publish events via an email sink based on the values provided for the mandatory parameters. As shown in the example, it publishes events from the 'FooStream' in 'json' format as emails to the specified 'to' recipients via the email sink. The email is sent from the 'sender.account@gmail.com' email address via a secure connection.

EXAMPLE 2

```
@sink(type='email', @map(type ='json'), subject='Alerts from Wso2 Stream Processor',to='{{email}}',)define stream FooStream (email string, loginId int, name string);
```

This example illustrates how to configure the query parameters and the system parameters in the 'deployment.yaml' file.\
&#x20;Corresponding parameters need to be configured under 'email', and namespace:'sink' as follows:\
&#x20; siddhi: extensions:\
&#x20;     \- extension:\
&#x20;         name:'email'\
&#x20;         namespace:'sink'\
&#x20;         properties:\
&#x20;           username: \<sender's email username>\
&#x20;           address: \<sender's email address>\
&#x20;           password: \<sender's email password>\
\
As shown in the example, events from the FooStream are published in 'json' format via the email sink as emails to the given 'to' recipients. The email is sent from the 'sender.account@gmail.com' address via a secure connection.

EXAMPLE 3

```
@sink(type='email', @map(type ='json'), username='sender.account', address='sender.account@gmail.com',password='account.password',host='smtp.gmail.com',port='465',ssl.enable='true',auth='true',content.type='text/html',subject='Alerts from Wso2 Stream Processor-{{name}}',to='to1.account@gmail.com, to2.account@gmail.com',cc='cc1.account@gmail.com, cc2.account@gmail.com',bcc='bcc1.account@gmail.com)define stream FooStream (name string, age int, country string);
```

This example illustrates how to publish events via the email sink. Events from the 'FooStream' stream are published in 'xml' format via the email sink as a text/html message and sent to the specified 'to', 'cc', and 'bcc' recipients via a secure connection. The 'name' namespace in the 'subject' attribute is the value of the 'name' parameter in the corresponding output event.

EXAMPLE 4

```
@sink(type='email', @map(type ='json'), username='sender.account', address='sender.account@gmail.com',password='account.password',host='smtp.gmail.com',port='465',ssl.enable='true',auth='true',content.type='text/html',subject='Alerts from Wso2 Stream Processor-{{name}}',to='to1.account@gmail.com, to2.account@gmail.com',cc='cc1.account@gmail.com, cc2.account@gmail.com',bcc='bcc1.account@gmail.comattachments= '{{attachments}}')define stream FooStream (name string, age int, country string, attachments string);
```

This example illustrates how to publish events via the email sink. Here, the email also contains attachments.\
&#x20;Events from the FooStream are published in 'xml' format via the email sink as a 'text/html' message to the specified 'to','cc', and 'bcc' recipients via a secure connection. The 'name' namespace in the 'subject' attribute is the value for the 'name' parameter in the corresponding output event.\
The attachments included in the email message are the local files available in the path specified as the value for the 'attachments' attribute.

#### http [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#http-sink" id="http-sink"></a>

This extension publish the HTTP events in any HTTP method POST, GET, PUT, DELETE via HTTP or https protocols. As the additional features this component can provide basic authentication as well as user can publish events using custom client truststore files when publishing events via https protocol. And also user can add any number of headers including HTTP\_METHOD header for each event dynamically.\
Following content types will be set by default according to the type of sink mapper used.\
You can override them by setting the new content types in headers.\
&#x20;    \- TEXT : text/plain\
&#x20;    \- XML : application/xml\
&#x20;    \- JSON : application/json\
&#x20;    \- KEYVALUE : application/x-www-form-urlencoded

_Origin: siddhi-io-http:2.0.4_

Syntax

```
@sink(type="http", publisher.url="<STRING>", basic.auth.username="<STRING>", basic.auth.password="<STRING>", https.truststore.file="<STRING>", https.truststore.password="<STRING>", headers="<STRING>", method="<STRING>", socket.idle.timeout="<INT>", chunk.disabled="<BOOL>", ssl.protocol="<STRING>", parameters="<STRING>", ciphers="<STRING>", ssl.enabled.protocols="<STRING>", client.enable.session.creation="<STRING>", follow.redirect="<BOOL>", max.redirect.count="<INT>", tls.store.type="<STRING>", proxy.host="<STRING>", proxy.port="<STRING>", proxy.username="<STRING>", proxy.password="<STRING>", client.bootstrap.configuration="<STRING>", client.bootstrap.nodelay="<BOOL>", client.bootstrap.keepalive="<BOOL>", client.bootstrap.sendbuffersize="<INT>", client.bootstrap.recievebuffersize="<INT>", client.bootstrap.connect.timeout="<INT>", client.bootstrap.socket.reuse="<BOOL>", client.bootstrap.socket.timeout="<STRING>", client.threadpool.configurations="<STRING>", client.connection.pool.count="<INT>", client.max.active.connections.per.pool="<INT>", client.min.idle.connections.per.pool="<INT>", client.max.idle.connections.per.pool="<INT>", client.min.eviction.idle.time="<STRING>", sender.thread.count="<STRING>", event.group.executor.thread.size="<STRING>", max.wait.for.client.connection.pool="<STRING>", oauth.username="<STRING>", oauth.password="<STRING>", consumer.key="<STRING>", consumer.secret="<STRING>", refresh.token="<STRING>", token.url="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Default Value                                           | Possible Data Types | Optional | Dynamic |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------- | ------------------- | -------- | ------- |
| publisher.url                          | The URL to which the outgoing events should be published via HTTP. This is a mandatory parameter and if this is not specified, an error is logged in the CLI. If user wants to enable SSL for the events, use `https` instead of `http` in the publisher.url.e.g., `http://localhost:8080/endpoint`, `https://localhost:8080/endpoint`                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                         | STRING              | No       | No      |
| basic.auth.username                    | The username to be included in the authentication header of the basic authentication enabled events. It is required to specify both username and password to enable basic authentication. If one of the parameter is not given by user then an error is logged in the CLI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                                         | STRING              | Yes      | No      |
| basic.auth.password                    | The password to include in the authentication header of the basic authentication enabled events. It is required to specify both username and password to enable basic authentication. If one of the parameter is not given by user then an error is logged in the CLI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                         | STRING              | Yes      | No      |
| https.truststore.file                  | The file path to the location of the truststore of the client that sends the HTTP events through 'https' protocol. A custom client-truststore can be specified if required.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | ${carbon.home}/resources/security/client-truststore.jks | STRING              | Yes      | No      |
| https.truststore.password              | The password for the client-truststore. A custom password can be specified if required. If no custom password is specified and the protocol of URL is 'https' then, the system uses default password.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | wso2carbon                                              | STRING              | Yes      | No      |
| headers                                | <p>The headers that should be included as HTTP request headers.<br>There can be any number of headers concatenated in following format. "'header1:value1','header2:value2'". User can include Content-Type header if he needs to use a specific content-type for the payload. Or else, system decides the Content-Type by considering the type of sink mapper, in following way.<br> - @map(xml):application/xml<br> - @map(json):application/json<br> - @map(text):plain/text )<br> - if user does not include any mapping type then the system gets 'plain/text' as default Content-Type header.<br>Note that providing content-length as a header is not supported. The size of the payload will be automatically calculated and included in the content-length header.</p> |                                                         | STRING              | Yes      | No      |
| method                                 | For HTTP events, HTTP\_METHOD header should be included as a request header. If the parameter is null then system uses 'POST' as a default header.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | POST                                                    | STRING              | Yes      | No      |
| socket.idle.timeout                    | Socket timeout value in millisecond                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 6000                                                    | INT                 | Yes      | No      |
| chunk.disabled                         | This parameter is used to disable/enable chunked transfer encoding                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | false                                                   | BOOL                | Yes      | No      |
| ssl.protocol                           | The SSL protocol version                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | TLS                                                     | STRING              | Yes      | No      |
| parameters                             | Parameters other than basics such as ciphers,sslEnabledProtocols,client.enable.session.creation. Expected format of these parameters is as follows: "'ciphers:xxx','sslEnabledProtocols,client.enable:xxx'"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | null                                                    | STRING              | Yes      | No      |
| ciphers                                | List of ciphers to be used. This parameter should include under parameters Ex: 'ciphers:TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | null                                                    | STRING              | Yes      | No      |
| ssl.enabled.protocols                  | SSL/TLS protocols to be enabled. This parameter should be in camel case format(sslEnabledProtocols) under parameters. Ex 'sslEnabledProtocols:true'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | null                                                    | STRING              | Yes      | No      |
| client.enable.session.creation         | Enable HTTP session creation.This parameter should include under parameters Ex: 'client.enable.session.creation:true'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | null                                                    | STRING              | Yes      | No      |
| follow.redirect                        | Redirect related enabled.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | true                                                    | BOOL                | Yes      | No      |
| max.redirect.count                     | Maximum redirect count.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | 5                                                       | INT                 | Yes      | No      |
| tls.store.type                         | TLS store type to be used.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | JKS                                                     | STRING              | Yes      | No      |
| proxy.host                             | Proxy server host                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | null                                                    | STRING              | Yes      | No      |
| proxy.port                             | Proxy server port                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | null                                                    | STRING              | Yes      | No      |
| proxy.username                         | Proxy server username                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | null                                                    | STRING              | Yes      | No      |
| proxy.password                         | Proxy server password                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | null                                                    | STRING              | Yes      | No      |
| client.bootstrap.configuration         | Client bootsrap configurations. Expected format of these parameters is as follows: "'client.bootstrap.nodelay:xxx','client.bootstrap.keepalive:xxx'"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | TODO                                                    | STRING              | Yes      | No      |
| client.bootstrap.nodelay               | Http client no delay.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | true                                                    | BOOL                | Yes      | No      |
| client.bootstrap.keepalive             | Http client keep alive.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | true                                                    | BOOL                | Yes      | No      |
| client.bootstrap.sendbuffersize        | Http client send buffer size.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | 1048576                                                 | INT                 | Yes      | No      |
| client.bootstrap.recievebuffersize     | Http client receive buffer size.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 1048576                                                 | INT                 | Yes      | No      |
| client.bootstrap.connect.timeout       | Http client connection timeout.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | 15000                                                   | INT                 | Yes      | No      |
| client.bootstrap.socket.reuse          | To enable http socket reuse.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | false                                                   | BOOL                | Yes      | No      |
| client.bootstrap.socket.timeout        | Http client socket timeout.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 15                                                      | STRING              | Yes      | No      |
| client.threadpool.configurations       | Thread pool configuration. Expected format of these parameters is as follows: "'client.connection.pool.count:xxx','client.max.active.connections.per.pool:xxx'"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | TODO                                                    | STRING              | Yes      | No      |
| client.connection.pool.count           | Connection pool count.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | 0                                                       | INT                 | Yes      | No      |
| client.max.active.connections.per.pool | Active connections per pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | -1                                                      | INT                 | Yes      | No      |
| client.min.idle.connections.per.pool   | Minimum ideal connection per pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 0                                                       | INT                 | Yes      | No      |
| client.max.idle.connections.per.pool   | Maximum ideal connection per pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 100                                                     | INT                 | Yes      | No      |
| client.min.eviction.idle.time          | Minimum eviction idle time.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 5 \* 60 \* 1000                                         | STRING              | Yes      | No      |
| sender.thread.count                    | Http sender thread count.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | 20                                                      | STRING              | Yes      | No      |
| event.group.executor.thread.size       | Event group executor thread size.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 15                                                      | STRING              | Yes      | No      |
| max.wait.for.client.connection.pool    | Maximum wait for client connection pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 60000                                                   | STRING              | Yes      | No      |
| oauth.username                         | The username to be included in the authentication header of the oauth authentication enabled events. It is required to specify both username and password to enable oauth authentication. If one of the parameter is not given by user then an error is logged in the CLI. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                         | STRING              | Yes      | No      |
| oauth.password                         | The password to be included in the authentication header of the oauth authentication enabled events. It is required to specify both username and password to enable oauth authentication. If one of the parameter is not given by user then an error is logged in the CLI. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                         | STRING              | Yes      | No      |
| consumer.key                           | consumer key for the Http request. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |                                                         | STRING              | Yes      | No      |
| consumer.secret                        | consumer secret for the Http request. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                         | STRING              | Yes      | No      |
| refresh.token                          | refresh token for the Http request. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                         | STRING              | Yes      | No      |
| token.url                              | token url for generate a new access token. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                         | STRING              | Yes      | No      |

System Parameters

| Name                           | Description                                                                                                                                                                                                               | Default Value                                           | Possible Parameters           |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- | ----------------------------- |
| clientBootstrapBossGroupSize   | property to configure number of boss threads, which accepts incoming connections until the ports are unbound. Once connection accepts successfully, boss thread passes the accepted channel to one of the worker threads. | Number of available processors                          | Any integer                   |
| clientBootstrapWorkerGroupSize | property to configure number of worker threads, which performs non blocking read and write for one or more channels in non-blocking mode.                                                                                 | (Number of available processors)\*2                     | Any integer                   |
| clientBootstrapClientGroupSize | property to configure number of client threads, which performs non blocking read and write for one or more channels in non-blocking mode.                                                                                 | (Number of available processors)\*2                     | Any integer                   |
| trustStoreLocation             | The default truststore file path.                                                                                                                                                                                         | ${carbon.home}/resources/security/client-truststore.jks | Path to client-truststore.jks |
| trustStorePassword             | The default truststore password.                                                                                                                                                                                          | wso2carbon                                              | Truststore password           |

ExamplesEXAMPLE 1

```
@sink(type='http',publisher.url='http://localhost:8009/foo', method='{{method}}',headers="'content-type:xml','content-length:94'", client.bootstrap.configuration="'client.bootstrap.socket.timeout:20', 'client.bootstrap.worker.group.size:10'", client.pool.configuration="'client.connection.pool.count:10','client.max.active.connections.per.pool:1'", @map(type='xml', @payload('{{payloadBody}}')))
define stream FooStream (payloadBody String, method string, headers string);
```

If it is xml mapping expected input should be in following format for FooStream:\
{\
\<events>\
&#x20;   \<event>\
&#x20;       \<symbol>WSO2\</symbol>\
&#x20;       \<price>55.6\</price>\
&#x20;       \<volume>100\</volume>\
&#x20;   \</event>\
\</events>,\
POST,\
Content-Length:24#Content-Location:USA#Retry-After:120\
}\
\
Above event will generate output as below.\
\~Output http event payload\
\<events>\
&#x20;   \<event>\
&#x20;       \<symbol>WSO2\</symbol>\
&#x20;       \<price>55.6\</price>\
&#x20;       \<volume>100\</volume>\
&#x20;   \</event>\
\</events>\
\
\~Output http event headers\
Content-Length:24,\
Content-Location:'USA',\
Retry-After:120,\
Content-Type:'application/xml',\
HTTP\_METHOD:'POST',\
\
\~Output http event properties\
HTTP\_METHOD:'POST',\
HOST:'localhost',\
PORT:8009,\
PROTOCOL:'http',\
TO:'/foo'

#### http-request [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink)[¶](https://siddhi.io/en/v5.0/docs/api/5.0.0/#http-request-sink) <a href="#http-request-sink" id="http-request-sink"></a>

This extension publish the HTTP events in any HTTP method POST, GET, PUT, DELETE via HTTP or https protocols. As the additional features this component can provide basic authentication as well as user can publish events using custom client truststore files when publishing events via https protocol. And also user can add any number of headers including HTTP\_METHOD header for each event dynamically.\
Following content types will be set by default according to the type of sink mapper used.\
You can override them by setting the new content types in headers.\
&#x20;    \- TEXT : text/plain\
&#x20;    \- XML : application/xml\
&#x20;    \- JSON : application/json\
&#x20;    \- KEYVALUE : application/x-www-form-urlencoded\
\
HTTP request sink is correlated with the The HTTP reponse source, through a unique `sink.id`.It sends the request to the defined url and the response is received by the response source which has the same 'sink.id'.

_Origin: siddhi-io-http:2.0.4_

Syntax

```
@sink(type="http-request", publisher.url="<STRING>", basic.auth.username="<STRING>", basic.auth.password="<STRING>", https.truststore.file="<STRING>", https.truststore.password="<STRING>", headers="<STRING>", method="<STRING>", socket.idle.timeout="<INT>", chunk.disabled="<BOOL>", ssl.protocol="<STRING>", parameters="<STRING>", ciphers="<STRING>", ssl.enabled.protocols="<STRING>", client.enable.session.creation="<STRING>", follow.redirect="<BOOL>", max.redirect.count="<INT>", tls.store.type="<STRING>", proxy.host="<STRING>", proxy.port="<STRING>", proxy.username="<STRING>", proxy.password="<STRING>", client.bootstrap.configuration="<STRING>", client.bootstrap.nodelay="<BOOL>", client.bootstrap.keepalive="<BOOL>", client.bootstrap.sendbuffersize="<INT>", client.bootstrap.recievebuffersize="<INT>", client.bootstrap.connect.timeout="<INT>", client.bootstrap.socket.reuse="<BOOL>", client.bootstrap.socket.timeout="<STRING>", client.threadpool.configurations="<STRING>", client.connection.pool.count="<INT>", client.max.active.connections.per.pool="<INT>", client.min.idle.connections.per.pool="<INT>", client.max.idle.connections.per.pool="<INT>", client.min.eviction.idle.time="<STRING>", sender.thread.count="<STRING>", event.group.executor.thread.size="<STRING>", max.wait.for.client.connection.pool="<STRING>", sink.id="<STRING>", downloading.enabled="<BOOL>", download.path="<STRING>", oauth.username="<STRING>", oauth.password="<STRING>", consumer.key="<STRING>", consumer.secret="<STRING>", refresh.token="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Default Value                                           | Possible Data Types | Optional | Dynamic |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------- | ------------------- | -------- | ------- |
| publisher.url                          | <p>The URL to which the outgoing events should be published via HTTP. This is a mandatory parameter and if this is not specified, an error is logged in the CLI. If user wants to enable SSL for the events, use <code>https</code> instead of <code>http</code> in the publisher.url.<br>e.g., <code>http://localhost:8080/endpoint</code>, <code>https://localhost:8080/endpoint</code><br>This can be used as a dynamic parameter as well.</p>                                                                                                                                                                                                                                                                                                                              |                                                         | STRING              | No       | Yes     |
| basic.auth.username                    | The username to be included in the authentication header of the basic authentication enabled events. It is required to specify both username and password to enable basic authentication. If one of the parameter is not given by user then an error is logged in the CLI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                                         | STRING              | Yes      | No      |
| basic.auth.password                    | The password to include in the authentication header of the basic authentication enabled events. It is required to specify both username and password to enable basic authentication. If one of the parameter is not given by user then an error is logged in the CLI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                         | STRING              | Yes      | No      |
| https.truststore.file                  | The file path to the location of the truststore of the client that sends the HTTP events through 'https' protocol. A custom client-truststore can be specified if required.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | ${carbon.home}/resources/security/client-truststore.jks | STRING              | Yes      | No      |
| https.truststore.password              | The password for the client-truststore. A custom password can be specified if required. If no custom password is specified and the protocol of URL is 'https' then, the system uses default password.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | wso2carbon                                              | STRING              | Yes      | No      |
| headers                                | <p>The headers that should be included as HTTP request headers.<br>There can be any number of headers concatenated in following format. "'header1:value1','header2:value2'". User can include Content-Type header if he needs to use a specific content-type for the payload. Or else, system decides the Content-Type by considering the type of sink mapper, in following way.<br> - @map(xml):application/xml<br> - @map(json):application/json<br> - @map(text):plain/text )<br> - if user does not include any mapping type then the system gets 'plain/text' as default Content-Type header.<br>Note that providing content-length as a header is not supported. The size of the payload will be automatically calculated and included in the content-length header.</p> |                                                         | STRING              | Yes      | No      |
| method                                 | For HTTP events, HTTP\_METHOD header should be included as a request header. If the parameter is null then system uses 'POST' as a default header.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | POST                                                    | STRING              | Yes      | No      |
| socket.idle.timeout                    | Socket timeout value in millisecond                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 6000                                                    | INT                 | Yes      | No      |
| chunk.disabled                         | port: Port number of the remote service                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | false                                                   | BOOL                | Yes      | No      |
| ssl.protocol                           | The SSL protocol version                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | TLS                                                     | STRING              | Yes      | No      |
| parameters                             | Parameters other than basics such as ciphers,sslEnabledProtocols,client.enable.session.creation. Expected format of these parameters is as follows: "'ciphers:xxx','sslEnabledProtocols,client.enable:xxx'"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | null                                                    | STRING              | Yes      | No      |
| ciphers                                | List of ciphers to be used. This parameter should include under parameters Ex: 'ciphers:TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | null                                                    | STRING              | Yes      | No      |
| ssl.enabled.protocols                  | SSL/TLS protocols to be enabled. This parameter should be in camel case format(sslEnabledProtocols) under parameters. Ex 'sslEnabledProtocols:true'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | null                                                    | STRING              | Yes      | No      |
| client.enable.session.creation         | Enable HTTP session creation.This parameter should include under parameters Ex: 'client.enable.session.creation:true'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | null                                                    | STRING              | Yes      | No      |
| follow.redirect                        | Redirect related enabled.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | true                                                    | BOOL                | Yes      | No      |
| max.redirect.count                     | Maximum redirect count.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | 5                                                       | INT                 | Yes      | No      |
| tls.store.type                         | TLS store type to be used.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | JKS                                                     | STRING              | Yes      | No      |
| proxy.host                             | Proxy server host                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | null                                                    | STRING              | Yes      | No      |
| proxy.port                             | Proxy server port                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | null                                                    | STRING              | Yes      | No      |
| proxy.username                         | Proxy server username                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | null                                                    | STRING              | Yes      | No      |
| proxy.password                         | Proxy server password                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | null                                                    | STRING              | Yes      | No      |
| client.bootstrap.configuration         | Client bootsrap configurations. Expected format of these parameters is as follows: "'client.bootstrap.nodelay:xxx','client.bootstrap.keepalive:xxx'"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | TODO                                                    | STRING              | Yes      | No      |
| client.bootstrap.nodelay               | Http client no delay.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | true                                                    | BOOL                | Yes      | No      |
| client.bootstrap.keepalive             | Http client keep alive.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | true                                                    | BOOL                | Yes      | No      |
| client.bootstrap.sendbuffersize        | Http client send buffer size.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | 1048576                                                 | INT                 | Yes      | No      |
| client.bootstrap.recievebuffersize     | Http client receive buffer size.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 1048576                                                 | INT                 | Yes      | No      |
| client.bootstrap.connect.timeout       | Http client connection timeout.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | 15000                                                   | INT                 | Yes      | No      |
| client.bootstrap.socket.reuse          | To enable http socket reuse.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | false                                                   | BOOL                | Yes      | No      |
| client.bootstrap.socket.timeout        | Http client socket timeout.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 15                                                      | STRING              | Yes      | No      |
| client.threadpool.configurations       | Thread pool configuration. Expected format of these parameters is as follows: "'client.connection.pool.count:xxx','client.max.active.connections.per.pool:xxx'"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | TODO                                                    | STRING              | Yes      | No      |
| client.connection.pool.count           | Connection pool count.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | 0                                                       | INT                 | Yes      | No      |
| client.max.active.connections.per.pool | Active connections per pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | -1                                                      | INT                 | Yes      | No      |
| client.min.idle.connections.per.pool   | Minimum ideal connection per pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 0                                                       | INT                 | Yes      | No      |
| client.max.idle.connections.per.pool   | Maximum ideal connection per pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 100                                                     | INT                 | Yes      | No      |
| client.min.eviction.idle.time          | Minimum eviction idle time.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 5 \* 60 \* 1000                                         | STRING              | Yes      | No      |
| sender.thread.count                    | Http sender thread count.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | 20                                                      | STRING              | Yes      | No      |
| event.group.executor.thread.size       | Event group executor thread size.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 15                                                      | STRING              | Yes      | No      |
| max.wait.for.client.connection.pool    | Maximum wait for client connection pool.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 60000                                                   | STRING              | Yes      | No      |
| sink.id                                | Identifier of the sink. This is used to co-relate with the corresponding http-response source which needs to process the repose for the request sent by this sink.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                         | STRING              | No       | No      |
| downloading.enabled                    | If this is set to 'true' then the response received by the response source will be written to a file. If downloading is enabled, the download.path parameter is mandatory.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | false                                                   | BOOL                | Yes      | No      |
| download.path                          | If downloading is enabled, the path of the file which is going to be downloaded should be specified using 'download.path' parameter. This should be an absolute path including the file name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | null                                                    | STRING              | Yes      | Yes     |
| oauth.username                         | The username to be included in the authentication header of the oauth authentication enabled events. It is required to specify both username and password to enable oauth authentication. If one of the parameter is not given by user then an error is logged in the CLI. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                         | STRING              | Yes      | No      |
| oauth.password                         | The password to be included in the authentication header of the oauth authentication enabled events. It is required to specify both username and password to enable oauth authentication. If one of the parameter is not given by user then an error is logged in the CLI. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                         | STRING              | Yes      | No      |
| consumer.key                           | consumer key for the Http request. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |                                                         | STRING              | Yes      | No      |
| consumer.secret                        | consumer secret for the Http request. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                         | STRING              | Yes      | No      |
| refresh.token                          | refresh token for the Http request. It is only applicable for for Oauth requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                         | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='http-request', sink.id='foo', publisher.url='http://localhost:8009/foo', @map(type='xml', @payload('{{payloadBody}}')))
define stream FooStream (payloadBody String, method string, headers string);
@source(type='http-response', sink.id='foo', http.status.code='2\\d+', 
@map(type='text', regex.A='((.|\n)*)', @attributes(headers='trp:headers', fileName='A[1]')))
define stream responseStream2xx(fileName string, headers string);

@source(type='http-response', sink.id='foo', http.status.code='4\\d+', 
@map(type='text', regex.A='((.|\n)*)', @attributes(errorMsg='A[1]')))
define stream responseStream4xx(errorMsg string);
```

In above example, the payload body for 'FooStream' will be in following format.\
{\
\<events>\
&#x20;   \<event>\
&#x20;       \<symbol>WSO2\</symbol>\
&#x20;       \<price>55.6\</price>\
&#x20;       \<volume>100\</volume>\
&#x20;   \</event>\
\</events>,\
This message will sent as the body of a POST request with the content-type 'application/xml' to the endpoint defined as the 'publisher.url' and in order to process the responses for these requests, there should be a source of type 'http-response' defined with the same sink id 'foo' in the siddhi app.\
&#x20;The responses with 2xx status codes will be received by the http-response source which has the http.status.code defined by the regex '2\d+'.\
If the response has a 4xx status code, it will be received by the http-response source which has the http.status.code defined by the regex '4\d+'.\


EXAMPLE 2

```
define stream FooStream (name String, id int, headers String, downloadPath string);
@sink(type='http-request', 
downloading.enabled='true',
download.path='{{downloadPath}}',publisher.url='http://localhost:8005/files',
method='GET', headers='{{headers}}',sink.id='download-sink',
@map(type='json')) 
define stream BarStream (name String, id int, headers String, downloadPath string);

@source(type='http-response', sink.id='download-sink', http.status.code='2\\d+', 
@map(type='text', regex.A='((.|\n)*)', @attributes(headers='trp:headers', fileName='A[1]')))
define stream responseStream2xx(fileName string, headers string);

@source(type='http-response', sink.id='download-sink', http.status.code='4\\d+', 
@map(type='text', regex.A='((.|\n)*)', @attributes(errorMsg='A[1]')))
define stream responseStream4xx(errorMsg string);
```

In above example, http-request sink will send a GET request to the publisher url and the requested file will be received as the response by a corresponding http-response source.\
If the http status code of the response is a successful one (2xx), it will be received by the http-response source which has the http.status.code '2\d+' and downloaded as a local file. Then the event received to the responseStream2xx will have the headers included in the request and the downloaded file name.\
If the http status code of the response is a 4xx code, it will be received by the http-response source which has the http.status.code '4\d+'. Then the event received to the responseStream4xx will have the response message body in text format.

#### http-response [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#http-response-sink" id="http-response-sink"></a>

HTTP response sink is correlated with the The HTTP request source, through a unique `source.id`, and it send a response to the HTTP request source having the same `source.id`. The response message can be formatted in `text`, `XML` or `JSON` and can be sent with appropriate headers.

_Origin: siddhi-io-http:2.0.4_

Syntax

```
@sink(type="http-response", source.id="<STRING>", message.id="<STRING>", headers="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Default Value | Possible Data Types | Optional | Dynamic |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| source.id  | Identifier of the source.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |               | STRING              | No       | No      |
| message.id | Identifier of the message.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |               | STRING              | No       | Yes     |
| headers    | The headers that should be included as HTTP response headers. There can be any number of headers concatenated on following format. "'header1:value1','header2:value2'" User can include content-type header if he/she need to have any specific type for payload. If not system get the mapping type as the content-Type header (ie.`@map(xml)`:`application/xml`, `@map(json)`:`application/json`, `@map(text)`:`plain/text`) and if user does not include any mapping type then system gets the `plain/text` as default Content-Type header. If user does not include Content-Length header then system calculate the bytes size of payload and include it as content-length header. |               | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='http-response', source.id='sampleSourceId', message.id='{{messageId}}', headers="'content-type:json','content-length:94'"@map(type='json', @payload('{{payloadBody}}')))
define stream FooStream (payloadBody String, messageId string, headers string);
```

If it is json mapping expected input should be in following format for FooStream:\
{\
{"events":\
&#x20;   {"event":\
&#x20;       "symbol":WSO2,\
&#x20;       "price":55.6,\
&#x20;       "volume":100,\
&#x20;   }\
},\
0cf708b1-7eae-440b-a93e-e72f801b486a,\
Content-Length:24#Content-Location:USA\
}\
\
Above event will generate response for the matching source message as below.\
\
\~Output http event payload\
{"events":\
&#x20;   {"event":\
&#x20;       "symbol":WSO2,\
&#x20;       "price":55.6,\
&#x20;       "volume":100,\
&#x20;   }\
}\
\
\~Output http event headers\
Content-Length:24,\
Content-Location:'USA',\
Content-Type:'application/json'\


#### inMemory [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#inmemory-sink" id="inmemory-sink"></a>

In-memory transport that can communicate with other in-memory transports within the same JVM, itis assumed that the publisher and subscriber of a topic uses same event schema (stream definition).

_Origin: siddhi-core:5.0.0_

Syntax

```
@sink(type="inMemory", topic="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name  | Description                                                     | Default Value | Possible Data Types | Optional | Dynamic |
| ----- | --------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| topic | Event will be delivered to allthe subscribers of the same topic |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
@sink(type='inMemory', @map(type='passThrough'))
define stream BarStream (symbol string, price float, volume long)
```

In this example BarStream uses inMemory transport which emit the Siddhi events internally without using external transport and transformation.

#### kafka [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#kafka-sink" id="kafka-sink"></a>

A Kafka sink publishes events processed by WSO2 SP to a topic with a partition for a Kafka cluster. The events can be published in the `TEXT` `XML` `JSON` or `Binary` format.\
If the topic is not already created in the Kafka cluster, the Kafka sink creates the default partition for the given topic. The publishing topic and partition can be a dynamic value taken from the Siddhi event.\
To configure a sink to use the Kafka transport, the `type` parameter should have `kafka` as its value.

_Origin: siddhi-io-kafka:5.0.0_

Syntax

```
@sink(type="kafka", bootstrap.servers="<STRING>", topic="<STRING>", partition.no="<INT>", sequence.id="<STRING>", key="<STRING>", is.binary.message="<BOOL>", optional.configuration="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name                   | Description                                                                                                                                                                                             | Default Value | Possible Data Types | Optional | Dynamic |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| bootstrap.servers      | This parameter specifies the list of Kafka servers to which the Kafka sink must publish events. This list should be provided as a set of comma separated values. e.g., `localhost:9092,localhost:9093`. |               | STRING              | No       | No      |
| topic                  | The topic to which the Kafka sink needs to publish events. Only one topic must be specified.                                                                                                            |               | STRING              | No       | No      |
| partition.no           | The partition number for the given topic. Only one partition ID can be defined. If no value is specified for this parameter, the Kafka sink publishes to the default partition of the topic (i.e., 0)   | 0             | INT                 | Yes      | No      |
| sequence.id            | A unique identifier to identify the messages published by this sink. This ID allows receivers to identify the sink that published a specific message.                                                   | null          | STRING              | Yes      | No      |
| key                    | The key contains the values that are used to maintain ordering in a Kafka partition.                                                                                                                    | null          | STRING              | Yes      | No      |
| is.binary.message      | In order to send the binary events via kafka sink, this parameter is set to 'True'.                                                                                                                     | null          | BOOL                | No       | No      |
| optional.configuration | <p>This parameter contains all the other possible configurations that the producer is created with.<br>e.g., <code>producer.type:async,batch.size:200</code></p>                                        | null          | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@App:name('TestExecutionPlan') 
define stream FooStream (symbol string, price float, volume long); 
@info(name = 'query1') 
@sink(
type='kafka',
topic='topic_with_partitions',
partition.no='0',
bootstrap.servers='localhost:9092',
@map(type='xml'))
Define stream BarStream (symbol string, price float, volume long);
from FooStream select symbol, price, volume insert into BarStream;
```

This Kafka sink configuration publishes to 0th partition of the topic named `topic_with_partitions`.

EXAMPLE 2

```
@App:name('TestExecutionPlan') 
define stream FooStream (symbol string, price float, volume long); 
@info(name = 'query1') 
@sink(
type='kafka',
topic='{{symbol}}',
partition.no='{{volume}}',
bootstrap.servers='localhost:9092',
@map(type='xml'))
Define stream BarStream (symbol string, price float, volume long); 
from FooStream select symbol, price, volume insert into BarStream;
```

This query publishes dynamic topic and partitions that are taken from the Siddhi event. The value for `partition.no` is taken from the `volume` attribute, and the topic value is taken from the `symbol` attribute.

#### kafkaMultiDC [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#kafkamultidc-sink" id="kafkamultidc-sink"></a>

A Kafka sink publishes events processed by WSO2 SP to a topic with a partition for a Kafka cluster. The events can be published in the `TEXT` `XML` `JSON` or `Binary` format.\
If the topic is not already created in the Kafka cluster, the Kafka sink creates the default partition for the given topic. The publishing topic and partition can be a dynamic value taken from the Siddhi event.\
To configure a sink to publish events via the Kafka transport, and using two Kafka brokers to publish events to the same topic, the `type` parameter must have `kafkaMultiDC` as its value.

_Origin: siddhi-io-kafka:5.0.0_

Syntax

```
@sink(type="kafkaMultiDC", bootstrap.servers="<STRING>", topic="<STRING>", sequence.id="<STRING>", key="<STRING>", partition.no="<INT>", is.binary.message="<BOOL>", optional.configuration="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name                   | Description                                                                                                                                                                                                                                               | Default Value | Possible Data Types | Optional | Dynamic |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| bootstrap.servers      | This parameter specifies the list of Kafka servers to which the Kafka sink must publish events. This list should be provided as a set of comma -separated values. There must be at least two servers in this list. e.g., `localhost:9092,localhost:9093`. |               | STRING              | No       | No      |
| topic                  | The topic to which the Kafka sink needs to publish events. Only one topic must be specified.                                                                                                                                                              |               | STRING              | No       | No      |
| sequence.id            | A unique identifier to identify the messages published by this sink. This ID allows receivers to identify the sink that published a specific message.                                                                                                     | null          | STRING              | Yes      | No      |
| key                    | The key contains the values that are used to maintain ordering in a Kafka partition.                                                                                                                                                                      | null          | STRING              | Yes      | No      |
| partition.no           | The partition number for the given topic. Only one partition ID can be defined. If no value is specified for this parameter, the Kafka sink publishes to the default partition of the topic (i.e., 0)                                                     | 0             | INT                 | Yes      | No      |
| is.binary.message      | In order to send the binary events via kafkaMultiDCSink, it is required to set this parameter to `true`.                                                                                                                                                  | null          | BOOL                | No       | No      |
| optional.configuration | <p>This parameter contains all the other possible configurations that the producer is created with.<br>e.g., <code>producer.type:async,batch.size:200</code></p>                                                                                          | null          | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@App:name('TestExecutionPlan') 
define stream FooStream (symbol string, price float, volume long); 
@info(name = 'query1') 
@sink(type='kafkaMultiDC', topic='myTopic', partition.no='0',bootstrap.servers='host1:9092, host2:9092', @map(type='xml'))Define stream BarStream (symbol string, price float, volume long);
from FooStream select symbol, price, volume insert into BarStream;
```

This query publishes to the default (i.e., 0th) partition of the brokers in two data centers

#### log [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#log-sink" id="log-sink"></a>

This is a sink that can be used as a logger. This will log the output events in the output stream with user specified priority and a prefix

_Origin: siddhi-core:5.0.0_

Syntax

```
@sink(type="log", priority="<STRING>", prefix="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name     | Description                                                                                                                                                     | Default Value            | Possible Data Types | Optional | Dynamic |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ | ------------------- | -------- | ------- |
| priority | This will set the logger priority i.e log level. Accepted values are INFO, DEBUG, WARN, FATAL, ERROR, OFF, TRACE                                                | INFO                     | STRING              | Yes      | No      |
| prefix   | This will be the prefix to the output message. If the output stream has event \[2,4] and the prefix is given as "Hello" then the log will show "Hello : \[2,4]" | default prefix will be : | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='log', prefix='My Log', priority='DEBUG') 
define stream BarStream (symbol string, price float, volume long)
```

In this example BarStream uses log sink and the prefix is given as My Log. Also the priority is set to DEBUG.

EXAMPLE 2

```
@sink(type='log', priority='DEBUG') 
define stream BarStream (symbol string, price float, volume long)
```

In this example BarStream uses log sink and the priority is set to DEBUG. User has not specified prefix so the default prefix will be in the form \<Siddhi App Name> : \<Stream Name>

EXAMPLE 3

```
@sink(type='log', prefix='My Log') 
define stream BarStream (symbol string, price float, volume long)
```

In this example BarStream uses log sink and the prefix is given as My Log. User has not given a priority so it will be set to default INFO.

EXAMPLE 4

```
@sink(type='log') 
define stream BarStream (symbol string, price float, volume long)
```

In this example BarStream uses log sink. The user has not given prefix or priority so they will be set to their default values.

#### nats [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#nats-sink" id="nats-sink"></a>

NATS Sink allows users to subscribe to a NATS broker and publish messages.

_Origin: siddhi-io-nats:2.0.1_

Syntax

```
@sink(type="nats", destination="<STRING>", bootstrap.servers="<STRING>", client.id="<STRING>", cluster.id="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name              | Description                                                                                                                               | Default Value         | Possible Data Types | Optional | Dynamic |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ------------------- | -------- | ------- |
| destination       | Subject name which NATS sink should publish to.                                                                                           |                       | STRING              | No       | Yes     |
| bootstrap.servers | The NATS based url of the NATS server.                                                                                                    | nats://localhost:4222 | STRING              | Yes      | No      |
| client.id         | The identifier of the client publishing/connecting to the NATS broker. Should be unique for each client connecting to the server/cluster. | None                  | STRING              | Yes      | No      |
| cluster.id        | The identifier of the NATS server/cluster.                                                                                                | test-cluster          | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='nats', @map(type='xml'), destination='SP_NATS_OUTPUT_TEST', bootstrap.servers='nats://localhost:4222',client.id='nats_client',server.id='test-cluster')
define stream outputStream (name string, age int, country string);
```

This example shows how to publish to a NATS subject with all supporting configurations. With the following configuration the sink identified as 'nats-client' will publish to a subject named as 'SP\_NATS\_OUTPUT\_TEST' which resides in a nats instance with a cluster id of 'test-cluster', running in localhost and listening to the port 4222 for client connection.

EXAMPLE 2

```
@sink(type='nats', @map(type='xml'), destination='SP_NATS_OUTPUT_TEST')
define stream outputStream (name string, age int, country string);
```

This example shows how to publish to a NATS subject with mandatory configurations. With the following configuration the sink identified with an auto generated client id will publish to a subject named as 'SP\_NATS\_OUTPUT\_TEST' which resides in a nats instance with a cluster id of 'test-cluster', running in localhost and listening to the port 4222 for client connection.

#### tcp [_(Sink)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink) <a href="#tcp-sink" id="tcp-sink"></a>

A Siddhi application can be configured to publish events via the TCP transport by adding the @Sink(type = 'tcp') annotation at the top of an event stream definition.

_Origin: siddhi-io-tcp:3.0.1_

Syntax

```
@sink(type="tcp", url="<STRING>", sync="<STRING>", tcp.no.delay="<BOOL>", keep.alive="<BOOL>", worker.threads="<INT|LONG>", @map(...)))
```

QUERY PARAMETERS

| Name           | Description                                                                                                                                                                                                                                                                                                                   | Default Value | Possible Data Types | Optional | Dynamic |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| url            | <p>The URL to which outgoing events should be published via TCP.<br>The URL should adhere to <code>tcp://&#x3C;host>:&#x3C;port>/&#x3C;context></code> format.</p>                                                                                                                                                            |               | STRING              | No       | No      |
| sync           | <p>This parameter defines whether the events should be published in a synchronized manner or not.<br>If sync = 'true', then the worker will wait for the ack after sending the message.<br>Else it will not wait for an ack.</p>                                                                                              | false         | STRING              | Yes      | Yes     |
| tcp.no.delay   | <p>This is to specify whether to disable Nagle algorithm during message passing.<br>If tcp.no.delay = 'true', the execution of Nagle algorithm will be disabled in the underlying TCP logic. Hence there will be no delay between two successive writes to the TCP connection.<br>Else there can be a constant ack delay.</p> | true          | BOOL                | Yes      | No      |
| keep.alive     | This property defines whether the server should be kept alive when there are no connections available.                                                                                                                                                                                                                        | true          | BOOL                | Yes      | No      |
| worker.threads | Number of threads to publish events.                                                                                                                                                                                                                                                                                          | 10            | <p>INT<br>LONG</p>  | Yes      | No      |

ExamplesEXAMPLE 1

```
@Sink(type = 'tcp', url='tcp://localhost:8080/abc', sync='true' 
   @map(type='binary'))
define stream Foo (attribute1 string, attribute2 int);
```

A sink of type 'tcp' has been defined.\
All events arriving at Foo stream via TCP transport will be sent to the url tcp://localhost:8080/abc in a synchronous manner.

### Sinkmapper <a href="#sinkmapper" id="sinkmapper"></a>

#### binary [_(Sink Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink-mapper) <a href="#binary-sink-mapper" id="binary-sink-mapper"></a>

This section explains how to map events processed via Siddhi in order to publish them in the `binary` format.

_Origin: siddhi-map-binary:2.0.0_

Syntax

```
@sink(..., @map(type="binary")
```

ExamplesEXAMPLE 1

```
@sink(type='inMemory', topic='WSO2', @map(type='binary')) define stream FooStream (symbol string, price float, volume long); 
```

This will publish Siddhi event in binary format.

#### csv [_(Sink Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink-mapper) <a href="#csv-sink-mapper" id="csv-sink-mapper"></a>

This output mapper extension allows you to convert Siddhi events processed by the WSO2 SP to CSV message before publishing them. You can either use custom placeholder to map a custom CSV message or use pre-defined CSV format where event conversion takes place without extra configurations.

_Origin: siddhi-map-csv:2.0.0_

Syntax

```
@sink(..., @map(type="csv", delimiter="<STRING>", header="<BOOL>", event.grouping.enabled="<BOOL>")
```

QUERY PARAMETERS

| Name                   | Description                                                                                                                                                                                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| delimiter              | This parameter used to separate the output CSV data, when converting a Siddhi event to CSV format,                                                                                                                           | ,             | STRING              | Yes      | No      |
| header                 | This parameter specifies whether the CSV messages will be generated with header or not. If this parameter is set to true, message will be generated with header                                                              | false         | BOOL                | Yes      | No      |
| event.grouping.enabled | If this parameter is set to `true`, events are grouped via a line.separator when multiple events are received. It is required to specify a value for the System.lineSeparator() when the value for this parameter is `true`. | false         | BOOL                | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='inMemory', topic='{{symbol}}', @map(type='csv'))
define stream BarStream (symbol string, price float, volume long);
```

Above configuration will perform a default CSV output mapping, which will generate output as follows:\
&#x20;WSO2,55.6,100\<OS supported line separator>If header is true and delimiter is "-", then the output will be as follows:\
symbol-price-volume\<OS supported line separator>WSO2-55.6-100\<OS supported line separator>

EXAMPLE 2

```
@sink(type='inMemory', topic='{{symbol}}', @map(type='csv',header='true',delimiter='-',@payload(symbol='0',price='2',volume='1')))define stream BarStream (symbol string, price float,volume long); 
```

Above configuration will perform a custom CSV mapping. Here, user can add custom place order in the @payload. The place order indicates that where the attribute name's value will be appear in the output message, The output will be produced output as follows:\
WSO2,100,55.6\
If header is true and delimiter is "-", then the output will be as follows:\
symbol-price-volume\
WSO2-55.6-100\<OS supported line separator>If event grouping is enabled, then the output is as follows:\
WSO2-55.6-100\<OS supported line separator>\
WSO2-55.6-100\<OS supported line separator>\
WSO2-55.6-100\<OS supported line separator>\


#### json [_(Sink Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink-mapper) <a href="#json-sink-mapper" id="json-sink-mapper"></a>

This extension is an Event to JSON output mapper.\
Transports that publish messages can utilize this extension to convert Siddhi events to JSON messages.\
You can either send a pre-defined JSON format or a custom JSON message.\


_Origin: siddhi-map-json:5.0.1_

Syntax

```
@sink(..., @map(type="json", validate.json="<BOOL>", enclosing.element="<STRING>")
```

QUERY PARAMETERS

| Name              | Description                                                                                                                                                                                                                                                                                                                                                                                | Default Value | Possible Data Types | Optional | Dynamic |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| validate.json     | <p>If this property is set to <code>true</code>, it enables JSON validation for the JSON messages generated.<br>When validation is carried out, messages that do not adhere to proper JSON standards are dropped. This property is set to 'false' by default.<br></p>                                                                                                                      | false         | BOOL                | Yes      | No      |
| enclosing.element | <p>This specifies the enclosing element to be used if multiple events are sent in the same JSON message.<br>Siddhi treats the child elements of the given enclosing element as events and executes JSON expressions on them.<br>If an <code>enclosing.element</code> is not provided, the multiple event scenario is disregarded and JSON path is evaluated based on the root element.</p> | $             | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='inMemory', topic='stock', @map(type='json'))
define stream FooStream (symbol string, price float, volume long);
```

Above configuration does a default JSON input mapping that generates the output given below.\
{\
&#x20;   "event":{\
&#x20;       "symbol":WSO2,\
&#x20;       "price":55.6,\
&#x20;       "volume":100\
&#x20;   }\
}\


EXAMPLE 2

```
@sink(type='inMemory', topic='{{symbol}}', @map(type='json', enclosing.element='$.portfolio', validate.json='true', @payload( """{"StockData":{"Symbol":"{{symbol}}","Price":{{price}}}""")))
define stream BarStream (symbol string, price float, volume long);
```

The above configuration performs a custom JSON mapping that generates the following JSON message as the output.\
{"portfolio":{\
&#x20;   "StockData":{\
&#x20;       "Symbol":WSO2,\
&#x20;       "Price":55.6\
&#x20;     }\
&#x20; }\
}

#### keyvalue [_(Sink Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink-mapper) <a href="#keyvalue-sink-mapper" id="keyvalue-sink-mapper"></a>

The `Event to Key-Value Map` output mapper extension allows you to convert Siddhi events processed by WSO2 SP to key-value map events before publishing them. You can either use pre-defined keys where conversion takes place without extra configurations, or use custom keys with which the messages can be published.

_Origin: siddhi-map-keyvalue:2.0.0_

Syntax

```
@sink(..., @map(type="keyvalue")
```

ExamplesEXAMPLE 1

```
@sink(type='inMemory', topic='stock', @map(type='keyvalue'))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a default Key-Value output mapping. The expected output is something similar to the following:symbol:'WSO2'\
price : 55.6f\
volume: 100L

EXAMPLE 2

```
@sink(type='inMemory', topic='stock', @map(type='keyvalue', @payload(a='symbol',b='price',c='volume')))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a custom Key-Value output mapping where values are passed as objects. Values for `symbol`, `price`, and `volume` attributes are published with the keys `a`, `b` and `c` respectively. The expected output is a map similar to the following:\
a:'WSO2'\
b : 55.6f\
c: 100L

EXAMPLE 3

```
@sink(type='inMemory', topic='stock', @map(type='keyvalue', @payload(a='{{symbol}} is here',b='`price`',c='volume')))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a custom Key-Value output mapping where the values of the `a` and `b` attributes are strings and c is object.&#x20;

#### passThrough [_(Sink Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink-mapper) <a href="#passthrough-sink-mapper" id="passthrough-sink-mapper"></a>

Pass-through mapper passed events (Event\[]) through without any mapping or modifications.

_Origin: siddhi-core:5.0.0_

Syntax

```
@sink(..., @map(type="passThrough")
```

ExamplesEXAMPLE 1

```
@sink(type='inMemory', @map(type='passThrough'))
define stream BarStream (symbol string, price float, volume long);
```

In the following example BarStream uses passThrough outputmapper which emit Siddhi event directly without any transformation into sink.

#### text [_(Sink Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink-mapper) <a href="#text-sink-mapper" id="text-sink-mapper"></a>

This extension is a Event to Text output mapper. Transports that publish text messages can utilize this extension to convert the Siddhi events to text messages. Users can use a pre-defined text format where event conversion is carried out without any additional configurations, or use custom placeholder(using `{{` and `}}` or `{{{` and `}}}`) to map custom text messages. All variables are HTML escaped by default.\
For example:\
`&` is replaced with `&amp;`\
`"` is replaced with `&quot;`\
`=` is replaced with `&#61;`\
If you want to return unescaped HTML, use the triple mustache `{{{` instead of double `{{`.

_Origin: siddhi-map-text:2.0.0_

Syntax

```
@sink(..., @map(type="text", event.grouping.enabled="<BOOL>", delimiter="<STRING>", new.line.character="<STRING>")
```

QUERY PARAMETERS

| Name                   | Description                                                                                                                                                                                                                                                             | Default Value | Possible Data Types | Optional | Dynamic |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| event.grouping.enabled | If this parameter is set to `true`, events are grouped via a delimiter when multiple events are received. It is required to specify a value for the `delimiter` parameter when the value for this parameter is `true`.                                                  | false         | BOOL                | Yes      | No      |
| delimiter              | This parameter specifies how events are separated when a grouped event is received. This must be a whole line and not a single character.                                                                                                                               | \~\~\~\~      | STRING              | Yes      | No      |
| new.line.character     | This attribute indicates the new line character of the event that is expected to be received. This is used mostly when communication between 2 types of operating systems is expected. For example, Linux uses  whereas Windows uses `\r` as the end of line character. |               | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='inMemory', topic='stock', @map(type='text'))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a default text input mapping. The expected output is as follows:\
symbol:"WSO2",\
price:55.6,\
volume:100

EXAMPLE 2

```
@sink(type='inMemory', topic='stock', @map(type='text', event.grouping.enabled='true'))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a default text input mapping with event grouping. The expected output is as follows:\
symbol:"WSO2",\
price:55.6,\
volume:100\
\~\~\~\~\
symbol:"WSO2",\
price:55.6,\
volume:100

EXAMPLE 3

```
@sink(type='inMemory', topic='stock', @map(type='text',  @payload("SensorID : {{symbol}}/{{volume}}, SensorPrice : Rs{{price}}/=, Value : {{volume}}ml")))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a custom text mapping. The expected output is as follows:\
SensorID : wso2/100, SensorPrice : Rs1000/=, Value : 100ml\
for the following siddhi event.\
{wso2,1000,100}

EXAMPLE 4

```
@sink(type='inMemory', topic='stock', @map(type='text', event.grouping.enabled='true', @payload("Stock price of {{symbol}} is {{price}}")))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a custom text mapping with event grouping. The expected output is as follows:\
Stock price of WSO2 is 55.6\
\~\~\~\~\
Stock price of WSO2 is 55.6\
\~\~\~\~\
Stock price of WSO2 is 55.6\
for the following siddhi event.\
{WSO2,55.6,10}

EXAMPLE 5

```
@sink(type='inMemory', topic='stock', @map(type='text',  @payload("SensorID : {{{symbol}}}/{{{volume}}}, SensorPrice : Rs{{{price}}}/=, Value : {{{volume}}}ml")))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a custom text mapping to return unescaped HTML. The expected output is as follows:\
SensorID : a\&b/100, SensorPrice : Rs1000/=, Value : 100ml\
for the following siddhi event.\
{a\&b,1000,100}

#### xml [_(Sink Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#sink-mapper) <a href="#xml-sink-mapper" id="xml-sink-mapper"></a>

This mapper converts Siddhi output events to XML before they are published via transports that publish in XML format. Users can either send a pre-defined XML format or a custom XML message containing event data.

_Origin: siddhi-map-xml:5.0.0_

Syntax

```
@sink(..., @map(type="xml", validate.xml="<BOOL>", enclosing.element="<STRING>")
```

QUERY PARAMETERS

| Name              | Description                                                                                                                                                                                                                                                                                                                            | Default Value                                           | Possible Data Types | Optional | Dynamic |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- | ------------------- | -------- | ------- |
| validate.xml      | This parameter specifies whether the XML messages generated should be validated or not. If this parameter is set to true, messages that do not adhere to proper XML standards are dropped.                                                                                                                                             | false                                                   | BOOL                | Yes      | No      |
| enclosing.element | When an enclosing element is specified, the child elements (e.g., the immediate child elements) of that element are considered as events. This is useful when you need to send multiple events in a single XML message. When an enclosing element is not specified, one XML message per every event will be emitted without enclosing. | None in custom mapping and \<events> in default mapping | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='inMemory', topic='stock', @map(type='xml'))
define stream FooStream (symbol string, price float, volume long);
```

Above configuration will do a default XML input mapping which will generate below output\
\<events>\
&#x20;   \<event>\
&#x20;       \<symbol>WSO2\</symbol>\
&#x20;       \<price>55.6\</price>\
&#x20;       \<volume>100\</volume>\
&#x20;   \</event>\
\</events>\


EXAMPLE 2

```
@sink(type='inMemory', topic='{{symbol}}', @map(type='xml', enclosing.element='<portfolio>', validate.xml='true', @payload( "<StockData><Symbol>{{symbol}}</Symbol><Price>{{price}}</Price></StockData>")))
define stream BarStream (symbol string, price float, volume long);
```

Above configuration will perform a custom XML mapping. Inside @payload you can specify the custom template that you want to send the messages out and addd placeholders to places where you need to add event attributes.Above config will produce below output XML message\
\<portfolio>\
&#x20;   \<StockData>\
&#x20;       \<Symbol>WSO2\</Symbol>\
&#x20;       \<Price>55.6\</Price>\
&#x20;   \</StockData>\
\</portfolio>

### Source <a href="#source" id="source"></a>

#### cdc [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#cdc-source" id="cdc-source"></a>

The CDC source receives events when change events (i.e., INSERT, UPDATE, DELETE) are triggered for a database table. Events are received in the 'key-value' format.\
The key values of the map of a CDC change event are as follows.\
&#x20;   For insert: Keys are specified as columns of the table.\
&#x20;   For delete: Keys are followed followed by the specified table columns. This is achieved via 'before\_'. e.g., specifying 'before\_X' results in the key being added before the column named 'X'.\
&#x20;   For update: Keys are followed followed by the specified table columns. This is achieved via 'before\_'. e.g., specifying 'before\_X' results in the key being added before the column named 'X'.\
For 'polling' mode: Keys are specified as the coloumns of the table.\
See parameter: mode for supported databases and change events.

_Origin: siddhi-io-cdc:2.0.0_

Syntax

```
@source(type="cdc", url="<STRING>", mode="<STRING>", jdbc.driver.name="<STRING>", username="<STRING>", password="<STRING>", pool.properties="<STRING>", datasource.name="<STRING>", table.name="<STRING>", polling.column="<STRING>", polling.interval="<INT>", operation="<STRING>", connector.properties="<STRING>", database.server.id="<STRING>", database.server.name="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Default Value                        | Possible Data Types | Optional | Dynamic |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ | ------------------- | -------- | ------- |
| url                  | <p>The connection URL to the database.<br>F=The format used is: 'jdbc:mysql://&#x3C;host>:&#x3C;port>/&#x3C;database_name>'</p>                                                                                                                                                                                                                                                                                                                                                                                                                |                                      | STRING              | No       | No      |
| mode                 | <p>Mode to capture the change data. The type of events that can be received, and the required parameters differ based on the mode. The mode can be one of the following:<br>'polling': This mode uses a column named 'polling.column' to monitor the given table. It captures change events of the 'RDBMS', 'INSERT, and 'UPDATE' types.<br>'listening': This mode uses logs to monitor the given table. It currently supports change events only of the 'MySQL', 'INSERT', 'UPDATE', and 'DELETE' types.</p>                                  | listening                            | STRING              | Yes      | No      |
| jdbc.driver.name     | The driver class name for connecting the database. **It is required to specify a value for this parameter when the mode is 'polling'.**                                                                                                                                                                                                                                                                                                                                                                                                        |                                      | STRING              | Yes      | No      |
| username             | <p>The username to be used for accessing the database. This user needs to have the 'SELECT', 'RELOAD', 'SHOW DATABASES', 'REPLICATION SLAVE', and 'REPLICATION CLIENT'privileges for the change data capturing table (specified via the 'table.name' parameter).<br>To operate in the polling mode, the user needs 'SELECT' privileges.</p>                                                                                                                                                                                                    |                                      | STRING              | No       | No      |
| password             | The password of the username you specified for accessing the database.                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                      | STRING              | No       | No      |
| pool.properties      | The pool parameters for the database connection can be specified as key-value pairs.                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                      | STRING              | Yes      | No      |
| datasource.name      | <p>Name of the wso2 datasource to connect to the database. When datasource name is provided, the URL, username and password are not needed. A datasource based connection is given more priority over the URL based connection.<br> This parameter is applicable only when the mode is set to 'polling', and it can be applied only when you use this extension with WSO2 Stream Processor.</p>                                                                                                                                                |                                      | STRING              | Yes      | No      |
| table.name           | The name of the table that needs to be monitored for data changes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                      | STRING              | No       | No      |
| polling.column       | <p>The column name that is polled to capture the change data. It is recommended to have a TIMESTAMP field as the 'polling.column' in order to capture the inserts and updates.<br>Numeric auto-incremental fields and char fields can also be used as 'polling.column'. However, note that fields of these types only support insert change capturing, and the possibility of using a char field also depends on how the data is input.<br><strong>It is required to enter a value for this parameter when the mode is 'polling'.</strong></p> |                                      | STRING              | Yes      | No      |
| polling.interval     | <p>The time interval (specified in seconds) to poll the given table for changes.<br>This parameter is applicable only when the mode is set to 'polling'.</p>                                                                                                                                                                                                                                                                                                                                                                                   | 1                                    | INT                 | Yes      | No      |
| operation            | <p>The change event operation you want to carry out. Possible values are 'insert', 'update' or 'delete'. It is required to specify a value when the mode is 'listening'.<br>This parameter is not case sensitive.</p>                                                                                                                                                                                                                                                                                                                          |                                      | STRING              | No       | No      |
| connector.properties | <p>Here, you can specify Debezium connector properties as a comma-separated string.<br>The properties specified here are given more priority over the parameters. This parameter is applicable only for the 'listening' mode.</p>                                                                                                                                                                                                                                                                                                              | Empty\_String                        | STRING              | Yes      | No      |
| database.server.id   | An ID to be used when joining MySQL database cluster to read the bin log. This should be a unique integer between 1 to 2^32. This parameter is applicable only when the mode is 'listening'.                                                                                                                                                                                                                                                                                                                                                   | Random integer between 5400 and 6400 | STRING              | Yes      | No      |
| database.server.name | A logical name that identifies and provides a namespace for the database server. This parameter is applicable only when the mode is 'listening'.                                                                                                                                                                                                                                                                                                                                                                                               | {host}\_{port}                       | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@source(type = 'cdc' , url = 'jdbc:mysql://localhost:3306/SimpleDB', 
username = 'cdcuser', password = 'pswd4cdc', 
table.name = 'students', operation = 'insert', 
@map(type='keyvalue', @attributes(id = 'id', name = 'name')))
define stream inputStream (id string, name string);
```

In this example, the CDC source listens to the row insertions that are made in the 'students' table with the column name, and the ID. This table belongs to the 'SimpleDB' MySQL database that can be accessed via the given URL.

EXAMPLE 2

```
@source(type = 'cdc' , url = 'jdbc:mysql://localhost:3306/SimpleDB', 
username = 'cdcuser', password = 'pswd4cdc', 
table.name = 'students', operation = 'update', 
@map(type='keyvalue', @attributes(id = 'id', name = 'name', 
before_id = 'before_id', before_name = 'before_name')))
define stream inputStream (before_id string, id string, 
before_name string , name string);
```

In this example, the CDC source listens to the row updates that are made in the 'students' table. This table belongs to the 'SimpleDB' MySQL database that can be accessed via the given URL.

EXAMPLE 3

```
@source(type = 'cdc' , url = 'jdbc:mysql://localhost:3306/SimpleDB', 
username = 'cdcuser', password = 'pswd4cdc', 
table.name = 'students', operation = 'delete', 
@map(type='keyvalue', @attributes(before_id = 'before_id', before_name = 'before_name')))
define stream inputStream (before_id string, before_name string);
```

In this example, the CDC source listens to the row deletions made in the 'students' table. This table belongs to the 'SimpleDB' database that can be accessed via the given URL.

EXAMPLE 4

```
@source(type = 'cdc', mode='polling', polling.column = 'id', 
jdbc.driver.name = 'com.mysql.jdbc.Driver', url = 'jdbc:mysql://localhost:3306/SimpleDB', 
username = 'cdcuser', password = 'pswd4cdc', 
table.name = 'students', 
@map(type='keyvalue'), @attributes(id = 'id', name = 'name'))
define stream inputStream (id int, name string);
```

In this example, the CDC source polls the 'students' table for inserts. 'id' that is specified as the polling colum' is an auto incremental field. The connection to the database is made via the URL, username, password, and the JDBC driver name.

EXAMPLE 5

```
@source(type = 'cdc', mode='polling', polling.column = 'id', datasource.name = 'SimpleDB',
table.name = 'students', 
@map(type='keyvalue'), @attributes(id = 'id', name = 'name'))
define stream inputStream (id int, name string);
```

In this example, the CDC source polls the 'students' table for inserts. The given polling column is a char column with the 'S001, S002, ... .' pattern. The connection to the database is made via a data source named 'SimpleDB'. Note that the 'datasource.name' parameter works only with the Stream Processor.

EXAMPLE 6

```
@source(type = 'cdc', mode='polling', polling.column = 'last_updated', datasource.name = 'SimpleDB',
table.name = 'students', 
@map(type='keyvalue'))
define stream inputStream (name string);
```

In this example, the CDC source polls the 'students' table for inserts and updates. The polling column is a timestamp field.

#### email [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#email-source" id="email-source"></a>

The 'Email' source allows you to receive events via emails. An 'Email' source can be configured using the 'imap' or 'pop3' server to receive events. This allows you to filter the messages that satisfy the criteria specified under the 'search term' option. The email source parameters can be defined in either the '\<SP\_HOME>/conf/\<PROFILE>/deployment yaml' file or the stream definition. If the parameter configurations are not available in either place, the default values are considered (i.e., if default values are available). If you need to configure server system parameters that are not provided as options in the stream definition, they need to be defined in the 'deployment yaml' file under 'email source properties'. For more information about 'imap' and 'pop3' server system parameters, see the following.\
\[JavaMail Reference Implementation - IMAP Store]\(https://javaee.github.io/javamail/IMAP-Store)\
\[JavaMail Reference Implementation - POP3 Store Store]\(https://javaee.github.io/javamail/POP3-Store)

_Origin: siddhi-io-email:2.0.1_

Syntax

```
@source(type="email", username="<STRING>", password="<STRING>", store="<STRING>", host="<STRING>", port="<INT>", folder="<STRING>", search.term="<STRING>", polling.interval="<LONG>", action.after.processed="<STRING>", folder.to.move="<STRING>", content.type="<STRING>", ssl.enable="<BOOL>", @map(...)))
```

QUERY PARAMETERS

| Name                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Default Value                                                                                                                                | Possible Data Types | Optional | Dynamic |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | -------- | ------- |
| username               | The user name of the email account. e.g., 'wso2mail' is the username of the 'wso2mail@gmail.com' mail account.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                              | STRING              | No       | No      |
| password               | The password of the email account                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                                                              | STRING              | No       | No      |
| store                  | The store type that used to receive emails. Possible values are 'imap' and 'pop3'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | imap                                                                                                                                         | STRING              | Yes      | No      |
| host                   | The host name of the server (e.g., 'imap.gmail.com' is the host name for a gmail account with an IMAP store.). The default value 'imap.gmail.com' is only valid if the email account is a gmail account with IMAP enabled.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | If store type is 'imap', then the default value is 'imap.gmail.com'. If the store type is 'pop3', then thedefault value is 'pop3.gmail.com'. | STRING              | Yes      | No      |
| port                   | The port that is used to create the connection.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | '993', the default value is valid only if the store is 'imap' and ssl-enabled.                                                               | INT                 | Yes      | No      |
| folder                 | The name of the folder to which the emails should be fetched.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | INBOX                                                                                                                                        | STRING              | Yes      | No      |
| search.term            | The option that includes conditions such as key-value pairs to search for emails. In a string search term, the key and the value should be separated by a semicolon (';'). Each key-value pair must be within inverted commas (' '). The string search term can define multiple comma-separated key-value pairs. This string search term currently supports only the 'subject', 'from', 'to', 'bcc', and 'cc' keys. e.g., if you enter 'subject:DAS, from:carbon, bcc:wso2', the search term creates a search term instance that filters emails that contain 'DAS' in the subject, 'carbon' in the 'from' address, and 'wso2' in one of the 'bcc' addresses. The string search term carries out sub string matching that is case-sensitive. If '@' in included in the value for any key other than the 'subject' key, it checks for an address that is equal to the value given. e.g., If you search for 'abc@', the string search terms looks for an address that contains 'abc' before the '@' symbol. | None                                                                                                                                         | STRING              | Yes      | No      |
| polling.interval       | This defines the time interval in seconds at which th email source should poll the account to check for new mail arrivals.in seconds.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 600                                                                                                                                          | LONG                | Yes      | No      |
| action.after.processed | <p>The action to be performed by the email source for the processed mail. Possible values are as follows:<br>'FLAGGED': Sets the flag as 'flagged'.<br>'SEEN': Sets the flag as 'read'.<br>'ANSWERED': Sets the flag as 'answered'.<br>'DELETE': Deletes tha mail after the polling cycle.<br>'MOVE': Moves the mail to the folder specified in the 'folder.to.move' parameter.<br> If the folder specified is 'pop3', then the only option available is 'DELETE'.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | NONE                                                                                                                                         | STRING              | Yes      | No      |
| folder.to.move         | The name of the folder to which the mail must be moved once it is processed. If the action after processing is 'MOVE', it is required to specify a value for this parameter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                              | STRING              | No       | No      |
| content.type           | The content type of the email. It can be either 'text/plain' or 'text/html.'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | text/plain                                                                                                                                   | STRING              | Yes      | No      |
| ssl.enable             | If this is set to 'true', a secure port is used to establish the connection. The possible values are 'true' and 'false'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | true                                                                                                                                         | BOOL                | Yes      | No      |

System Parameters

| Name                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                | Default Value                                                           | Possible Parameters       |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ------------------------- |
| mail.imap.partialfetch            | This determines whether the IMAP partial-fetch capability should be used.                                                                                                                                                                                                                                                                                                                                                                                  | true                                                                    | true or false             |
| mail.imap.fetchsize               | The partial fetch size in bytes.                                                                                                                                                                                                                                                                                                                                                                                                                           | 16K                                                                     | value in bytes            |
| mail.imap.peek                    | If this is set to 'true', the IMAP PEEK option should be used when fetching body parts to avoid setting the 'SEEN' flag on messages. The default value is 'false'. This can be overridden on a per-message basis by the 'setPeek method' in 'IMAPMessage'.                                                                                                                                                                                                 | false                                                                   | true or false             |
| mail.imap.connectiontimeout       | The socket connection timeout value in milliseconds. This timeout is implemented by 'java.net.Socket'.                                                                                                                                                                                                                                                                                                                                                     | infinity timeout                                                        | Any Integer value         |
| mail.imap.timeout                 | The socket read timeout value in milliseconds. This timeout is implemented by 'java.net.Socket'.                                                                                                                                                                                                                                                                                                                                                           | infinity timeout                                                        | Any Integer value         |
| mail.imap.writetimeout            | The socket write timeout value in milliseconds. This timeout is implemented by using a 'java.util.concurrent.ScheduledExecutorService' per connection that schedules a thread to close the socket if the timeout period elapses. Therefore, the overhead of using this timeout is one thread per connection.                                                                                                                                               | infinity timeout                                                        | Any Integer value         |
| mail.imap.statuscachetimeout      | The timeout value in milliseconds for the cache of 'STATUS' command response.                                                                                                                                                                                                                                                                                                                                                                              | 1000ms                                                                  | Time out in miliseconds   |
| mail.imap.appendbuffersize        | The maximum size of a message to buffer in memory when appending to an IMAP folder.                                                                                                                                                                                                                                                                                                                                                                        | None                                                                    | Any Integer value         |
| mail.imap.connectionpoolsize      | The maximum number of available connections in the connection pool.                                                                                                                                                                                                                                                                                                                                                                                        | 1                                                                       | Any Integer value         |
| mail.imap.connectionpooltimeout   | The timeout value in milliseconds for connection pool connections.                                                                                                                                                                                                                                                                                                                                                                                         | 45000ms                                                                 | Any Integer               |
| mail.imap.separatestoreconnection | If this parameter is set to 'true', it indicates that a dedicated store connection needs to be used for store commands.                                                                                                                                                                                                                                                                                                                                    | true                                                                    | true or false             |
| mail.imap.auth.login.disable      | If this is set to 'true', it is not possible to use the non-standard 'AUTHENTICATE LOGIN' command instead of the plain 'LOGIN' command.                                                                                                                                                                                                                                                                                                                    | false                                                                   | true or false             |
| mail.imap.auth.plain.disable      | If this is set to 'true', the 'AUTHENTICATE PLAIN' command cannot be used.                                                                                                                                                                                                                                                                                                                                                                                 | false                                                                   | true or false             |
| mail.imap.auth.ntlm.disable       | If true, prevents use of the AUTHENTICATE NTLM command.                                                                                                                                                                                                                                                                                                                                                                                                    | false                                                                   | true or false             |
| mail.imap.proxyauth.user          | If the server supports the PROXYAUTH extension, this property specifies the name of the user to act as. Authentication to log in to the server is carried out using the administrator's credentials. After authentication, the IMAP provider issues the 'PROXYAUTH' command with the user name specified in this property.                                                                                                                                 | None                                                                    | Valid string value        |
| mail.imap.localaddress            | The local address (host name) to bind to when creating the IMAP socket.                                                                                                                                                                                                                                                                                                                                                                                    | Defaults to the address picked by the Socket class.                     | Valid string value        |
| mail.imap.localport               | The local port number to bind to when creating the IMAP socket.                                                                                                                                                                                                                                                                                                                                                                                            | Defaults to the port number picked by the Socket class.                 | Valid String value        |
| mail.imap.sasl.enable             | If this parameter is set to 'true', the system attempts to use the 'javax.security.sasl' package to choose an authentication mechanism for the login.                                                                                                                                                                                                                                                                                                      | false                                                                   | true or false             |
| mail.imap.sasl.mechanisms         | A list of SASL mechanism names that the system should to try to use. The names can be separated by spaces or commas.                                                                                                                                                                                                                                                                                                                                       | None                                                                    | Valid string value        |
| mail.imap.sasl.authorizationid    | The authorization ID to use in the SASL authentication.                                                                                                                                                                                                                                                                                                                                                                                                    | If this parameter is not set, the authentication ID (username) is used. | Valid string value        |
| mail.imap.sasl.realm              | The realm to use with SASL authentication mechanisms that require a realm, such as 'DIGEST-MD5'.                                                                                                                                                                                                                                                                                                                                                           | None                                                                    | Valid string value        |
| mail.imap.auth.ntlm.domain        | The NTLM authentication domain.                                                                                                                                                                                                                                                                                                                                                                                                                            | None                                                                    | Valid string value        |
| The NTLM authentication domain.   | NTLM protocol-specific flags.                                                                                                                                                                                                                                                                                                                                                                                                                              | None                                                                    | Valid integer value       |
| mail.imap.socketFactory           | If this parameter is set to a class that implements the 'javax.net.SocketFactory' interface, this class is used to create IMAP sockets.                                                                                                                                                                                                                                                                                                                    | None                                                                    | Valid SocketFactory       |
| mail.imap.socketFactory.class     | If this parameter is set, it specifies the name of a class that implements the 'javax.net.SocketFactory' interface. This class is used to create IMAP sockets.                                                                                                                                                                                                                                                                                             | None                                                                    | Valid string              |
| mail.imap.socketFactory.fallback  | If this parameter is set to 'true', failure to create a socket using the specified socket factory class results in the socket being created using the 'java.net.Socket' class.                                                                                                                                                                                                                                                                             | true                                                                    | true or false             |
| mail.imap.socketFactory.port      | This specifies the port to connect to when using the specified socket factory. If this parameter is not set, the default port is used.                                                                                                                                                                                                                                                                                                                     | 143                                                                     | Valid Integer             |
| mail.imap.ssl.checkserveridentity | If this parameter is set to 'true', the system checks the server identity as specified by RFC 2595.                                                                                                                                                                                                                                                                                                                                                        | false                                                                   | true or false             |
| mail.imap.ssl.trust               | <p>If this parameter is set and a socket factory has not been specified, it enables the use of a 'MailSSLSocketFactory'.<br>If this parameter is set to '<em>', all the hosts are trusted.</em><br><em>If this parameter specifies list of hosts separated by white spaces, only those hosts are trusted.</em><br><em>If the parameter is not set to any of the values mentioned above, trust depends on the certificate presented by the server.</em></p> |                                                                         | Valid String              |
| mail.imap.ssl.socketFactory       | If this parameter is set to a class that extends the 'javax.net.ssl.SSLSocketFactory' class this class is used to create IMAP SSL sockets.                                                                                                                                                                                                                                                                                                                 | None                                                                    | SSL Socket Factory        |
| mail.imap.ssl.socketFactory.class | If this parameter is set, it specifies the name of a class that extends the 'javax.net.ssl.SSLSocketFactory' class. This class is used to create IMAP SSL sockets.                                                                                                                                                                                                                                                                                         | None                                                                    | Valid String              |
| mail.imap.ssl.socketFactory.port  | This specifies the port to connect to when using the specified socket factory.                                                                                                                                                                                                                                                                                                                                                                             | the default port 993 is used.                                           | valid port number         |
| mail.imap.ssl.protocols           | This specifies the SSL protocols that are enabled for SSL connections. The property value is a whitespace-separated list of tokens acceptable to the 'javax.net.ssl.SSLSocket.setEnabledProtocols' method.                                                                                                                                                                                                                                                 | None                                                                    | Valid string              |
| mail.imap.starttls.enable         | If this parameter is set to 'true', it is possible to use the 'STARTTLS' command (if supported by the server) to switch the connection to a TLS-protected connection before issuing any login commands.                                                                                                                                                                                                                                                    | false                                                                   | true or false             |
| mail.imap.socks.host              | This specifies the host name of a 'SOCKS5' proxy server that is used to connect to the mail server.                                                                                                                                                                                                                                                                                                                                                        | None                                                                    | Valid String              |
| mail.imap.socks.port              | This specifies the port number for the 'SOCKS5' proxy server. This is needed if the proxy server is not using the standard port number 1080.                                                                                                                                                                                                                                                                                                               | 1080                                                                    | Valid String              |
| mail.imap.minidletime             | This property sets the delay in milliseconds.                                                                                                                                                                                                                                                                                                                                                                                                              | 10 milliseconds                                                         | time in seconds (Integer) |
| mail.imap.enableimapevents        | If this property is set to 'true', it enables special IMAP-specific events to be delivered to the 'ConnectionListener' of the store. The unsolicited responses received during the idle method of the store are sent as connection events with 'IMAPStore.RESPONSE' as the type. The event's message is the raw IMAP response string.                                                                                                                      | false                                                                   | true or false             |
| mail.imap.folder.class            | The class name of a subclass of 'com.sun.mail.imap.IMAPFolder'. The subclass can be used to provide support for additional IMAP commands. The subclass must have public constructors of the form 'public MyIMAPFolder'(String fullName, char separator, IMAPStore store, Boolean isNamespace) and public 'MyIMAPFolder'(ListInfo li, IMAPStore store)                                                                                                      | None                                                                    | Valid String              |
| mail.pop3.connectiontimeout       | The socket connection timeout value in milliseconds.                                                                                                                                                                                                                                                                                                                                                                                                       | Infinite timeout                                                        | Integer value             |
| mail.pop3.timeout                 | The socket I/O timeout value in milliseconds.                                                                                                                                                                                                                                                                                                                                                                                                              | Infinite timeout                                                        | Integer value             |
| mail.pop3.message.class           | The class name of a subclass of 'com.sun.mail.pop3.POP3Message'.                                                                                                                                                                                                                                                                                                                                                                                           | None                                                                    | Valid String              |
| mail.pop3.localaddress            | The local address (host name) to bind to when creating the POP3 socket.                                                                                                                                                                                                                                                                                                                                                                                    | Defaults to the address picked by the Socket class.                     | Valid String              |
| mail.pop3.localport               | The local port number to bind to when creating the POP3 socket.                                                                                                                                                                                                                                                                                                                                                                                            | Defaults to the port number picked by the Socket class.                 | Valid port number         |
| mail.pop3.apop.enable             | If this parameter is set to 'true', use 'APOP' instead of 'USER/PASS' to log in to the 'POP3' server (if the 'POP3' server supports 'APOP'). APOP sends a digest of the password instead of clearing the text password.                                                                                                                                                                                                                                    | false                                                                   | true or false             |
| mail.pop3.socketFactory           | If this parameter is set to a class that implements the 'javax.net.SocketFactory' interface, this class is used to create 'POP3' sockets.                                                                                                                                                                                                                                                                                                                  | None                                                                    | Socket Factory            |
| mail.pop3.socketFactory.class     | If this parameter is set, it specifies the name of a class that implements the 'javax.net.SocketFactory' interface. This class is used to create 'POP3' sockets.                                                                                                                                                                                                                                                                                           | None                                                                    | Valid String              |
| mail.pop3.socketFactory.fallback  | If this parameter is set to 'true', failure to create a socket using the specified socket factory class results in the socket being created using the 'java.net.Socket' class.                                                                                                                                                                                                                                                                             | false                                                                   | true or false             |
| mail.pop3.socketFactory.port      | This specifies the port to connect to when using the specified socket factory.                                                                                                                                                                                                                                                                                                                                                                             | Default port                                                            | Valid port number         |
| mail.pop3.ssl.checkserveridentity | If this parameter is set to 'true', check the server identity as specified by RFC 2595.                                                                                                                                                                                                                                                                                                                                                                    | false                                                                   | true or false             |
| mail.pop3.ssl.trust               | <p>If this parameter is set and a socket factory has not been specified, it is possible to use a 'MailSSLSocketFactory'.<br>If this parameter is set to '<em>', all the hosts are trusted.</em><br><em>If the parameter is set to a whitespace-separated list of hosts, only those hosts are trusted.</em><br><em>If the parameter is not set to any of the values mentioned above, trust depends on the certificate presented by the server.</em></p>     |                                                                         | Valid String              |
| mail.pop3.ssl.socketFactory       | If this parameter is set to a class that extends the 'javax.net.ssl.SSLSocketFactory' class, this class is used to create 'POP3' SSL sockets.                                                                                                                                                                                                                                                                                                              | None                                                                    | SSL Socket Factory        |
| mail.pop3.ssl.checkserveridentity | If this parameter is set to 'true', the system checks the server identity as specified by 'RFC 2595'.                                                                                                                                                                                                                                                                                                                                                      | false                                                                   | true or false             |
| mail.pop3.ssl.trust               | <p>If this parameter is set and a socket factory has not been specified, it is possible to use a 'MailSSLSocketFactory'.<br>If this parameter is set to '*', all the hosts are trusted.<br>If the parameter is set to a whitespace-separated list of hosts, only those hosts are trusted.</p>                                                                                                                                                              | Trust depends on the certificate presented by the server.               | Valid String              |
| mail.pop3.ssl.socketFactory       | If this parameter is set to a class that extends the 'javax.net.ssl.SSLSocketFactory' class, this class is used to create 'POP3 SSL' sockets.                                                                                                                                                                                                                                                                                                              | None                                                                    | SSL Socket Factory        |
| mail.pop3.ssl.socketFactory.class | If this parameter is set, it specifies the name of a class that extends the 'javax.net.ssl.SSLSocketFactory' class. This class is used to create 'POP3 SSL' sockets.                                                                                                                                                                                                                                                                                       | None                                                                    | Valid String              |
| mail.pop3.ssl.socketFactory.p     | This parameter pecifies the port to connect to when using the specified socket factory.                                                                                                                                                                                                                                                                                                                                                                    | 995                                                                     | Valid Integer             |
| mail.pop3.ssl.protocols           | This parameter specifies the SSL protocols that are enabled for SSL connections. The property value is a whitespace-separated list of tokens acceptable to the 'javax.net.ssl.SSLSocket.setEnabledProtocols' method.                                                                                                                                                                                                                                       | None                                                                    | Valid String              |
| mail.pop3.starttls.enable         | If this parameter is set to 'true', it is possible to use the 'STLS' command (if supported by the server) to switch the connection to a TLS-protected connection before issuing any login commands.                                                                                                                                                                                                                                                        | false                                                                   | true or false             |
| mail.pop3.starttls.required       | If this parameter is set to 'true', it is required to use the 'STLS' command. The connect method fails if the server does not support the 'STLS' command or if the command fails.                                                                                                                                                                                                                                                                          | false                                                                   | true or false             |
| mail.pop3.socks.host              | This parameter specifies the host name of a 'SOCKS5' proxy server that can be used to connect to the mail server.                                                                                                                                                                                                                                                                                                                                          | None                                                                    | Valid String              |
| mail.pop3.socks.port              | This parameter specifies the port number for the 'SOCKS5' proxy server.                                                                                                                                                                                                                                                                                                                                                                                    | None                                                                    | Valid String              |
| mail.pop3.disabletop              | If this parameter is set to 'true', the 'POP3 TOP' command is not used to fetch message headers.                                                                                                                                                                                                                                                                                                                                                           | false                                                                   | true or false             |
| mail.pop3.forgettopheaders        | If this parameter is set to 'true', the headers that might have been retrieved using the 'POP3 TOP' command is forgotten and replaced by the headers retrieved when the 'POP3 RETR' command is executed.                                                                                                                                                                                                                                                   | false                                                                   | true or false             |
| mail.pop3.filecache.enable        | If this parameter is set to 'true', the 'POP3' provider caches message data in a temporary file instead of caching them in memory. Messages are only added to the cache when accessing the message content. Message headers are always cached in memory (on demand). The file cache is removed when the folder is closed or the JVM terminates.                                                                                                            | false                                                                   | true or false             |
| mail.pop3.filecache.dir           | If the file cache is enabled, this property is used to override the default directory used by the JDK for temporary files.                                                                                                                                                                                                                                                                                                                                 | None                                                                    | Valid String              |
| mail.pop3.cachewriteto            | This parameter controls the behavior of the 'writeTo' method on a 'POP3' message object. If the parameter is set to 'true', the message content has not been cached yet, and the 'ignoreList' is null, the message is cached before being written. If not, the message is streamed directly to the output stream without being cached.                                                                                                                     | false                                                                   | true or false             |
| mail.pop3.keepmessagecontent      | If this property is set to 'true', a hard reference to the cached content is retained, preventing the memory from being reused until the folder is closed, or until the cached content is explicitly invalidated (using the 'invalidate' method).                                                                                                                                                                                                          | false                                                                   | true or false             |

ExamplesEXAMPLE 1

```
@source(type='email', @map(type='xml'), username='receiver.account', password='account.password',)define stream inputStream (name string, age int, country string);
```

This example illustrates how to receive events in 'xml' format via the email source. In this example, only the required parameters are defined in the stream definition. The default values are taken for the other parameters. The search term is not defined, and therefore, all the new messages in the inbox folder are polled and taken.

EXAMPLE 2

```
@source(type='email', @map(type='xml'), username='receiver.account', password='account.password',store = 'imap',host = 'imap.gmail.com',port = '993',searchTerm = 'subject:Stream Processor, from: from.account@ , cc: cc.account',polling.interval='500',action.after.processed='DELETE',content.type='text/html,)define stream inputStream (name string, age int, country string);
```

This example illustrates how to receive events in 'xml' format via the email source. The email source polls the mail account every 500 seconds to check whether any new mails have arrived. It processes new mails only if they satisfy the conditions specified for the email search term (the value for 'from' of the email message should be 'from.account@.\<host name>', and the message should contain 'cc.account' in the cc receipient list and the word 'Stream Processor' in the mail subject). in this example, the action after processing is 'DELETE'. Therefore,after processing the event, corresponding mail is deleted from the mail folder.

#### http [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#http-source" id="http-source"></a>

The HTTP source receives POST requests via HTTP or HTTPS in format such as `text`, `XML` and `JSON`. In WSO2 SP, if required, you can enable basic authentication to ensure that events are received only from users who are authorized to access the service.

_Origin: siddhi-io-http:2.0.4_

Syntax

```
@source(type="http", receiver.url="<STRING>", basic.auth.enabled="<STRING>", worker.count="<INT>", socket.idle.timeout="<INT>", ssl.verify.client="<STRING>", ssl.protocol="<STRING>", tls.store.type="<STRING>", parameters="<STRING>", ciphers="<STRING>", ssl.enabled.protocols="<STRING>", server.enable.session.creation="<STRING>", server.supported.snimatchers="<STRING>", server.suported.server.names="<STRING>", request.size.validation.configuration="<STRING>", request.size.validation="<STRING>", request.size.validation.maximum.value="<STRING>", request.size.validation.reject.status.code="<STRING>", request.size.validation.reject.message="<STRING>", request.size.validation.reject.message.content.type="<STRING>", header.size.validation="<STRING>", header.validation.maximum.request.line="<STRING>", header.validation.maximum.size="<STRING>", header.validation.maximum.chunk.size="<STRING>", header.validation.reject.status.code="<STRING>", header.validation.reject.message="<STRING>", header.validation.reject.message.content.type="<STRING>", server.bootstrap.configuration="<OBJECT>", server.bootstrap.nodelay="<BOOL>", server.bootstrap.keepalive="<BOOL>", server.bootstrap.sendbuffersize="<INT>", server.bootstrap.recievebuffersize="<INT>", server.bootstrap.connect.timeout="<INT>", server.bootstrap.socket.reuse="<BOOL>", server.bootstrap.socket.timeout="<BOOL>", server.bootstrap.socket.backlog="<BOOL>", trace.log.enabled="<BOOL>", @map(...)))
```

QUERY PARAMETERS

| Name                                                | Description                                                                                                                                                                                                                                                                                                                                            | Default Value                                                                                                   | Possible Data Types | Optional | Dynamic |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------- | ------------------- | -------- | ------- |
| receiver.url                                        | The URL to which the events should be received. User can provide any valid url and if the url is not provided the system will use the following format `http://0.0.0.0:9763/<appNAme>/<streamName>`If the user want to use SSL the url should be given in following format `https://localhost:8080/<streamName>`                                       | http://0.0.0.0:9763//                                                                                           | STRING              | Yes      | No      |
| basic.auth.enabled                                  | This works only in WSO2 SP. If this is set to `true`, basic authentication is enabled for incoming events, and the credentials with which each event is sent are verified to ensure that the user is authorized to access the service. If basic authentication fails, the event is not authenticated and an authentication error is logged in the CLI. | false                                                                                                           | STRING              | Yes      | No      |
| worker.count                                        | The number of active worker threads to serve the incoming events. The value is 1 by default. This will ensure that the events are directed to the event stream in the same order in which they arrive. By increasing this value the performance might increase at the cost of loosing event ordering.                                                  | 1                                                                                                               | INT                 | Yes      | No      |
| socket.idle.timeout                                 | Idle timeout for HTTP connection.                                                                                                                                                                                                                                                                                                                      | 120000                                                                                                          | INT                 | Yes      | No      |
| ssl.verify.client                                   | The type of client certificate verification.                                                                                                                                                                                                                                                                                                           | null                                                                                                            | STRING              | Yes      | No      |
| ssl.protocol                                        | ssl/tls related options                                                                                                                                                                                                                                                                                                                                | TLS                                                                                                             | STRING              | Yes      | No      |
| tls.store.type                                      | TLS store type.                                                                                                                                                                                                                                                                                                                                        | JKS                                                                                                             | STRING              | Yes      | No      |
| parameters                                          | Parameters other than basics such as ciphers,sslEnabledProtocols,client.enable.session.creation. Expected format of these parameters is as follows: "'ciphers:xxx','sslEnabledProtocols,client.enable:xxx'"                                                                                                                                            | null                                                                                                            | STRING              | Yes      | No      |
| ciphers                                             | List of ciphers to be used. This parameter should include under parameters Ex: 'ciphers:TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256'                                                                                                                                                                                                                  | null                                                                                                            | STRING              | Yes      | No      |
| ssl.enabled.protocols                               | SSL/TLS protocols to be enabled. This parameter should be in camel case format(sslEnabledProtocols) under parameters. Ex 'sslEnabledProtocols:true'                                                                                                                                                                                                    | null                                                                                                            | STRING              | Yes      | No      |
| server.enable.session.creation                      | Enable HTTP session creation.This parameter should include under parameters Ex: 'client.enable.session.creation:true'                                                                                                                                                                                                                                  | null                                                                                                            | STRING              | Yes      | No      |
| server.supported.snimatchers                        | Http SNIMatcher to be added. This parameter should include under parameters Ex: 'server.supported.snimatchers:SNIMatcher'                                                                                                                                                                                                                              | null                                                                                                            | STRING              | Yes      | No      |
| server.suported.server.names                        | Http supported servers. This parameter should include under parameters Ex: 'server.suported.server.names:server'                                                                                                                                                                                                                                       | null                                                                                                            | STRING              | Yes      | No      |
| request.size.validation.configuration               | Parameters that responsible for validating the http request and request headers. Expected format of these parameters is as follows: "'request.size.validation:xxx','request.size.validation.maximum.value:xxx'"                                                                                                                                        | null                                                                                                            | STRING              | Yes      | No      |
| request.size.validation                             | To enable the request size validation.                                                                                                                                                                                                                                                                                                                 | false                                                                                                           | STRING              | Yes      | No      |
| request.size.validation.maximum.value               | If request size is validated then maximum size.                                                                                                                                                                                                                                                                                                        | Integer.MAX\_VALUE                                                                                              | STRING              | Yes      | No      |
| request.size.validation.reject.status.code          | If request is exceed maximum size and request.size.validation is enabled then status code to be send as response.                                                                                                                                                                                                                                      | 401                                                                                                             | STRING              | Yes      | No      |
| request.size.validation.reject.message              | If request is exceed maximum size and request.size.validation is enabled then status message to be send as response.                                                                                                                                                                                                                                   | Message is bigger than the valid size                                                                           | STRING              | Yes      | No      |
| request.size.validation.reject.message.content.type | If request is exceed maximum size and request.size.validation is enabled then content type to be send as response.                                                                                                                                                                                                                                     | plain/text                                                                                                      | STRING              | Yes      | No      |
| header.size.validation                              | To enable the header size validation.                                                                                                                                                                                                                                                                                                                  | false                                                                                                           | STRING              | Yes      | No      |
| header.validation.maximum.request.line              | If header header validation is enabled then the maximum request line.                                                                                                                                                                                                                                                                                  | 4096                                                                                                            | STRING              | Yes      | No      |
| header.validation.maximum.size                      | If header header validation is enabled then the maximum expected header size.                                                                                                                                                                                                                                                                          | 8192                                                                                                            | STRING              | Yes      | No      |
| header.validation.maximum.chunk.size                | If header header validation is enabled then the maximum expected chunk size.                                                                                                                                                                                                                                                                           | 8192                                                                                                            | STRING              | Yes      | No      |
| header.validation.reject.status.code                | 401                                                                                                                                                                                                                                                                                                                                                    | If header is exceed maximum size and header.size.validation is enabled then status code to be send as response. | STRING              | Yes      | No      |
| header.validation.reject.message                    | If header is exceed maximum size and header.size.validation is enabled then message to be send as response.                                                                                                                                                                                                                                            | Message header is bigger than the valid size                                                                    | STRING              | Yes      | No      |
| header.validation.reject.message.content.type       | If header is exceed maximum size and header.size.validation is enabled then content type to be send as response.                                                                                                                                                                                                                                       | plain/text                                                                                                      | STRING              | Yes      | No      |
| server.bootstrap.configuration                      | Parameters that for bootstrap configurations of the server. Expected format of these parameters is as follows: "'ciphers:xxx','sslEnabledProtocols,client.enable:xxx'"                                                                                                                                                                                 | null                                                                                                            | OBJECT              | Yes      | No      |
| server.bootstrap.nodelay                            | Http server no delay.                                                                                                                                                                                                                                                                                                                                  | true                                                                                                            | BOOL                | Yes      | No      |
| server.bootstrap.keepalive                          | Http server keep alive.                                                                                                                                                                                                                                                                                                                                | true                                                                                                            | BOOL                | Yes      | No      |
| server.bootstrap.sendbuffersize                     | Http server send buffer size.                                                                                                                                                                                                                                                                                                                          | 1048576                                                                                                         | INT                 | Yes      | No      |
| server.bootstrap.recievebuffersize                  | Http server receive buffer size.                                                                                                                                                                                                                                                                                                                       | 1048576                                                                                                         | INT                 | Yes      | No      |
| server.bootstrap.connect.timeout                    | Http server connection timeout.                                                                                                                                                                                                                                                                                                                        | 15000                                                                                                           | INT                 | Yes      | No      |
| server.bootstrap.socket.reuse                       | To enable http socket reuse.                                                                                                                                                                                                                                                                                                                           | false                                                                                                           | BOOL                | Yes      | No      |
| server.bootstrap.socket.timeout                     | Http server socket timeout.                                                                                                                                                                                                                                                                                                                            | 15                                                                                                              | BOOL                | Yes      | No      |
| server.bootstrap.socket.backlog                     | THttp server socket backlog.                                                                                                                                                                                                                                                                                                                           | 100                                                                                                             | BOOL                | Yes      | No      |
| trace.log.enabled                                   | Http traffic monitoring.                                                                                                                                                                                                                                                                                                                               | false                                                                                                           | BOOL                | Yes      | No      |

System Parameters

| Name                           | Description                                                                                                                                                                                                               | Default Value                                    | Possible Parameters         |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | --------------------------- |
| serverBootstrapBossGroupSize   | property to configure number of boss threads, which accepts incoming connections until the ports are unbound. Once connection accepts successfully, boss thread passes the accepted channel to one of the worker threads. | Number of available processors                   | Any integer                 |
| serverBootstrapWorkerGroupSize | property to configure number of worker threads, which performs non blocking read and write for one or more channels in non-blocking mode.                                                                                 | (Number of available processors)\*2              | Any integer                 |
| serverBootstrapClientGroupSize | property to configure number of client threads, which performs non blocking read and write for one or more channels in non-blocking mode.                                                                                 | (Number of available processors)\*2              | Any integer                 |
| defaultHost                    | The default host of the transport.                                                                                                                                                                                        | 0.0.0.0                                          | Any valid host              |
| defaultHttpPort                | The default port if the default scheme is 'http'.                                                                                                                                                                         | 8280                                             | Any valid port              |
| defaultHttpsPort               | The default port if the default scheme is 'https'.                                                                                                                                                                        | 8243                                             | Any valid port              |
| defaultScheme                  | The default protocol.                                                                                                                                                                                                     | http                                             | <p>http<br>https</p>        |
| keyStoreLocation               | The default keystore file path.                                                                                                                                                                                           | ${carbon.home}/resources/security/wso2carbon.jks | Path to wso2carbon.jks file |
| keyStorePassword               | The default keystore password.                                                                                                                                                                                            | wso2carbon                                       | String of keystore password |

ExamplesEXAMPLE 1

```
@source(type='http', receiver.url='http://localhost:9055/endpoints/RecPro', socketIdleTimeout='150000', parameters="'ciphers : TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256', 'sslEnabledProtocols:TLSv1.1,TLSv1.2'",request.size.validation.configuration="request.size.validation:true",server.bootstrap.configuration="server.bootstrap.socket.timeout:25" @map(type='xml'))
define stream FooStream (symbol string, price float, volume long);
```

Above source listenerConfiguration performs a default XML input mapping. The expected input is as follows:\
\<events>\
&#x20;   \<event>\
&#x20;       \<symbol>WSO2\</symbol>\
&#x20;       \<price>55.6\</price>\
&#x20;       \<volume>100\</volume>\
&#x20;   \</event>\
\</events>\
If basic authentication is enabled via the `basic.auth.enabled='true` setting, each input event is also expected to contain the `Authorization:'Basic encodeBase64(username:Password)'` header.

#### http-request [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#http-request-source" id="http-request-source"></a>

The HTTP request is correlated with the HTTP response sink, through a unique `source.id`, and for each POST requests it receives via HTTP or HTTPS in format such as `text`, `XML` and `JSON` it sends the response via the HTTP response sink. The individual request and response messages are correlated at the sink using the `message.id` of the events. If required, you can enable basic authentication at the source to ensure that events are received only from users who are authorized to access the service.

_Origin: siddhi-io-http:2.0.4_

Syntax

```
@source(type="http-request", receiver.url="<STRING>", source.id="<STRING>", connection.timeout="<INT>", basic.auth.enabled="<STRING>", worker.count="<INT>", socket.idle.timeout="<INT>", ssl.verify.client="<STRING>", ssl.protocol="<STRING>", tls.store.type="<STRING>", parameters="<STRING>", ciphers="<STRING>", ssl.enabled.protocols="<STRING>", server.enable.session.creation="<STRING>", server.supported.snimatchers="<STRING>", server.suported.server.names="<STRING>", request.size.validation.configuration="<STRING>", request.size.validation="<STRING>", request.size.validation.maximum.value="<STRING>", request.size.validation.reject.status.code="<STRING>", request.size.validation.reject.message="<STRING>", request.size.validation.reject.message.content.type="<STRING>", header.size.validation="<STRING>", header.validation.maximum.request.line="<STRING>", header.validation.maximum.size="<STRING>", header.validation.maximum.chunk.size="<STRING>", header.validation.reject.status.code="<STRING>", header.validation.reject.message="<STRING>", header.validation.reject.message.content.type="<STRING>", server.bootstrap.configuration="<OBJECT>", server.bootstrap.nodelay="<BOOL>", server.bootstrap.keepalive="<BOOL>", server.bootstrap.sendbuffersize="<INT>", server.bootstrap.recievebuffersize="<INT>", server.bootstrap.connect.timeout="<INT>", server.bootstrap.socket.reuse="<BOOL>", server.bootstrap.socket.timeout="<BOOL>", server.bootstrap.socket.backlog="<BOOL>", trace.log.enabled="<BOOL>", @map(...)))
```

QUERY PARAMETERS

| Name                                                | Description                                                                                                                                                                                                                                                                                                                                               | Default Value                                                                                                   | Possible Data Types | Optional | Dynamic |
| --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ------------------- | -------- | ------- |
| receiver.url                                        | The URL to which the events should be received. User can provide any valid url and if the url is not provided the system will use the following format `http://0.0.0.0:9763/<appNAme>/<streamName>`If the user want to use SSL the url should be given in following format `https://localhost:8080/<streamName>`                                          | http://0.0.0.0:9763//                                                                                           | STRING              | Yes      | No      |
| source.id                                           | Identifier need to map the source to sink.                                                                                                                                                                                                                                                                                                                |                                                                                                                 | STRING              | No       | No      |
| connection.timeout                                  | Connection timeout in milliseconds. If the mapped http-response sink does not get a correlated message, after this timeout value, a timeout response is sent                                                                                                                                                                                              | 120000                                                                                                          | INT                 | Yes      | No      |
| basic.auth.enabled                                  | If this is set to `true`, basic authentication is enabled for incoming events, and the credentials with which each event is sent are verified to ensure that the user is authorized to access the service. If basic authentication fails, the event is not authenticated and an authentication error is logged in the CLI. By default this values 'false' | false                                                                                                           | STRING              | Yes      | No      |
| worker.count                                        | The number of active worker threads to serve the incoming events. The value is 1 by default. This will ensure that the events are directed to the event stream in the same order in which they arrive. By increasing this value the performance might increase at the cost of loosing event ordering.                                                     | 1                                                                                                               | INT                 | Yes      | No      |
| socket.idle.timeout                                 | Idle timeout for HTTP connection.                                                                                                                                                                                                                                                                                                                         | 120000                                                                                                          | INT                 | Yes      | No      |
| ssl.verify.client                                   | The type of client certificate verification.                                                                                                                                                                                                                                                                                                              | null                                                                                                            | STRING              | Yes      | No      |
| ssl.protocol                                        | ssl/tls related options                                                                                                                                                                                                                                                                                                                                   | TLS                                                                                                             | STRING              | Yes      | No      |
| tls.store.type                                      | TLS store type.                                                                                                                                                                                                                                                                                                                                           | JKS                                                                                                             | STRING              | Yes      | No      |
| parameters                                          | Parameters other than basics such as ciphers,sslEnabledProtocols,client.enable.session.creation. Expected format of these parameters is as follows: "'ciphers:xxx','sslEnabledProtocols,client.enable:xxx'"                                                                                                                                               | null                                                                                                            | STRING              | Yes      | No      |
| ciphers                                             | List of ciphers to be used. This parameter should include under parameters Ex: 'ciphers:TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256'                                                                                                                                                                                                                     | null                                                                                                            | STRING              | Yes      | No      |
| ssl.enabled.protocols                               | SSL/TLS protocols to be enabled. This parameter should be in camel case format(sslEnabledProtocols) under parameters. Ex 'sslEnabledProtocols:true'                                                                                                                                                                                                       | null                                                                                                            | STRING              | Yes      | No      |
| server.enable.session.creation                      | Enable HTTP session creation.This parameter should include under parameters Ex: 'client.enable.session.creation:true'                                                                                                                                                                                                                                     | null                                                                                                            | STRING              | Yes      | No      |
| server.supported.snimatchers                        | Http SNIMatcher to be added. This parameter should include under parameters Ex: 'server.supported.snimatchers:SNIMatcher'                                                                                                                                                                                                                                 | null                                                                                                            | STRING              | Yes      | No      |
| server.suported.server.names                        | Http supported servers. This parameter should include under parameters Ex: 'server.suported.server.names:server'                                                                                                                                                                                                                                          | null                                                                                                            | STRING              | Yes      | No      |
| request.size.validation.configuration               | Parameters that responsible for validating the http request and request headers. Expected format of these parameters is as follows: "'request.size.validation:xxx','request.size.validation.maximum.value:xxx'"                                                                                                                                           | null                                                                                                            | STRING              | Yes      | No      |
| request.size.validation                             | To enable the request size validation.                                                                                                                                                                                                                                                                                                                    | false                                                                                                           | STRING              | Yes      | No      |
| request.size.validation.maximum.value               | If request size is validated then maximum size.                                                                                                                                                                                                                                                                                                           | Integer.MAX\_VALUE                                                                                              | STRING              | Yes      | No      |
| request.size.validation.reject.status.code          | If request is exceed maximum size and request.size.validation is enabled then status code to be send as response.                                                                                                                                                                                                                                         | 401                                                                                                             | STRING              | Yes      | No      |
| request.size.validation.reject.message              | If request is exceed maximum size and request.size.validation is enabled then status message to be send as response.                                                                                                                                                                                                                                      | Message is bigger than the valid size                                                                           | STRING              | Yes      | No      |
| request.size.validation.reject.message.content.type | If request is exceed maximum size and request.size.validation is enabled then content type to be send as response.                                                                                                                                                                                                                                        | plain/text                                                                                                      | STRING              | Yes      | No      |
| header.size.validation                              | To enable the header size validation.                                                                                                                                                                                                                                                                                                                     | false                                                                                                           | STRING              | Yes      | No      |
| header.validation.maximum.request.line              | If header header validation is enabled then the maximum request line.                                                                                                                                                                                                                                                                                     | 4096                                                                                                            | STRING              | Yes      | No      |
| header.validation.maximum.size                      | If header header validation is enabled then the maximum expected header size.                                                                                                                                                                                                                                                                             | 8192                                                                                                            | STRING              | Yes      | No      |
| header.validation.maximum.chunk.size                | If header header validation is enabled then the maximum expected chunk size.                                                                                                                                                                                                                                                                              | 8192                                                                                                            | STRING              | Yes      | No      |
| header.validation.reject.status.code                | 401                                                                                                                                                                                                                                                                                                                                                       | If header is exceed maximum size and header.size.validation is enabled then status code to be send as response. | STRING              | Yes      | No      |
| header.validation.reject.message                    | If header is exceed maximum size and header.size.validation is enabled then message to be send as response.                                                                                                                                                                                                                                               | Message header is bigger than the valid size                                                                    | STRING              | Yes      | No      |
| header.validation.reject.message.content.type       | If header is exceed maximum size and header.size.validation is enabled then content type to be send as response.                                                                                                                                                                                                                                          | plain/text                                                                                                      | STRING              | Yes      | No      |
| server.bootstrap.configuration                      | Parameters that for bootstrap configurations of the server. Expected format of these parameters is as follows: "'ciphers:xxx','sslEnabledProtocols,client.enable:xxx'"                                                                                                                                                                                    | null                                                                                                            | OBJECT              | Yes      | No      |
| server.bootstrap.nodelay                            | Http server no delay.                                                                                                                                                                                                                                                                                                                                     | true                                                                                                            | BOOL                | Yes      | No      |
| server.bootstrap.keepalive                          | Http server keep alive.                                                                                                                                                                                                                                                                                                                                   | true                                                                                                            | BOOL                | Yes      | No      |
| server.bootstrap.sendbuffersize                     | Http server send buffer size.                                                                                                                                                                                                                                                                                                                             | 1048576                                                                                                         | INT                 | Yes      | No      |
| server.bootstrap.recievebuffersize                  | Http server receive buffer size.                                                                                                                                                                                                                                                                                                                          | 1048576                                                                                                         | INT                 | Yes      | No      |
| server.bootstrap.connect.timeout                    | Http server connection timeout.                                                                                                                                                                                                                                                                                                                           | 15000                                                                                                           | INT                 | Yes      | No      |
| server.bootstrap.socket.reuse                       | To enable http socket reuse.                                                                                                                                                                                                                                                                                                                              | false                                                                                                           | BOOL                | Yes      | No      |
| server.bootstrap.socket.timeout                     | Http server socket timeout.                                                                                                                                                                                                                                                                                                                               | 15                                                                                                              | BOOL                | Yes      | No      |
| server.bootstrap.socket.backlog                     | THttp server socket backlog.                                                                                                                                                                                                                                                                                                                              | 100                                                                                                             | BOOL                | Yes      | No      |
| trace.log.enabled                                   | Http traffic monitoring.                                                                                                                                                                                                                                                                                                                                  | false                                                                                                           | BOOL                | Yes      | No      |

System Parameters

| Name                           | Description                                                                                                                                                                                                               | Default Value                                    | Possible Parameters         |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | --------------------------- |
| serverBootstrapBossGroupSize   | property to configure number of boss threads, which accepts incoming connections until the ports are unbound. Once connection accepts successfully, boss thread passes the accepted channel to one of the worker threads. | Number of available processors                   | Any integer                 |
| serverBootstrapWorkerGroupSize | property to configure number of worker threads, which performs non blocking read and write for one or more channels in non-blocking mode.                                                                                 | (Number of available processors)\*2              | Any integer                 |
| serverBootstrapClientGroupSize | property to configure number of client threads, which performs non blocking read and write for one or more channels in non-blocking mode.                                                                                 | (Number of available processors)\*2              | Any integer                 |
| defaultHost                    | The default host of the transport.                                                                                                                                                                                        | 0.0.0.0                                          | Any valid host              |
| defaultHttpPort                | The default port if the default scheme is 'http'.                                                                                                                                                                         | 8280                                             | Any valid port              |
| defaultHttpsPort               | The default port if the default scheme is 'https'.                                                                                                                                                                        | 8243                                             | Any valid port              |
| defaultScheme                  | The default protocol.                                                                                                                                                                                                     | http                                             | <p>http<br>https</p>        |
| keyStoreLocation               | The default keystore file path.                                                                                                                                                                                           | ${carbon.home}/resources/security/wso2carbon.jks | Path to wso2carbon.jks file |
| keyStorePassword               | The default keystore password.                                                                                                                                                                                            | wso2carbon                                       | String of keystore password |
| certPassword                   | The default cert password.                                                                                                                                                                                                | wso2carbon                                       | String of cert password     |

ExamplesEXAMPLE 1

```
@source(type='http-request', source.id='sampleSourceId, receiver.url='http://localhost:9055/endpoints/RecPro', connection.timeout='150000', parameters="'ciphers : TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256', 'sslEnabledProtocols:TLSv1.1,TLSv1.2'", request.size.validation.configuration="request.size.validation:true", server.bootstrap.configuration="server.bootstrap.socket.timeout:25", @map(type='json, @attributes(messageId='trp:messageId', symbol='$.events.event.symbol', price='$.events.event.price', volume='$.events.event.volume')))
define stream FooStream (messageId string, symbol string, price float, volume long);
```

The expected input is as follows:\
{"events":\
&#x20;   {"event":\
&#x20;       "symbol":WSO2,\
&#x20;       "price":55.6,\
&#x20;       "volume":100,\
&#x20;   }\
}\
If basic authentication is enabled via the `basic.auth.enabled='true` setting, each input event is also expected to contain the `Authorization:'Basic encodeBase64(username:Password)'` header.

#### http-response [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#http-response-source" id="http-response-source"></a>

The http-response source co-relates with http-request sink with the parameter 'sink.id'.\
This receives responses for the requests sent by the http-request sink which has the same sink id.\
Response messages can be in formats such as TEXT, JSON and XML.\
In order to handle the responses with different http status codes, user is allowed to defined the acceptable response source code using the parameter 'http.status.code'\


_Origin: siddhi-io-http:2.0.4_

Syntax

```
@source(type="http-response", sink.id="<STRING>", http.status.code="<STRING>", allow.streaming.responses="<BOOL>", @map(...)))
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                          | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| sink.id                   | This parameter is used to map the http-response source to a http-request sink. Then this source will accepts the response messages for the requests sent by corresponding http-request sink.                                                                                         |               | STRING              | No       | No      |
| http.status.code          | <p>Acceptable http status code for the responses.<br>This can be a complete string or a regex.<br>Only the responses with matching status codes to the defined value, will be received by the http-response source.<br>Eg: 'http.status.code = '200', http.status.code = '2\d+''</p> | 200           | STRING              | Yes      | No      |
| allow.streaming.responses | If responses can be received multiple times for a single request, this option should be enabled. If this is not enabled, for every request, response will be extracted only once.                                                                                                    | false         | BOOL                | Yes      | No      |

ExamplesEXAMPLE 1

```
@sink(type='http-request', 
downloading.enabled='true',
publisher.url='http://localhost:8005/registry/employee',
method='POST', headers='{{headers}}',sink.id='employee-info',
@map(type='json')) 
define stream BarStream (name String, id int, headers String, downloadPath string);

@source(type='http-response' , sink.id='employee-info', http.status.code='2\\d+',
@map(type='text', regex.A='((.|\n)*)', @attributes(message='A[1]'))) 
define stream responseStream2xx(message string);@source(type='http-response' , sink.id='employee-info', http.status.code='4\\d+' ,
@map(type='text', regex.A='((.|\n)*)', @attributes(message='A[1]')))  
define stream responseStream4xx(message string);
```

In above example, the defined http-request sink will send a POST requests to the endpoint defined by 'publisher.url'.\
Then for those requests, the source with the response code '2\d+' and sink.id 'employee-info' will receive the responses with 2xx status codes.\
The http-response source which has 'employee-info' as the 'sink.id' and '4\d+' as the http.response.code will receive all the responses with 4xx status codes.\
. Then the body of the response message will be extracted using text mapper and converted into siddhi events.\
.

#### inMemory [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#inmemory-source" id="inmemory-source"></a>

In-memory source that can communicate with other in-memory sinks within the same JVM, it is assumed that the publisher and subscriber of a topic uses same event schema (stream definition).

_Origin: siddhi-core:5.0.0_

Syntax

```
@source(type="inMemory", topic="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name  | Description                            | Default Value | Possible Data Types | Optional | Dynamic |
| ----- | -------------------------------------- | ------------- | ------------------- | -------- | ------- |
| topic | Subscribes to sent on the given topic. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
@source(type='inMemory', @map(type='passThrough'))
define stream BarStream (symbol string, price float, volume long)
```

In this example BarStream uses inMemory transport which passes the received event internally without using external transport.

#### kafka [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#kafka-source" id="kafka-source"></a>

A Kafka source receives events to be processed by WSO2 SP from a topic with a partition for a Kafka cluster. The events received can be in the `TEXT` `XML` `JSON` or `Binary` format.\
If the topic is not already created in the Kafka cluster, the Kafka sink creates the default partition for the given topic.

_Origin: siddhi-io-kafka:5.0.0_

Syntax

```
@source(type="kafka", bootstrap.servers="<STRING>", topic.list="<STRING>", group.id="<STRING>", threading.option="<STRING>", partition.no.list="<STRING>", seq.enabled="<BOOL>", is.binary.message="<BOOL>", topic.offset.map="<STRING>", optional.configuration="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Default Value | Possible Data Types | Optional | Dynamic |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| bootstrap.servers      | <p>This specifies the list of Kafka servers to which the Kafka source must listen. This list can be provided as a set of comma-separated values.<br>e.g., <code>localhost:9092,localhost:9093</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |               | STRING              | No       | No      |
| topic.list             | <p>This specifies the list of topics to which the source must listen. This list can be provided as a set of comma-separated values.<br>e.g., <code>topic_one,topic_two</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |               | STRING              | No       | No      |
| group.id               | This is an ID to identify the Kafka source group. The group ID ensures that sources with the same topic and partition that are in the same group do not receive the same event.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |               | STRING              | No       | No      |
| threading.option       | <p>This specifies whether the Kafka source is to be run on a single thread, or in multiple threads based on a condition. Possible values are as follows:<br><code>single.thread</code>: To run the Kafka source on a single thread.<br><code>topic-wise</code>: To use a separate thread per topic.<br><code>partition.wise</code>: To use a separate thread per partition.</p>                                                                                                                                                                                                                                                                                                  |               | STRING              | No       | No      |
| partition.no.list      | The partition number list for the given topic. This is provided as a list of comma-separated values. e.g., `0,1,2,`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 0             | STRING              | Yes      | No      |
| seq.enabled            | If this parameter is set to `true`, the sequence of the events received via the source is taken into account. Therefore, each event should contain a sequence number as an attribute value to indicate the sequence.                                                                                                                                                                                                                                                                                                                                                                                                                                                             | false         | BOOL                | Yes      | No      |
| is.binary.message      | In order to receive binary events via the Kafka source,it is required to setthis parameter to 'True'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | false         | BOOL                | Yes      | No      |
| topic.offset.map       | <p>This parameter specifies reading offsets for each topic and partition. The value for this parameter is specified in the following format:<br> <code>&#x3C;topic>=&#x3C;offset>,&#x3C;topic>=&#x3C;offset>,</code><br>  When an offset is defined for a topic, the Kafka source skips reading the message with the number specified as the offset as well as all the messages sent previous to that message. If the offset is not defined for a specific topic it reads messages from the beginning.<br>e.g., <code>stocks=100,trades=50</code> reads from the 101th message of the <code>stocks</code> topic, and from the 51st message of the <code>trades</code> topic.</p> | null          | STRING              | Yes      | No      |
| optional.configuration | <p>This parameter contains all the other possible configurations that the consumer is created with.<br>e.g., <code>ssl.keystore.type:JKS,batch.size:200</code>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | null          | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@App:name('TestExecutionPlan') 
define stream BarStream (symbol string, price float, volume long); 
@info(name = 'query1') 
@source(
type='kafka', 
topic.list='kafka_topic,kafka_topic2', 
group.id='test', 
threading.option='partition.wise', 
bootstrap.servers='localhost:9092', 
partition.no.list='0,1', 
@map(type='xml'))
Define stream FooStream (symbol string, price float, volume long);
from FooStream select symbol, price, volume insert into BarStream;
```

This kafka source configuration listens to the `kafka_topic` and `kafka_topic2` topics with `0` and `1` partitions. A thread is created for each topic and partition combination. The events are received in the XML format, mapped to a Siddhi event, and sent to a stream named `FooStream`.

EXAMPLE 2

```
@App:name('TestExecutionPlan') 
define stream BarStream (symbol string, price float, volume long); 
@info(name = 'query1') 
@source(
type='kafka', 
topic.list='kafka_topic',
group.id='test', 
threading.option='single.thread',
bootstrap.servers='localhost:9092',
@map(type='xml'))
Define stream FooStream (symbol string, price float, volume long);
from FooStream select symbol, price, volume insert into BarStream;
```

This Kafka source configuration listens to the `kafka_topic` topic for the default partition because no `partition.no.list` is defined. Only one thread is created for the topic. The events are received in the XML format, mapped to a Siddhi event, and sent to a stream named `FooStream`.

#### kafkaMultiDC [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#kafkamultidc-source" id="kafkamultidc-source"></a>

The Kafka Multi-Datacenter(DC) source receives records from the same topic in brokers deployed in two different kafka clusters. It filters out all the duplicate messages and ensuresthat the events are received in the correct order using sequential numbering. It receives events in formats such as `TEXT`, `XML` JSON `and` Binary\`.The Kafka Source creates the default partition '0' for a given topic, if the topic has not yet been created in the Kafka cluster.

_Origin: siddhi-io-kafka:5.0.0_

Syntax

```
@source(type="kafkaMultiDC", bootstrap.servers="<STRING>", topic="<STRING>", partition.no="<INT>", is.binary.message="<BOOL>", optional.configuration="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name                   | Description                                                                                                                                            | Default Value | Possible Data Types | Optional | Dynamic |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| bootstrap.servers      | This contains the kafka server list which the kafka source listens to. This is given using comma-separated values. eg: 'localhost:9092,localhost:9093' |               | STRING              | No       | No      |
| topic                  | This is the topic that the source listens to. eg: 'topic\_one'                                                                                         |               | STRING              | No       | No      |
| partition.no           | This is the partition number of the given topic.                                                                                                       | 0             | INT                 | Yes      | No      |
| is.binary.message      | In order to receive the binary events via the Kafka Multi-DC source, the value of this parameter needs to be set to 'True'.                            | false         | BOOL                | Yes      | No      |
| optional.configuration | This contains all the other possible configurations with which the consumer can be created.eg: producer.type:async,batch.size:200                      | null          | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@App:name('TestExecutionPlan') 
define stream BarStream (symbol string, price float, volume long); 
@info(name = 'query1') 
@source(type='kafkaMultiDC', topic='kafka_topic', bootstrap.servers='host1:9092,host1:9093', partition.no='1', @map(type='xml'))
Define stream FooStream (symbol string, price float, volume long);
from FooStream select symbol, price, volume insert into BarStream;
```

The following query listens to 'kafka\_topic' topic, deployed in the broker host1:9092 and host1:9093, with partition 1. A thread is created for each broker. The receiving xml events are mapped to a siddhi event and sent to the FooStream.

#### nats [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#nats-source" id="nats-source"></a>

NATS Source allows users to subscribe to a NATS broker and receive messages. It has the ability to receive all the message types supported by NATS.

_Origin: siddhi-io-nats:2.0.1_

Syntax

```
@source(type="nats", destination="<STRING>", bootstrap.servers="<STRING>", client.id="<STRING>", cluster.id="<STRING>", queue.group.name="<STRING>", durable.name="<STRING>", subscription.sequence="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name                  | Description                                                                                                                                                                                                                                       | Default Value         | Possible Data Types | Optional | Dynamic |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ------------------- | -------- | ------- |
| destination           | Subject name which NATS Source should subscribe to.                                                                                                                                                                                               |                       | STRING              | No       | No      |
| bootstrap.servers     | The NATS based url of the NATS server.                                                                                                                                                                                                            | nats://localhost:4222 | STRING              | Yes      | No      |
| client.id             | The identifier of the client subscribing/connecting to the NATS broker.                                                                                                                                                                           | None                  | STRING              | Yes      | No      |
| cluster.id            | The identifier of the NATS server/cluster.                                                                                                                                                                                                        | test-cluster          | STRING              | Yes      | No      |
| queue.group.name      | This can be used when there is a requirement to share the load of a NATS subject. Clients belongs to the same queue group share the subscription load.                                                                                            | None                  | STRING              | Yes      | No      |
| durable.name          | This can be used to subscribe to a subject from the last acknowledged message when a client or connection failure happens. The client can be uniquely identified using the tuple (client.id, durable.name).                                       | None                  | STRING              | Yes      | No      |
| subscription.sequence | This can be used to subscribe to a subject from a given number of message sequence. All the messages from the given point of sequence number will be passed to the client. If not provided then the either the persisted value or 0 will be used. | None                  | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@source(type='nats', @map(type='text'), destination='SP_NATS_INPUT_TEST', bootstrap.servers='nats://localhost:4222',client.id='nats_client',server.id='test-cluster',queue.group.name = 'group_nats',durable.name = 'nats-durable',subscription.sequence = '100')
define stream inputStream (name string, age int, country string);
```

This example shows how to subscribe to a NATS subject with all supporting configurations.With the following configuration the source identified as 'nats-client' will subscribes to a subject named as 'SP\_NATS\_INPUT\_TEST' which resides in a nats instance with a cluster id of 'test-cluster', running in localhost and listening to the port 4222 for client connection. This subscription will receive all the messages from 100th in the subject.

EXAMPLE 2

```
@source(type='nats', @map(type='text'), destination='SP_NATS_INPUT_TEST', )
define stream inputStream (name string, age int, country string);
```

This example shows how to subscribe to a NATS subject with mandatory configurations.With the following configuration the source identified with an auto generated client id will subscribes to a subject named as 'SP\_NATS\_INTPUT\_TEST' which resides in a nats instance with a cluster id of 'test-cluster', running in localhost and listening to the port 4222 for client connection. This will receive all available messages in the subject

#### tcp [_(Source)_](https://siddhi.io/en/v5.0/docs/query-guide/#source) <a href="#tcp-source" id="tcp-source"></a>

A Siddhi application can be configured to receive events via the TCP transport by adding the @Source(type = 'tcp') annotation at the top of an event stream definition.\
\
When this is defined the associated stream will receive events from the TCP transport on the host and port defined in the system.

_Origin: siddhi-io-tcp:3.0.1_

Syntax

```
@source(type="tcp", context="<STRING>", @map(...)))
```

QUERY PARAMETERS

| Name    | Description                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| ------- | ------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| context | The URL 'context' that should be used to receive the events. | /             | STRING              | Yes      | No      |

System Parameters

| Name             | Description                                                                                                                                                                                                                                                                                                                   | Default Value | Possible Parameters                 |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ----------------------------------- |
| host             | Tcp server host.                                                                                                                                                                                                                                                                                                              | 0.0.0.0       | Any valid host or IP                |
| port             | Tcp server port.                                                                                                                                                                                                                                                                                                              | 9892          | Any integer representing valid port |
| receiver.threads | Number of threads to receive connections.                                                                                                                                                                                                                                                                                     | 10            | Any positive integer                |
| worker.threads   | Number of threads to serve events.                                                                                                                                                                                                                                                                                            | 10            | Any positive integer                |
| tcp.no.delay     | <p>This is to specify whether to disable Nagle algorithm during message passing.<br>If tcp.no.delay = 'true', the execution of Nagle algorithm will be disabled in the underlying TCP logic. Hence there will be no delay between two successive writes to the TCP connection.<br>Else there can be a constant ack delay.</p> | true          | <p>true<br>false</p>                |
| keep.alive       | This property defines whether the server should be kept alive when there are no connections available.                                                                                                                                                                                                                        | true          | <p>true<br>false</p>                |

ExamplesEXAMPLE 1

```
@Source(type = 'tcp', context='abc', @map(type='binary'))
define stream Foo (attribute1 string, attribute2 int );
```

Under this configuration, events are received via the TCP transport on default host,port, `abc` context, and they are passed to `Foo` stream for processing.

### Sourcemapper <a href="#sourcemapper" id="sourcemapper"></a>

#### binary [_(Source Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#source-mapper) <a href="#binary-source-mapper" id="binary-source-mapper"></a>

This extension is a binary input mapper that converts events received in `binary` format to Siddhi events before they are processed.

_Origin: siddhi-map-binary:2.0.0_

Syntax

```
@source(..., @map(type="binary")
```

ExamplesEXAMPLE 1

```
@source(type='inMemory', topic='WSO2', @map(type='binary'))define stream FooStream (symbol string, price float, volume long); 
```

This query performs a mapping to convert an event of the `binary` format to a Siddhi event.

#### csv [_(Source Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#source-mapper) <a href="#csv-source-mapper" id="csv-source-mapper"></a>

This extension is used to convert CSV message to Siddhi event input mapper. You can either receive pre-defined CSV message where event conversion takes place without extra configurations,or receive custom CSV message where a custom place order to map from custom CSV message.

_Origin: siddhi-map-csv:2.0.0_

Syntax

```
@source(..., @map(type="csv", delimiter="<STRING>", header.present="<BOOL>", fail.on.unknown.attribute="<BOOL>", event.grouping.enabled="<BOOL>")
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                                                                                         | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| delimiter                 | When converting a CSV format message to Siddhi event, this parameter indicatesinput CSV message's data should be split by this parameter                                                                                                                                                                                                            | ,             | STRING              | Yes      | No      |
| header.present            | When converting a CSV format message to Siddhi event, this parameter indicates whether CSV message has header or not. This can either have value true or false.If it's set to `false` then it indicates that CSV message has't header.                                                                                                              | false         | BOOL                | Yes      | No      |
| fail.on.unknown.attribute | This parameter specifies how unknown attributes should be handled. If it's set to `true` and one or more attributes don't havevalues, then SP will drop that message. If this parameter is set to `false`, the Stream Processor adds the required attribute's values to such events with a null value and the event is converted to a Siddhi event. | true          | BOOL                | Yes      | No      |
| event.grouping.enabled    | This parameter specifies whether event grouping is enabled or not. To receive a group of events together and generate multiple events, this parameter must be set to `true`.                                                                                                                                                                        | false         | BOOL                | Yes      | No      |

ExamplesEXAMPLE 1

```
@source(type='inMemory', topic='stock', @map(type='csv'))
 define stream FooStream (symbol string, price float, volume int); 
```

Above configuration will do a default CSV input mapping. Expected input will look like below:\
&#x20;WSO2 ,55.6 , 100OR\
&#x20;"WSO2,No10,Palam Groove Rd,Col-03" ,55.6 , 100If header.present is true and delimiter is "-", then the input is as follows:\
symbol-price-volumeWSO2-55.6-100

EXAMPLE 2

```
@source(type='inMemory', topic='stock', @map(type='csv',header='true', @attributes(symbol = "2", price = "0", volume = "1")))
define stream FooStream (symbol string, price float, volume long); 
```

Above configuration will perform a custom CSV mapping. Here, user can add place order of each attribute in the @attribute. The place order indicates where the attribute name's value has appeared in the input.Expected input will look like below:\
55.6,100,WSO2\
OR55.6,100,"WSO2,No10,Palm Groove Rd,Col-03"\
If header is true and delimiter is "-", then the output is as follows:\
price-volume-symbol\
55.6-100-WSO2\
If group events is enabled then input should be as follows:\
price-volume-symbol\
55.6-100-WSO2System.lineSeparator()\
55.6-100-IBMSystem.lineSeparator()\
55.6-100-IFSSystem.lineSeparator()\


#### json [_(Source Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#source-mapper) <a href="#json-source-mapper" id="json-source-mapper"></a>

This extension is a JSON-to-Event input mapper. Transports that accept JSON messages can utilize this extension to convert an incoming JSON message into a Siddhi event. Users can either send a pre-defined JSON format, where event conversion happens without any configurations, or use the JSON path to map from a custom JSON message.\
In default mapping, the JSON string of the event can be enclosed by the element "event", though optional.

_Origin: siddhi-map-json:5.0.1_

Syntax

```
@source(..., @map(type="json", enclosing.element="<STRING>", fail.on.missing.attribute="<BOOL>")
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                                                                                                                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| enclosing.element         | <p>This is used to specify the enclosing element when sending multiple events in the same JSON message.<br>Mapper treats the child elements of a given enclosing element as events and executes the JSON path expressions on these child elements.<br>If the enclosing.element is not provided then the multiple-event scenario is disregarded and the JSON path is evaluated based on the root element.</p> | $             | STRING              | Yes      | No      |
| fail.on.missing.attribute | <p><br>This parameter allows users to handle unknown attributes.The value of this can either be true or false. By default it is true.<br> If a JSON execution fails or returns null, mapper drops that message.<br>However, setting this property to false prompts mapper to send an event with a null value to Siddhi, where users can handle it as required, ie., assign a default value.)</p>             | true          | BOOL                | Yes      | No      |

ExamplesEXAMPLE 1

```
@source(type='inMemory', topic='stock', @map(type='json'))
define stream FooStream (symbol string, price float, volume long);
```

This configuration performs a default JSON input mapping.\
&#x20;For a single event, the input is required to be in one of the following formats:\
{\
&#x20;   "event":{\
&#x20;       "symbol":"WSO2",\
&#x20;       "price":55.6,\
&#x20;       "volume":100\
&#x20;   }\
}\
\
or\
\
{\
&#x20;   "symbol":"WSO2",\
&#x20;   "price":55.6,\
&#x20;   "volume":100\
}\


EXAMPLE 2

```
@source(type='inMemory', topic='stock', @map(type='json'))
define stream FooStream (symbol string, price float, volume long);
```

This configuration performs a default JSON input mapping.\
For multiple events, the input is required to be in one of the following formats:\
\[\
{"event":{"symbol":"WSO2","price":55.6,"volume":100\}},\
{"event":{"symbol":"WSO2","price":56.6,"volume":99\}},\
{"event":{"symbol":"WSO2","price":57.6,"volume":80\}}\
]\
\
or\
\
\[\
{"symbol":"WSO2","price":55.6,"volume":100},\
{"symbol":"WSO2","price":56.6,"volume":99},\
{"symbol":"WSO2","price":57.6,"volume":80}\
]

EXAMPLE 3

```
@source(type='inMemory', topic='stock', @map(type='json', enclosing.element="$.portfolio", @attributes(symbol = "company.symbol", price = "price", volume = "volume")))
```

This configuration performs a custom JSON mapping.\
For a single event, the expected input is similar to the one shown below:\
.{\
&#x20;"portfolio":{\
&#x20;    "stock":{ "volume":100,\
&#x20;       "company":{\
&#x20;          "symbol":"WSO2"\
&#x20;      },\
&#x20;       "price":55.6\
&#x20;   }\
}\


EXAMPLE 4

```
@source(type='inMemory', topic='stock', @map(type='json', enclosing.element="$.portfolio", @attributes(symbol = "stock.company.symbol", price = "stock.price", volume = "stock.volume")))
define stream FooStream (symbol string, price float, volume long);
```

The configuration performs a custom JSON mapping.\
For multiple events, expected input looks as follows.\
.{"portfolio":\
&#x20;  \[ {"stock":{"volume":100,"company":{"symbol":"wso2"},"price":56.6\}}, {"stock":{"volume":200,"company":{"symbol":"wso2"},"price":57.6\}} ]\
}\


#### keyvalue [_(Source Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#source-mapper) <a href="#keyvalue-source-mapper" id="keyvalue-source-mapper"></a>

`Key-Value Map to Event` input mapper extension allows transports that accept events as key value maps to convert those events to Siddhi events. You can either receive pre-defined keys where conversion takes place without extra configurations, or use custom keys to map from the message.

_Origin: siddhi-map-keyvalue:2.0.0_

Syntax

```
@source(..., @map(type="keyvalue", fail.on.missing.attribute="<BOOL>")
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                          | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| fail.on.missing.attribute | If this parameter is set to `true`, if an event arrives without a matching key for a specific attribute in the connected stream, it is dropped and not processed by the Stream Processor. If this parameter is set to `false` the Stream Processor adds the required key to such events with a null value, and the event is converted to a Siddhi event so that you could handle them as required before they are further processed. | true          | BOOL                | Yes      | No      |

ExamplesEXAMPLE 1

```
@source(type='inMemory', topic='stock', @map(type='keyvalue'))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a default key value input mapping. The expected input is a map similar to the following:\
symbol: 'WSO2'\
price: 55.6f\
volume: 100

EXAMPLE 2

```
@source(type='inMemory', topic='stock', @map(type='keyvalue', fail.on.missing.attribute='true', @attributes(symbol = 's', price = 'p', volume = 'v')))define stream FooStream (symbol string, price float, volume long); 
```

This query performs a custom key value input mapping. The matching keys for the `symbol`, `price` and `volume` attributes are be `s`, `p, and` v\` respectively. The expected input is a map similar to the following: s: 'WSO2' p: 55.6 v: 100

#### passThrough [_(Source Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#source-mapper) <a href="#passthrough-source-mapper" id="passthrough-source-mapper"></a>

Pass-through mapper passed events (Event\[]) through without any mapping or modifications.

_Origin: siddhi-core:5.0.0_

Syntax

```
@source(..., @map(type="passThrough")
```

ExamplesEXAMPLE 1

```
@source(type='tcp', @map(type='passThrough'))
define stream BarStream (symbol string, price float, volume long);
```

In this example BarStream uses passThrough inputmapper which passes the received Siddhi event directly without any transformation into source.

#### text [_(Source Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#source-mapper) <a href="#text-source-mapper" id="text-source-mapper"></a>

This extension is a text to Siddhi event input mapper. Transports that accept text messages can utilize this extension to convert the incoming text message to Siddhi event. Users can either use a pre-defined text format where event conversion happens without any additional configurations, or specify a regex to map a text message using custom configurations.

_Origin: siddhi-map-text:2.0.0_

Syntax

```
@source(..., @map(type="text", regex.groupid="<STRING>", fail.on.missing.attribute="<BOOL>", event.grouping.enabled="<BOOL>", delimiter="<STRING>", new.line.character="<STRING>")
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                         | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| regex.groupid             | This parameter specifies a regular expression group. The `groupid` can be any capital letter (e.g., regex.A,regex.B .. etc). You can specify any number of regular expression groups. In the attribute annotation, you need to map all attributes to the regular expression group with the matching group index. If you need to to enable custom mapping, it is required to specifythe matching group for each and every attribute. |               | STRING              | No       | No      |
| fail.on.missing.attribute | This parameter specifies how unknown attributes should be handled. If it is set to `true` a message is dropped if its execution fails, or if one or more attributes do not have values. If this parameter is set to `false`, null values are assigned to attributes with missing values, and messages with such attributes are not dropped.                                                                                         | true          | BOOL                | Yes      | No      |
| event.grouping.enabled    | This parameter specifies whether event grouping is enabled or not. To receive a group of events together and generate multiple events, this parameter must be set to `true`.                                                                                                                                                                                                                                                        | false         | BOOL                | Yes      | No      |
| delimiter                 | This parameter specifies how events must be separated when multiple events are received. This must be whole line and not a single character.                                                                                                                                                                                                                                                                                        | \~\~\~\~      | STRING              | Yes      | No      |
| new.line.character        | This attribute indicates the new line character of the event that is expected to be received. This is used mostly when communication between 2 types of operating systems is expected. For example, Linux uses  as the end of line character whereas windows uses `\r`.                                                                                                                                                             |               | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
@source(type='inMemory', topic='stock', @map(type='text'))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a default text input mapping. The expected input is as follows:\
symbol:"WSO2",\
price:55.6,\
volume:100\
OR\
symbol:'WSO2',\
price:55.6,\
volume:100\
\
If group events is enabled then input should be as follows:\
symbol:"WSO2",\
price:55.6,\
volume:100\
\~\~\~\~\
symbol:"WSO2",\
price:55.6,\
volume:100

EXAMPLE 2

```
@source(type='inMemory', topic='stock', @map(type='text', fail.on.unknown.attribute = 'true', regex.A='(\w+)\s([-0-9]+)',regex.B='volume\s([-0-9]+)', @attributes(symbol = 'A[1]',price = 'A[2]',volume = 'B')))
define stream FooStream (symbol string, price float, volume long);
```

This query performs a custom text mapping. The expected input is as follows:\
wos2 550 volume 100\
\
If group events is enabled then input should be as follows:\
wos2 550 volume 100\
\~\~\~\~\
wos2 550 volume 100\
\~\~\~\~\
wos2 550 volume 100\


#### xml [_(Source Mapper)_](https://siddhi.io/en/v5.0/docs/query-guide/#source-mapper) <a href="#xml-source-mapper" id="xml-source-mapper"></a>

This mapper converts XML input to Siddhi event. Transports which accepts XML messages can utilize this extension to convert the incoming XML message to Siddhi event. Users can either send a pre-defined XML format where event conversion will happen without any configs or can use xpath to map from a custom XML message.

_Origin: siddhi-map-xml:5.0.0_

Syntax

```
@source(..., @map(type="xml", namespaces="<STRING>", enclosing.element="<STRING>", fail.on.missing.attribute="<BOOL>")
```

QUERY PARAMETERS

| Name                      | Description                                                                                                                                                                                                                                                                                                                                                                           | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| namespaces                | Used to provide namespaces used in the incoming XML message beforehand to configure xpath expressions. User can provide a comma separated list. If these are not provided xpath evaluations will fail                                                                                                                                                                                 | None          | STRING              | Yes      | No      |
| enclosing.element         | Used to specify the enclosing element in case of sending multiple events in same XML message. WSO2 DAS will treat the child element of given enclosing element as events and execute xpath expressions on child elements. If enclosing.element is not provided multiple event scenario is disregarded and xpaths will be evaluated with respect to root element.                      | Root element  | STRING              | Yes      | No      |
| fail.on.missing.attribute | This can either have value true or false. By default it will be true. This attribute allows user to handle unknown attributes. By default if an xpath execution fails or returns null DAS will drop that message. However setting this property to false will prompt DAS to send and event with null value to Siddhi where user can handle it accordingly(ie. Assign a default value) | True          | BOOL                | Yes      | No      |

ExamplesEXAMPLE 1

```
@source(type='inMemory', topic='stock', @map(type='xml'))
define stream FooStream (symbol string, price float, volume long);
```

Above configuration will do a default XML input mapping. Expected input will look like below.\<events>\
&#x20;   \<event>\
&#x20;       \<symbol>WSO2\</symbol>\
&#x20;       \<price>55.6\</price>\
&#x20;       \<volume>100\</volume>\
&#x20;   \</event>\
\</events>\


EXAMPLE 2

```
@source(type='inMemory', topic='stock', @map(type='xml', namespaces = "dt=urn:schemas-microsoft-com:datatypes", enclosing.element="//portfolio", @attributes(symbol = "company/symbol", price = "price", volume = "volume")))
define stream FooStream (symbol string, price float, volume long);
```

Above configuration will perform a custom XML mapping. In the custom mapping user can add xpath expressions representing each event attribute using @attribute annotation. Expected input will look like below.\
\<portfolio xmlns:dt="urn:schemas-microsoft-com:datatypes">\
&#x20;   \<stock exchange="nasdaq">\
&#x20;       \<volume>100\</volume>\
&#x20;       \<company>\
&#x20;          \<symbol>WSO2\</symbol>\
&#x20;       \</company>\
&#x20;       \<price dt:type="number">55.6\</price>\
&#x20;   \</stock>\
\</portfolio>

### Store <a href="#store" id="store"></a>

#### rdbms [_(Store)_](https://siddhi.io/en/v5.0/docs/query-guide/#store) <a href="#rdbms-store" id="rdbms-store"></a>

This extension assigns data sources and connection instructions to event tables. It also implements read-write operations on connected datasources.

_Origin: siddhi-store-rdbms:6.0.0_

Syntax

```
@Store(type="rdbms", jdbc.url="<STRING>", username="<STRING>", password="<STRING>", jdbc.driver.name="<STRING>", pool.properties="<STRING>", jndi.resource="<STRING>", datasource="<STRING>", table.name="<STRING>", field.length="<STRING>")
@PrimaryKey("PRIMARY_KEY")
@Index("INDEX")
```

QUERY PARAMETERS

| Name             | Description                                                                                                                                                                                                                                                                                                                                       | Default Value                                   | Possible Data Types | Optional | Dynamic |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- | ------------------- | -------- | ------- |
| jdbc.url         | The JDBC URL via which the RDBMS data store is accessed.                                                                                                                                                                                                                                                                                          |                                                 | STRING              | No       | No      |
| username         | The username to be used to access the RDBMS data store.                                                                                                                                                                                                                                                                                           |                                                 | STRING              | No       | No      |
| password         | The password to be used to access the RDBMS data store.                                                                                                                                                                                                                                                                                           |                                                 | STRING              | No       | No      |
| jdbc.driver.name | The driver class name for connecting the RDBMS data store.                                                                                                                                                                                                                                                                                        |                                                 | STRING              | No       | No      |
| pool.properties  | Any pool parameters for the database connection must be specified as key-value pairs.                                                                                                                                                                                                                                                             | null                                            | STRING              | Yes      | No      |
| jndi.resource    | The name of the JNDI resource through which the connection is attempted. If this is found, the pool properties described above are not taken into account and the connection is attempted via JNDI lookup instead.                                                                                                                                | null                                            | STRING              | Yes      | No      |
| datasource       | The name of the Carbon datasource that should be used for creating the connection with the database. If this is found, neither the pool properties nor the JNDI resource name described above are taken into account and the connection is attempted via Carbon datasources instead.                                                              | null                                            | STRING              | Yes      | No      |
| table.name       | The name with which the event table should be persisted in the store. If no name is specified via this parameter, the event table is persisted with the same name as the Siddhi table.                                                                                                                                                            | The table name defined in the Siddhi App query. | STRING              | Yes      | No      |
| field.length     | The number of characters that the values for fields of the 'STRING' type in the table definition must contain. Each required field must be provided as a comma-separated list of key-value pairs in the '\<field.name>:\<length>' format. If this is not specified, the default number of characters specific to the database type is considered. | null                                            | STRING              | Yes      | No      |

System Parameters

| Name                                       | Description                                                                                                                                                                                                                      | Default Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Possible Parameters |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| \{{RDBMS-Name\}}.maxVersion                | The latest version supported for \{{RDBMS-Name\}}.                                                                                                                                                                               | 0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | N/A                 |
| \{{RDBMS-Name\}}.minVersion                | The earliest version supported for \{{RDBMS-Name\}}.                                                                                                                                                                             | 0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | N/A                 |
| \{{RDBMS-Name\}}.tableCheckQuery           | The template query for the 'check table' operation in \{{RDBMS-Name\}}.                                                                                                                                                          | <p>H2: CREATE TABLE {{TABLE_NAME}} ({{COLUMNS, PRIMARY_KEYS}})<br>MySQL: CREATE TABLE {{TABLE_NAME}} ({{COLUMNS, PRIMARY_KEYS}})<br>Oracle: CREATE TABLE {{TABLE_NAME}} ({{COLUMNS, PRIMARY_KEYS}})<br>Microsoft SQL Server: CREATE TABLE {{TABLE_NAME}} ({{COLUMNS, PRIMARY_KEYS}})<br>PostgreSQL: CREATE TABLE {{TABLE_NAME}} ({{COLUMNS, PRIMARY_KEYS}})<br>DB2.<em>: CREATE TABLE {{TABLE_NAME}} ({{COLUMNS, PRIMARY_KEYS}})</em></p>                                                                                                                                    | N/A                 |
| \{{RDBMS-Name\}}.tableCreateQuery          | The template query for the 'create table' operation in \{{RDBMS-Name\}}.                                                                                                                                                         | <p>H2: SELECT 1 FROM {{TABLE_NAME}} LIMIT 1<br>MySQL: SELECT 1 FROM {{TABLE_NAME}} LIMIT 1<br>Oracle: SELECT 1 FROM {{TABLE_NAME}} WHERE rownum=1<br>Microsoft SQL Server: SELECT TOP 1 1 from {{TABLE_NAME}}<br>PostgreSQL: SELECT 1 FROM {{TABLE_NAME}} LIMIT 1<br>DB2.: SELECT 1 FROM {{TABLE_NAME}} FETCH FIRST 1 ROWS ONLY</p>                                                                                                                                                                                                                                          | N/A                 |
| \{{RDBMS-Name\}}.indexCreateQuery          | The template query for the 'create index' operation in \{{RDBMS-Name\}}.                                                                                                                                                         | <p>H2: CREATE INDEX {{TABLE_NAME}}_INDEX ON {{TABLE_NAME}} ({{INDEX_COLUMNS}})<br>MySQL: CREATE INDEX {{TABLE_NAME}}_INDEX ON {{TABLE_NAME}} ({{INDEX_COLUMNS}})<br>Oracle: CREATE INDEX {{TABLE_NAME}}_INDEX ON {{TABLE_NAME}} ({{INDEX_COLUMNS}})<br>Microsoft SQL Server: CREATE INDEX {{TABLE_NAME}}_INDEX ON {{TABLE_NAME}} ({{INDEX_COLUMNS}}) {{TABLE_NAME}} ({{INDEX_COLUMNS}})<br>PostgreSQL: CREATE INDEX {{TABLE_NAME}}_INDEX ON {{TABLE_NAME}} ({{INDEX_COLUMNS}})<br>DB2.<em>: CREATE INDEX {{TABLE_NAME}}_INDEX ON {{TABLE_NAME}} ({{INDEX_COLUMNS}})</em></p> | N/A                 |
| \{{RDBMS-Name\}}.recordInsertQuery         | The template query for the 'insert record' operation in \{{RDBMS-Name\}}.                                                                                                                                                        | <p>H2: INSERT INTO {{TABLE_NAME}} ({{COLUMNS}}) VALUES ({{Q}})<br>MySQL: INSERT INTO {{TABLE_NAME}} ({{COLUMNS}}) VALUES ({{Q}})<br>Oracle: INSERT INTO {{TABLE_NAME}} ({{COLUMNS}}) VALUES ({{Q}})<br>Microsoft SQL Server: INSERT INTO {{TABLE_NAME}} ({{COLUMNS}}) VALUES ({{Q}})<br>PostgreSQL: INSERT INTO {{TABLE_NAME}} ({{COLUMNS}}) VALUES ({{Q}})<br>DB2.: INSERT INTO {{TABLE_NAME}} ({{COLUMNS}}) VALUES ({{Q}})</p>                                                                                                                                             | N/A                 |
| \{{RDBMS-Name\}}.recordUpdateQuery         | The template query for the 'update record' operation in \{{RDBMS-Name\}}.                                                                                                                                                        | <p>H2: UPDATE {{TABLE_NAME}} SET {{COLUMNS_AND_VALUES}} {{CONDITION}}<br>MySQL: UPDATE {{TABLE_NAME}} SET {{COLUMNS_AND_VALUES}} {{CONDITION}}<br>Oracle: UPDATE {{TABLE_NAME}} SET {{COLUMNS_AND_VALUES}} {{CONDITION}}<br>Microsoft SQL Server: UPDATE {{TABLE_NAME}} SET {{COLUMNS_AND_VALUES}} {{CONDITION}}<br>PostgreSQL: UPDATE {{TABLE_NAME}} SET {{COLUMNS_AND_VALUES}} {{CONDITION}}<br>DB2.<em>: UPDATE {{TABLE_NAME}} SET {{COLUMNS_AND_VALUES}} {{CONDITION}}</em></p>                                                                                          | N/A                 |
| \{{RDBMS-Name\}}.recordSelectQuery         | The template query for the 'select record' operation in \{{RDBMS-Name\}}.                                                                                                                                                        | <p>H2: SELECT * FROM {{TABLE_NAME}} {{CONDITION}}<br>MySQL: SELECT * FROM {{TABLE_NAME}} {{CONDITION}}<br>Oracle: SELECT * FROM {{TABLE_NAME}} {{CONDITION}}<br>Microsoft SQL Server: SELECT * FROM {{TABLE_NAME}} {{CONDITION}}<br>PostgreSQL: SELECT * FROM {{TABLE_NAME}} {{CONDITION}}<br>DB2.: SELECT * FROM {{TABLE_NAME}} {{CONDITION}}</p>                                                                                                                                                                                                                           | N/A                 |
| \{{RDBMS-Name\}}.recordExistsQuery         | The template query for the 'check record existence' operation in \{{RDBMS-Name\}}.                                                                                                                                               | <p>H2: SELECT TOP 1 1 FROM {{TABLE_NAME}} {{CONDITION}}<br>MySQL: SELECT 1 FROM {{TABLE_NAME}} {{CONDITION}}<br>Oracle: SELECT COUNT(1) INTO existence FROM {{TABLE_NAME}} {{CONDITION}}<br>Microsoft SQL Server: SELECT TOP 1 FROM {{TABLE_NAME}} {{CONDITION}}<br>PostgreSQL: SELECT 1 FROM {{TABLE_NAME}} {{CONDITION}} LIMIT 1<br>DB2.<em>: SELECT 1 FROM {{TABLE_NAME}} {{CONDITION}} FETCH FIRST 1 ROWS ONLY</em></p>                                                                                                                                                  | N/A                 |
| \{{RDBMS-Name\}}.recordDeleteQuery         | The query for the 'delete record' operation in \{{RDBMS-Name\}}.                                                                                                                                                                 | <p>H2: DELETE FROM {{TABLE_NAME}} {{CONDITION}}<br>MySQL: DELETE FROM {{TABLE_NAME}} {{CONDITION}}<br>Oracle: DELETE FROM {{TABLE_NAME}} {{CONDITION}}<br>Microsoft SQL Server: DELETE FROM {{TABLE_NAME}} {{CONDITION}}<br>PostgreSQL: DELETE FROM {{TABLE_NAME}} {{CONDITION}}<br>DB2.: DELETE FROM {{TABLE_NAME}} {{CONDITION}}</p>                                                                                                                                                                                                                                       | N/A                 |
| \{{RDBMS-Name\}}.stringSize                | This defines the length for the string fields in \{{RDBMS-Name\}}.                                                                                                                                                               | <p>H2: 254<br>MySQL: 254<br>Oracle: 254<br>Microsoft SQL Server: 254<br>PostgreSQL: 254<br>DB2.<em>: 254</em></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                            | N/A                 |
| \{{RDBMS-Name\}}.fieldSizeLimit            | This defines the field size limit for select/switch to big string type from the default string type if the 'bigStringType' is available in field type list.                                                                      | <p>H2: N/A<br>MySQL: N/A<br>Oracle: 2000<br>Microsoft SQL Server: N/A<br>PostgreSQL: N/A<br>DB2.: N/A</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 0 =< n =< INT\_MAX  |
| \{{RDBMS-Name\}}.batchSize                 | This defines the batch size when operations are performed for batches of events.                                                                                                                                                 | <p>H2: 1000<br>MySQL: 1000<br>Oracle: 1000<br>Microsoft SQL Server: 1000<br>PostgreSQL: 1000<br>DB2.<em>: 1000</em></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                      | N/A                 |
| \{{RDBMS-Name\}}.batchEnable               | This specifies whether 'Update' and 'Insert' operations can be performed for batches of events or not.                                                                                                                           | <p>H2: true<br>MySQL: true<br>Oracle (versions 12.0 and less): false<br>Oracle (versions 12.1 and above): true<br>Microsoft SQL Server: true<br>PostgreSQL: true<br>DB2.: true</p>                                                                                                                                                                                                                                                                                                                                                                                           | N/A                 |
| \{{RDBMS-Name\}}.transactionSupported      | This is used to specify whether the JDBC connection that is used supports JDBC transactions or not.                                                                                                                              | <p>H2: true<br>MySQL: true<br>Oracle: true<br>Microsoft SQL Server: true<br>PostgreSQL: true<br>DB2.<em>: true</em></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                      | N/A                 |
| \{{RDBMS-Name\}}.typeMapping.binaryType    | This is used to specify the binary data type. An attribute defines as 'object' type in Siddhi stream will be stored into RDBMS with this type.                                                                                   | <p>H2: BLOB<br>MySQL: BLOB<br>Oracle: BLOB<br>Microsoft SQL Server: VARBINARY(max)<br>PostgreSQL: BYTEA<br>DB2.: BLOB(64000)</p>                                                                                                                                                                                                                                                                                                                                                                                                                                             | N/A                 |
| \{{RDBMS-Name\}}.typeMapping.booleanType   | This is used to specify the boolean data type. An attribute defines as 'bool' type in Siddhi stream will be stored into RDBMS with this type.                                                                                    | <p>H2: TINYINT(1)<br>MySQL: TINYINT(1)<br>Oracle: NUMBER(1)<br>Microsoft SQL Server: BIT<br>PostgreSQL: BOOLEAN<br>DB2.<em>: SMALLINT</em></p>                                                                                                                                                                                                                                                                                                                                                                                                                               | N/A                 |
| \{{RDBMS-Name\}}.typeMapping.doubleType    | This is used to specify the double data type. An attribute defines as 'double' type in Siddhi stream will be stored into RDBMS with this type.                                                                                   | <p>H2: DOUBLE<br>MySQL: DOUBLE<br>Oracle: NUMBER(19,4)<br>Microsoft SQL Server: FLOAT(32)<br>PostgreSQL: DOUBLE PRECISION<br>DB2.: DOUBLE</p>                                                                                                                                                                                                                                                                                                                                                                                                                                | N/A                 |
| \{{RDBMS-Name\}}.typeMapping.floatType     | This is used to specify the float data type. An attribute defines as 'float' type in Siddhi stream will be stored into RDBMS with this type.                                                                                     | <p>H2: FLOAT<br>MySQL: FLOAT<br>Oracle: NUMBER(19,4)<br>Microsoft SQL Server: REAL<br>PostgreSQL: REAL<br>DB2.<em>: REAL</em></p>                                                                                                                                                                                                                                                                                                                                                                                                                                            | N/A                 |
| \{{RDBMS-Name\}}.typeMapping.integerType   | This is used to specify the integer data type. An attribute defines as 'int' type in Siddhi stream will be stored into RDBMS with this type.                                                                                     | <p>H2: INTEGER<br>MySQL: INTEGER<br>Oracle: NUMBER(10)<br>Microsoft SQL Server: INTEGER<br>PostgreSQL: INTEGER<br>DB2.: INTEGER</p>                                                                                                                                                                                                                                                                                                                                                                                                                                          | N/A                 |
| \{{RDBMS-Name\}}.typeMapping.longType      | This is used to specify the long data type. An attribute defines as 'long' type in Siddhi stream will be stored into RDBMS with this type.                                                                                       | <p>H2: BIGINT<br>MySQL: BIGINT<br>Oracle: NUMBER(19)<br>Microsoft SQL Server: BIGINT<br>PostgreSQL: BIGINT<br>DB2.<em>: BIGINT</em></p>                                                                                                                                                                                                                                                                                                                                                                                                                                      | N/A                 |
| \{{RDBMS-Name\}}.typeMapping.stringType    | This is used to specify the string data type. An attribute defines as 'string' type in Siddhi stream will be stored into RDBMS with this type.                                                                                   | <p>H2: VARCHAR(stringSize)<br>MySQL: VARCHAR(stringSize)<br>Oracle: VARCHAR(stringSize)<br>Microsoft SQL Server: VARCHAR(stringSize)<br>PostgreSQL: VARCHAR(stringSize)<br>DB2.: VARCHAR(stringSize)</p>                                                                                                                                                                                                                                                                                                                                                                     | N/A                 |
| \{{RDBMS-Name\}}.typeMapping.bigStringType | This is used to specify the big string data type. An attribute defines as 'string' type in Siddhi stream and field.length define in the annotation is greater than the fieldSizeLimit, will be stored into RDBMS with this type. | <p>H2: N/A<br>MySQL: N/AOracle: CLOBMicrosoft SQL Server: N/A<br>PostgreSQL: N/A<br>DB2.*: N/A</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | N/A                 |

ExamplesEXAMPLE 1

```
@Store(type="rdbms", jdbc.url="jdbc:mysql://localhost:3306/stocks", username="root", password="root", jdbc.driver.name="com.mysql.jdbc.Driver",field.length="symbol:100")
@PrimaryKey("symbol")
@Index("volume")
define table StockTable (symbol string, price float, volume long);
```

The above example creates an event table named 'StockTable' in the database if it does not already exist (with three attributes named 'symbol', 'price', and 'volume' of the types 'string', 'float', and 'long' respectively). The connection is made as specified by the parameters configured for the '@Store' annotation. The 'symbol' attribute is considered a unique field, and a DB index is created for it.

EXAMPLE 2

```
@Store(type="rdbms", jdbc.url="jdbc:mysql://localhost:3306/das", username="root", password="root" , jdbc.driver.name="org.h2.Driver",field.length="symbol:100")
@PrimaryKey("symbol")
@Index("symbol")
define table StockTable (symbol string, price float, volume long);
define stream InputStream (symbol string, volume long);
from InputStream as a join StockTable as b on str:contains(b.symbol, a.symbol)
select a.symbol as symbol, b.volume as volume
insert into FooStream;
```

The above example creates an event table named 'StockTable' in the database if it does not already exist (with three attributes named 'symbol', 'price', and 'volume' of the types 'string', 'float' and 'long' respectively). Then the table is joined with a stream named 'InputStream' based on a condition. The following operations are included in the condition:\
\[ AND, OR, Comparisons( < <= > >= == !=), IS NULL, NOT, str:contains(Table\<Column>, Stream\<Attribute> or Search.String)]

### Str <a href="#str" id="str"></a>

#### groupConcat [_(Aggregate Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#aggregate-function) <a href="#groupconcat-aggregate-function" id="groupconcat-aggregate-function"></a>

This function aggregates the received events by concatenating the keys in those events using a separator, e.g.,a comma (,) or a hyphen (-), and returns the concatenated key string.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:groupConcat(<STRING> key, <STRING> separator, <STRING> distinct, <STRING> order)
```

QUERY PARAMETERS

| Name      | Description                                                                                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| --------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| key       | The string that needs to be aggregated.                                                                                      |               | STRING              | No       | No      |
| separator | The separator that separates each string key after concatenating the keys.                                                   | ,             | STRING              | Yes      | No      |
| distinct  | This is used to only have distinct values in the concatenated string that is returned.                                       | false         | STRING              | Yes      | No      |
| order     | This parameter accepts 'ASC' or 'DESC' strings to sort the string keys in either ascending or descending order respectively. | No order      | STRING              | Yes      | No      |

ExamplesEXAMPLE 1

```
from InputStream#window.time(5 min)
select str:groupConcat("key") as groupedKeys
input OutputStream;
```

When we input events having values for the `key` as `'A'`, `'B'`, `'S'`, `'C'`, `'A'`, it returns `"A,B,S,C,A"` to the 'OutputStream'.

EXAMPLE 2

```
from InputStream#window.time(5 min)
select groupConcat("key","-",true,"ASC") as groupedKeys
input OutputStream;
```

When we input events having values for the `key` as `'A'`, `'B'`, `'S'`, `'C'`, `'A'`, specify the seperator as hyphen and choose the order to be ascending, the function returns `"A-B-C-S"` to the 'OutputStream'.

#### charAt [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#charat-function" id="charat-function"></a>

This function returns the 'char' value that is present at the given index position. of the input string.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:charAt(<STRING> input.value, <INT> index)
```

QUERY PARAMETERS

| Name        | Description                                                                          | Default Value | Possible Data Types | Optional | Dynamic |
| ----------- | ------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| input.value | The input string of which the char value at the given position needs to be returned. |               | STRING              | No       | No      |
| index       | The variable that specifies the index of the char value that needs to be returned.   |               | INT                 | No       | No      |

ExamplesEXAMPLE 1

```
charAt("WSO2", 1)
```

In this case, the functiion returns the character that exists at index 1. Hence, it returns 'S'.

#### coalesce [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#coalesce-function_1" id="coalesce-function_1"></a>

This returns the first input parameter value of the given argument, that is not null.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> str:coalesce(<INT|LONG|DOUBLE|FLOAT|STRING|BOOL|OBJECT> argn)
```

QUERY PARAMETERS

| Name | Description                                                                                                                           | Default Value | Possible Data Types                                               | Optional | Dynamic |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ----------------------------------------------------------------- | -------- | ------- |
| argn | It can have one or more input parameters in any data type. However, all the specified parameters are required to be of the same type. |               | <p>INT<br>LONG<br>DOUBLE<br>FLOAT<br>STRING<br>BOOL<br>OBJECT</p> | No       | No      |

ExamplesEXAMPLE 1

```
coalesce(null, "BBB", "CCC")
```

This returns the first input parameter that is not null. In this example, it returns "BBB".

#### concat [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#concat-function" id="concat-function"></a>

This function returns a string value that is obtained as a result of concatenating two or more input string values.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:concat(<STRING> argn)
```

QUERY PARAMETERS

| Name | Description                                               | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | --------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| argn | This can have two or more `string` type input parameters. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
concat("D533", "8JU^", "XYZ")
```

This returns a string value by concatenating two or more given arguments. In the example shown above, it returns "D5338JU^XYZ".

#### contains [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#contains-function" id="contains-function"></a>

This function returns `true` if the`input.string` contains the specified sequence of char values in the `search.string`.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<BOOL> str:contains(<STRING> input.string, <STRING> search.string)
```

QUERY PARAMETERS

| Name          | Description                                                | Default Value | Possible Data Types | Optional | Dynamic |
| ------------- | ---------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string  | Input string value.                                        |               | STRING              | No       | No      |
| search.string | The string value to be searched for in the `input.string`. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
contains("21 products are produced by WSO2 currently", "WSO2")
```

This returns a boolean value as the output. In this case, it returns`true`.

#### equalsIgnoreCase [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#equalsignorecase-function" id="equalsignorecase-function"></a>

This returns a boolean value by comparing two strings lexicographically without considering the letter case.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<BOOL> str:equalsIgnoreCase(<STRING> arg1, <STRING> arg2)
```

QUERY PARAMETERS

| Name | Description                                                                 | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| arg1 | The first input string argument.                                            |               | STRING              | No       | No      |
| arg2 | The second input string argument. This is compared with the first argument. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
equalsIgnoreCase("WSO2", "wso2")
```

This returns a boolean value as the output. In this scenario, it returns "true".

#### fillTemplate [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#filltemplate-function" id="filltemplate-function"></a>

This extension replaces the templated positions that are marked with an index value in a specified template with the strings provided.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:fillTemplate(<STRING> template, <STRING|INT|LONG|DOUBLE|FLOAT|BOOL> replacement.strings)
```

QUERY PARAMETERS

| Name                | Description                                                                                                                                                                                                                                                                        | Default Value | Possible Data Types                                     | Optional | Dynamic |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------------------------- | -------- | ------- |
| template            | <p>The string with templated fields that needs to be filled with the given strings. The format of the templated fields should be as follows:<br>{{INDEX}} where 'INDEX' is an integer.<br>This index is used to map the strings that are used to replace the templated fields.</p> |               | STRING                                                  | No       | No      |
| replacement.strings | <p>The strings with which the templated positions in the template need to be replaced.<br>The minimum of two arguments need to be included in the execution string. There is no upper limit on the number of arguments allowed to be included.</p>                                 |               | <p>STRING<br>INT<br>LONG<br>DOUBLE<br>FLOAT<br>BOOL</p> | No       | No      |

ExamplesEXAMPLE 1

```
str:fillTemplate("This is {{1}} for the {{2}} function", 'an example', 'fillTemplate')
```

In this example, the template is 'This is \{{1\}} for the \{{2\}} function'.Here, the templated string \{{1\}} is replaced with the 1st string value provided, which is 'an example'.\
\{{2\}} is replaced with the 2nd string provided, which is 'fillTemplate'\
The complete return string is 'This is an example for the fillTemplate function'.

#### hex [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#hex-function_1" id="hex-function_1"></a>

This function returns a hexadecimal string by converting each byte of each character in the input string to two hexadecimal digits.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:hex(<STRING> input.string)
```

QUERY PARAMETERS

| Name         | Description                                       | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | ------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string to derive the hexadecimal value. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
hex("MySQL") 
```

This returns the hexadecimal value of the input.string. In this scenario, the output is "4d7953514c".

#### length [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#length-function" id="length-function"></a>

Returns the length of the input string.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<INT> str:length(<STRING> input.string)
```

QUERY PARAMETERS

| Name         | Description                            | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | -------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string to derive the length. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
length("Hello World")
```

This outputs the length of the provided string. In this scenario, the, output is `11` .

#### lower [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#lower-function" id="lower-function"></a>

Converts the capital letters in the input string to the equivalent simple letters.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:lower(<STRING> input.string)
```

QUERY PARAMETERS

| Name         | Description                                                                      | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | -------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string to convert to the lower case (i.e., equivalent simple letters). |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
lower("WSO2 cep ")
```

This converts the capital letters in the input.string to the equivalent simple letters. In this scenario, the output is "wso2 cep ".

#### regexp [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#regexp-function" id="regexp-function"></a>

Returns a boolean value based on the matchability of the input string and the given regular expression.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<BOOL> str:regexp(<STRING> input.string, <STRING> regex)
```

QUERY PARAMETERS

| Name         | Description                                                  | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | ------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| input.string | The input string to match with the given regular expression. |               | STRING              | No       | No      |
| regex        | The regular expression to be matched with the input string.  |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
regexp("WSO2 abcdh", "WSO(.*h)")
```

This returns a boolean value after matching regular expression with the given string. In this scenario, it returns "true" as the output.

#### repeat [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#repeat-function" id="repeat-function"></a>

Repeats the input string for a specified number of times.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:repeat(<STRING> input.string, <INT> times)
```

QUERY PARAMETERS

| Name         | Description                                                                   | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | ----------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string that is repeated the number of times as defined by the user. |               | STRING              | No       | No      |
| times        | The number of times the input.string needs to be repeated .                   |               | INT                 | No       | No      |

ExamplesEXAMPLE 1

```
repeat("StRing 1", 3)
```

This returns a string value by repeating the string for a specified number of times. In this scenario, the output is "StRing 1StRing 1StRing 1".

#### replaceAll [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#replaceall-function" id="replaceall-function"></a>

Finds all the substrings of the input string that matches with the given expression, and replaces them with the given replacement string.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:replaceAll(<STRING> input.string, <STRING> regex, <STRING> replacement.string)
```

QUERY PARAMETERS

| Name               | Description                                                                                | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------ | ------------------------------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| input.string       | The input string to be replaced.                                                           |               | STRING              | No       | No      |
| regex              | The regular expression to be matched with the input string.                                |               | STRING              | No       | No      |
| replacement.string | The string with which each substring that matches the given expression should be replaced. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
replaceAll("hello hi hello",  'hello', 'test')
```

This returns a string after replacing the substrings of the input string with the replacement string. In this scenario, the output is "test hi test" .

#### replaceFirst [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#replacefirst-function" id="replacefirst-function"></a>

Finds the first substring of the input string that matches with the given regular expression, and replaces itwith the given replacement string.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:replaceFirst(<STRING> input.string, <STRING> regex, <STRING> replacement.string)
```

QUERY PARAMETERS

| Name               | Description                                                                                                       | Default Value | Possible Data Types | Optional | Dynamic |
| ------------------ | ----------------------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string       | The input string that should be replaced.                                                                         |               | STRING              | No       | No      |
| regex              | The regular expression with which the input string should be matched.                                             |               | STRING              | No       | No      |
| replacement.string | The string with which the first substring of input string that matches the regular expression should be replaced. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
replaceFirst("hello WSO2 A hello",  'WSO2(.*)A', 'XXXX')
```

This returns a string after replacing the first substring with the given replacement string. In this scenario, the output is "hello XXXX hello".

#### reverse [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#reverse-function" id="reverse-function"></a>

Returns the input string in the reverse order character-wise and string-wise.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:reverse(<STRING> input.string)
```

QUERY PARAMETERS

| Name         | Description                      | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | -------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string to be reversed. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
reverse("Hello World")
```

This outputs a string value by reversing the incoming `input.string`. In this scenario, the output is "dlroW olleH".

#### split [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#split-function" id="split-function"></a>

Splits the `input.string` into substrings using the value parsed in the `split.string` and returns the substring at the position specified in the `group.number`.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:split(<STRING> input.string, <STRING> split.string, <INT> group.number)
```

QUERY PARAMETERS

| Name         | Description                                              | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | -------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string to be replaced.                         |               | STRING              | No       | No      |
| split.string | The string value to be used to split the `input.string`. |               | STRING              | No       | No      |
| group.number | The index of the split group                             |               | INT                 | No       | No      |

ExamplesEXAMPLE 1

```
split("WSO2,ABM,NSFT", ",", 0)
```

This splits the given `input.string` by given `split.string` and returns the string in the index given by group.number. In this scenario, the output will is "WSO2".

#### strcmp [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#strcmp-function" id="strcmp-function"></a>

Compares two strings lexicographically and returns an integer value. If both strings are equal, 0 is returned. If the first string is lexicographically greater than the second string, a positive value is returned. If the first string is lexicographically greater than the second string, a negative value is returned.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<INT> str:strcmp(<STRING> arg1, <STRING> arg2)
```

QUERY PARAMETERS

| Name | Description                                                                                         | Default Value | Possible Data Types | Optional | Dynamic |
| ---- | --------------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| arg1 | The first input string argument.                                                                    |               | STRING              | No       | No      |
| arg2 | The second input string argument that should be compared with the first argument lexicographically. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
strcmp("AbCDefghiJ KLMN", 'Hello')
```

This compares two strings lexicographically and outputs an integer value.

#### substr [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#substr-function" id="substr-function"></a>

Returns a substring of the input string by considering a subset or all of the following factors: starting index, length, regular expression, and regex group number.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:substr(<STRING> input.string, <INT> begin.index, <INT> length, <STRING> regex, <INT> group.number)
```

QUERY PARAMETERS

| Name         | Description                                                          | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | -------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string to be processed.                                    |               | STRING              | No       | No      |
| begin.index  | Starting index to consider for the substring.                        |               | INT                 | No       | No      |
| length       | The length of the substring.                                         |               | INT                 | No       | No      |
| regex        | The regular expression that should be matched with the input string. |               | STRING              | No       | No      |
| group.number | The regex group number                                               |               | INT                 | No       | No      |

ExamplesEXAMPLE 1

```
substr("AbCDefghiJ KLMN", 4)
```

This outputs the substring based on the given `begin.index`. In this scenario, the output is "efghiJ KLMN".

EXAMPLE 2

```
substr("AbCDefghiJ KLMN",  2, 4) 
```

This outputs the substring based on the given `begin.index` and length. In this scenario, the output is "CDef".

EXAMPLE 3

```
substr("WSO2D efghiJ KLMN", '^WSO2(.*)')
```

This outputs the substring by applying the regex. In this scenario, the output is "WSO2D efghiJ KLMN".

EXAMPLE 4

```
substr("WSO2 cep WSO2 XX E hi hA WSO2 heAllo",  'WSO2(.*)A(.*)',  2)
```

This outputs the substring by applying the regex and considering the `group.number`. In this scenario, the output is " ello".

#### trim [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#trim-function" id="trim-function"></a>

Returns a copy of the input string without the leading and trailing whitespace (if any).

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:trim(<STRING> input.string)
```

QUERY PARAMETERS

| Name         | Description                                | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | ------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| input.string | The input string that needs to be trimmed. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
trim("  AbCDefghiJ KLMN  ")
```

This returns a copy of the `input.string` with the leading and/or trailing white-spaces omitted. In this scenario, the output is "AbCDefghiJ KLMN".

#### unhex [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#unhex-function" id="unhex-function"></a>

Returns a string by converting the hexadecimal characters in the input string.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:unhex(<STRING> input.string)
```

QUERY PARAMETERS

| Name         | Description                                                        | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | ------------------------------------------------------------------ | ------------- | ------------------- | -------- | ------- |
| input.string | The hexadecimal input string that needs to be converted to string. |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
unhex("4d7953514c")
```

This converts the hexadecimal value to string.

#### upper [_(Function)_](https://siddhi.io/en/v5.0/docs/query-guide/#function) <a href="#upper-function" id="upper-function"></a>

Converts the simple letters in the input string to the equivalent capital/block letters.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
<STRING> str:upper(<STRING> input.string)
```

QUERY PARAMETERS

| Name         | Description                                                                                     | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | ----------------------------------------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string that should be converted to the upper case (equivalent capital/block letters). |               | STRING              | No       | No      |

ExamplesEXAMPLE 1

```
upper("Hello World")
```

This converts the simple letters in the `input.string` to theequivalent capital letters. In this scenario, the output is "HELLO WORLD".

#### tokenize [_(Stream Processor)_](https://siddhi.io/en/v5.0/docs/query-guide/#stream-processor) <a href="#tokenize-stream-processor_1" id="tokenize-stream-processor_1"></a>

This function splits the input string into tokens using a given regular expression and returns the split tokens.

_Origin: siddhi-execution-string:5.0.1_

Syntax

```
str:tokenize(<STRING> input.string, <STRING> regex, <BOOL> distinct)
```

QUERY PARAMETERS

| Name         | Description                                                    | Default Value | Possible Data Types | Optional | Dynamic |
| ------------ | -------------------------------------------------------------- | ------------- | ------------------- | -------- | ------- |
| input.string | The input string which needs to be split.                      |               | STRING              | No       | No      |
| regex        | The string value which is used to tokenize the 'input.string'. |               | STRING              | No       | No      |
| distinct     | This flag is used to return only distinct values.              | false         | BOOL                | Yes      | No      |

Extra Return Attributes

| Name  | Description                                  | Possible Types |
| ----- | -------------------------------------------- | -------------- |
| token | The attribute which contains a single token. | STRING         |

ExamplesEXAMPLE 1

```
define stream inputStream (str string);
@info(name = 'query1')
from inputStream#str:tokenize(str , ',')
select text
insert into outputStream;
```

This query performs tokenization on the given string. If the str is "Android,Windows8,iOS", then the string is split into 3 events containing the `token` attribute values, i.e., `Android`, `Windows8` and `iOS`.
