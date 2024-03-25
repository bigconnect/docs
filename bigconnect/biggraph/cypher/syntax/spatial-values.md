# Spatial values

BigConnect supports only one type of spatial geometry, the _Point_ with the following characteristics:

* Each point can have either 2 or 3 dimensions. This means it contains either 2 or 3 64-bit floating point values, which together are called the _Coordinate_.
* Each point will also be associated with a specific Coordinate Reference System (CRS) that determines the meaning of the values in the _Coordinate_.
* Instances of _Point_ and lists of _Point_ can be assigned to node and relationship properties.
* Nodes with _Point_ or _List(Point)_ properties can be indexed using a spatial index. This is true for all CRS (and for both 2D and 3D).&#x20;
* The distance function will work on points in all CRS and in both 2D and 3D but only if the two points have the same CRS (and therefore also same dimension).

### Coordinate Reference Systems

Four Coordinate Reference Systems (CRS) are supported, each of which falls within one of two types: _geographic coordinates_ modeling points on the earth, or _cartesian coordinates_ modeling points in euclidean space:

* Geographic coordinate reference systems
  * WGS-84: longitude, latitude (x, y)
  * WGS-84-3D: longitude, latitude, height (x, y, z)
* Cartesian coordinate reference systems
  * Cartesian: x, y
  * Cartesian 3D: x, y, z

Data within different coordinate systems are entirely incomparable, and cannot be implicitly converted from one to the other. This is true even if they are both cartesian or both geographic. For example, if you search for 3D points using a 2D range, you will get no results.&#x20;

#### Geographic coordinate reference systems

Two Geographic Coordinate Reference Systems (CRS) are supported, modeling points on the earth:

* [WGS 84 2D](http://spatialreference.org/ref/epsg/4326/)
  * A 2D geographic point in the _WGS 84_ CRS is specified in one of two ways:
    * `longitude` and `latitude` (if these are specified, and the `crs` is not, then the `crs` is assumed to be `WGS-84`)
    * `x` and `y` (in this case the `crs` must be specified, or will be assumed to be `Cartesian`)
  * Specifying this CRS can be done using either the name 'wgs-84' or the SRID 4326 as described in Point(WGS-84)
* [WGS 84 3D](http://spatialreference.org/ref/epsg/4979/)
  * A 3D geographic point in the _WGS 84_ CRS is specified one of in two ways:
    * `longitude`, `latitude` and either `height` or `z` (if these are specified, and the `crs` is not, then the `crs` is assumed to be `WGS-84-3D`)
    * `x`, `y` and `z` (in this case the `crs` must be specified, or will be assumed to be `Cartesian-3D`)
  * Specifying this CRS can be done using either the name 'wgs-84-3d' or the SRID 4979 as described in Point(WGS-84-3D)

The units of the `latitude` and `longitude` fields are in decimal degrees, and need to be specified as floating point numbers using Cypher literals. It is not possible to use any other format, like 'degrees, minutes, seconds'. The units of the `height` field are in meters. When geographic points are passed to the `distance` function, the result will always be in meters. If the coordinates are in any other format or unit than supported, it is necessary to explicitly convert them. For example, if the incoming `$height` is a string field in kilometers, you would need to type `height: toFloat($height) * 1000`. Likewise if the results of the `distance` function are expected to be returned in kilometers, an explicit conversion is required. For example: `RETURN distance(a,b) / 1000 AS km`. An example demonstrating conversion on incoming and outgoing values is:

Query

```cypher
WITH point({ latitude:toFloat('13.43'), longitude:toFloat('56.21')}) AS p1, point({ latitude:toFloat('13.10'), longitude:toFloat('56.41')}) AS p2
RETURN toInteger(distance(p1,p2)/1000) AS km
```

| km    |
| ----- |
| `42`  |
| 1 row |

#### Cartesian coordinate reference systems

Two Cartesian Coordinate Reference Systems (CRS) are supported, modeling points in euclidean space:

* [Cartesian 2D](http://spatialreference.org/ref/sr-org/7203/)
  * A 2D point in the _Cartesian_ CRS is specified with a map containing `x` and `y` coordinate values
  * Specifying this CRS can be done using either the name 'cartesian' or the SRID 7203 as described in Point(Cartesian)
* [Cartesian 3D](http://spatialreference.org/ref/sr-org/9157/)
  * A 3D point in the _Cartesian_ CRS is specified with a map containing `x`, `y` and `z` coordinate values
  * Specifying this CRS can be done using either the name 'cartesian-3d' or the SRID 9157 as described in Point(Cartesian-3D)

The units of the `x`, `y` and `z` fields are unspecified and can mean anything the user intends them to mean. This also means that when two cartesian points are passed to the `distance` function, the resulting value will be in the same units as the original coordinates. This is true for both 2D and 3D points, as the _pythagoras_ equation used is generalized to any number of dimensions. However, just as you cannot compare geographic points to cartesian points, you cannot calculate the distance between a 2D point and a 3D point. If you need to do that, explicitly transform the one type into the other. For example:

Query

```cypher
WITH point({ x:3, y:0 }) AS p2d, point({ x:0, y:4, z:1 }) AS p3d
RETURN distance(p2d,p3d) AS bad, distance(p2d,point({ x:p3d.x, y:p3d.y })) AS good
```

| bad      | good  |
| -------- | ----- |
| `<null>` | `5.0` |
| 1 row    |       |

### Spatial instants

#### Creating points

All point types are created from two components:

* The _Coordinate_ containing either 2 or 3 floating point values (64-bit)
* The Coordinate Reference System (or CRS) defining the meaning (and possibly units) of the values in the _Coordinate_

For most use cases it is not necessary to specify the CRS explicitly as it will be deduced from the keys used to specify the coordinate. Two rules are applied to deduce the CRS from the coordinate:

* Choice of keys:
  * If the coordinate is specified using the keys `latitude` and `longitude` the CRS will be assumed to be _Geographic_ and therefor either `WGS-84` or `WGS-84-3D`.
  * If instead `x` and `y` are used, then the default CRS would be `Cartesian` or `Cartesian-3D`
* Number of dimensions:
  * If there are 2 dimensions in the coordinate, `x` & `y` or `longitude` & `latitude` the CRS will be a 2D CRS
  * If there is a third dimensions in the coordinate, `z` or `height` the CRS will be a 3D CRS

All fields are provided to the `point` function in the form of a map of explicitly named arguments. We specifically do not support an ordered list of coordinate fields because of the contradictory conventions between geographic and cartesian coordinates, where geographic coordinates normally list `y` before `x` (`latitude` before `longitude`). See for example the following query which returns points created in each of the four supported CRS. Take particular note of the order and keys of the coordinates in the original `point` function calls, and how those values are displayed in the results:

Query

```cypher
RETURN point({ x:3, y:0 }) AS cartesian_2d, point({ x:0, y:4, z:1 }) AS cartesian_3d, point({ latitude: 12, longitude: 56 }) AS geo_2d, point({ latitude: 12, longitude: 56, height: 1000 }) AS geo_3d
```

| cartesian\_2d                               | cartesian\_3d                                          | geo\_2d                                    | geo\_3d                                                  |
| ------------------------------------------- | ------------------------------------------------------ | ------------------------------------------ | -------------------------------------------------------- |
| `point({x: 3.0, y: 0.0, crs: 'cartesian'})` | `point({x: 0.0, y: 4.0, z: 1.0, crs: 'cartesian-3d'})` | `point({x: 56.0, y: 12.0, crs: 'wgs-84'})` | `point({x: 56.0, y: 12.0, z: 1000.0, crs: 'wgs-84-3d'})` |
| 1 row                                       |                                                        |                                            |                                                          |

#### Accessing components of points

Just as we construct points using a map syntax, we can also access components as properties of the instance.

<table data-full-width="true"><thead><tr><th>Component</th><th>Description</th><th>Type</th><th>Range/Format</th><th>WGS-84</th><th>WGS-84-3D</th><th>Cartesian</th><th>Cartesian-3D</th></tr></thead><tbody><tr><td><code>instant.x</code></td><td>The first element of the <em>Coordinate</em></td><td>Float</td><td>Number literal, range depends on CRS</td><td>X</td><td>X</td><td>X</td><td>X</td></tr><tr><td><code>instant.y</code></td><td>The second element of the <em>Coordinate</em></td><td>Float</td><td>Number literal, range depends on CRS</td><td>X</td><td>X</td><td>X</td><td>X</td></tr><tr><td><code>instant.z</code></td><td>The third element of the <em>Coordinate</em></td><td>Float</td><td>Number literal, range depends on CRS</td><td></td><td>X</td><td></td><td>X</td></tr><tr><td><code>instant.latitude</code></td><td>The <em>second</em> element of the <em>Coordinate</em> for geographic CRS, degrees North of the equator</td><td>Float</td><td>Number literal, <code>-90.0</code> to <code>90.0</code></td><td>X</td><td>X</td><td></td><td></td></tr><tr><td><code>instant.longitude</code></td><td>The <em>first</em> element of the <em>Coordinate</em> for geographic CRS, degrees East of the prime meridian</td><td>Float</td><td>Number literal, <code>-180.0</code> to <code>180.0</code></td><td>X</td><td>X</td><td></td><td></td></tr><tr><td><code>instant.height</code></td><td>The third element of the <em>Coordinate</em> for geographic CRS, meters above the ellipsoid defined by the datum (WGS-84)</td><td>Float</td><td>Number literal, range limited only by the underlying 64-bit floating point type</td><td></td><td>X</td><td></td><td></td></tr><tr><td><code>instant.crs</code></td><td>The name of the CRS</td><td>String</td><td>One of <code>wgs-84</code>, <code>wgs-84-3d</code>, <code>cartesian</code>, <code>cartesian-3d</code></td><td>X</td><td>X</td><td>X</td><td>X</td></tr><tr><td><code>instant.srid</code></td><td>The internal Neo4j ID for the CRS</td><td>Integer</td><td>One of <code>4326</code>, <code>4979</code>, <code>7203</code>, <code>9157</code></td><td>X</td><td>X</td><td>X</td><td>X</td></tr></tbody></table>

The following query shows how to extract the components of a _Cartesian 2D_ point value:

Query

```cypher
WITH point({ x:3, y:4 }) AS p
RETURN p.x, p.y, p.crs, p.srid
```

| p.x   | p.y   | p.crs         | p.srid |
| ----- | ----- | ------------- | ------ |
| `3.0` | `4.0` | `"cartesian"` | `7203` |
| 1 row |       |               |        |

The following query shows how to extract the components of a _WGS-84 3D_ point value:

Query

```cypher
WITH point({ latitude:3, longitude:4, height: 4321 }) AS p
RETURN p.latitude, p.longitude, p.height, p.x, p.y, p.z, p.crs, p.srid
```

| p.latitude | p.longitude | p.height | p.x   | p.y   | p.z      | p.crs         | p.srid |
| ---------- | ----------- | -------- | ----- | ----- | -------- | ------------- | ------ |
| `3.0`      | `4.0`       | `4321.0` | `4.0` | `3.0` | `4321.0` | `"wgs-84-3d"` | `4979` |
| 1 row      |             |          |       |       |          |               |        |

### Comparability and Orderability

Points with different CRS are not comparable. This means that any function operating on two points of different types will return `null`. This is true of the distance function as well as inequality comparisons. If these are used in a predicate, they will cause the associated `MATCH` to return no results.

Query

```cypher
WITH point({ x:3, y:0 }) AS p2d, point({ x:0, y:4, z:1 }) AS p3d
RETURN distance(p2d,p3d), p2d < p3d, p2d = p3d, p2d <> p3d, distance(p2d,point({ x:p3d.x, y:p3d.y }))
```

| distance(p2d,p3d) | p2d < p3d | p2d = p3d | p2d <> p3d | distance(p2d,point({ x:p3d.x, y:p3d.y })) |
| ----------------- | --------- | --------- | ---------- | ----------------------------------------- |
| `<null>`          | `<null>`  | `false`   | `true`     | `5.0`                                     |
| 1 row             |           |           |            |                                           |

However, all types are orderable. The Point types will be ordered after Numbers and before Temporal types. Points with different CRS with be ordered by their SRID numbers. For the current set of four CRS, this means the order is WGS84, WGS84-3D, Cartesian, Cartesian-3D.

Query

```cypher
UNWIND [point({ x:3, y:0 }), point({ x:0, y:4, z:1 }), point({ srid:4326, x:12, y:56 }), point({ srid:4979, x:12, y:56, z:1000 })] AS point
RETURN point
ORDER BY point
```

| point                                                    |
| -------------------------------------------------------- |
| `point({x: 12.0, y: 56.0, crs: 'wgs-84'})`               |
| `point({x: 12.0, y: 56.0, z: 1000.0, crs: 'wgs-84-3d'})` |
| `point({x: 3.0, y: 0.0, crs: 'cartesian'})`              |
| `point({x: 0.0, y: 4.0, z: 1.0, crs: 'cartesian-3d'})`   |
| 4 rows                                                   |
