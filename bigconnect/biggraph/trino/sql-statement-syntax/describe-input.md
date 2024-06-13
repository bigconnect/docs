# DESCRIBE INPUT

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DESCRIBE INPUT statement_name
```



### Description <a href="#description" id="description"></a>

Lists the input parameters of a prepared statement along with the position and type of each parameter. Parameter types that cannot be determined will appear as `unknown`.

### Examples <a href="#examples" id="examples"></a>

Prepare and describe a query with three parameters:

```
PREPARE my_select1 FROM
SELECT ? FROM nation WHERE regionkey = ? AND name < ?;
```



```
DESCRIBE INPUT my_select1;
```



```
 Position | Type
--------------------
        0 | unknown
        1 | bigint
        2 | varchar
(3 rows)
```



Prepare and describe a query with no parameters:

```
PREPARE my_select2 FROM
SELECT * FROM nation;
```



```
DESCRIBE INPUT my_select2;
```



```
 Position | Type
-----------------
(0 rows)
```



### See also <a href="#see-also" id="see-also"></a>

PREPARE
