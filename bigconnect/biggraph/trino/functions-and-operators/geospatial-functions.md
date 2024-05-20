# Geospatial functions

Trino Geospatial functions that begin with the `ST_` prefix support the SQL/MM specification and are compliant with the Open Geospatial Consortium’s (OGC) OpenGIS Specifications. As such, many Trino Geospatial functions require, or more accurately, assume that geometries that are operated on are both simple and valid. For example, it does not make sense to calculate the area of a polygon that has a hole defined outside of the polygon, or to construct a polygon from a non-simple boundary line.

Trino Geospatial functions support the Well-Known Text (WKT) and Well-Known Binary (WKB) form of spatial objects:

* `POINT (0 0)`
* `LINESTRING (0 0, 1 1, 1 2)`
* `POLYGON ((0 0, 4 0, 4 4, 0 4, 0 0), (1 1, 2 1, 2 2, 1 2, 1 1))`
* `MULTIPOINT (0 0, 1 2)`
* `MULTILINESTRING ((0 0, 1 1, 1 2), (2 3, 3 2, 5 4))`
* `MULTIPOLYGON (((0 0, 4 0, 4 4, 0 4, 0 0), (1 1, 2 1, 2 2, 1 2, 1 1)), ((-1 -1, -1 -2, -2 -2, -2 -1, -1 -1)))`
* `GEOMETRYCOLLECTION (POINT(2 3), LINESTRING (2 3, 3 4))`

Use `ST_GeometryFromText()` and `ST_GeomFromBinary()` functions to create geometry objects from WKT or WKB.

The `SphericalGeography` type provides native support for spatial features represented on _geographic_ coordinates (sometimes called _geodetic_ coordinates, or _lat/lon_, or _lon/lat_). Geographic coordinates are spherical coordinates expressed in angular units (degrees).

The basis for the `Geometry` type is a plane. The shortest path between two points on the plane is a straight line. That means calculations on geometries (areas, distances, lengths, intersections, etc) can be calculated using cartesian mathematics and straight line vectors.

The basis for the `SphericalGeography` type is a sphere. The shortest path between two points on the sphere is a great circle arc. That means that calculations on geographies (areas, distances, lengths, intersections, etc) must be calculated on the sphere, using more complicated mathematics. More accurate measurements that take the actual spheroidal shape of the world into account are not supported.

Values returned by the measurement functions `ST_Distance()` and `ST_Length()` are in the unit of meters; values returned by `ST_Area()` are in square meters.

Use `to_spherical_geography()` function to convert a geometry object to geography object.

For example, `ST_Distance(ST_Point(-71.0882, 42.3607), ST_Point(-74.1197, 40.6976))` returns `3.4577` in the unit of the passed-in values on the euclidean plane, while `ST_Distance(to_spherical_geography(ST_Point(-71.0882, 42.3607)), to_spherical_geography(ST_Point(-74.1197, 40.6976)))` returns `312822.179` in meters.

### Constructors[#](https://trino.io/docs/current/functions/geospatial.html#constructors) <a href="#constructors" id="constructors"></a>

ST\_AsBinary(_Geometry_) → varbinary[#](https://trino.io/docs/current/functions/geospatial.html#ST\_AsBinary)

Returns the WKB representation of the geometry.

ST\_AsText(_Geometry_) → varchar[#](https://trino.io/docs/current/functions/geospatial.html#ST\_AsText)

Returns the WKT representation of the geometry. For empty geometries, `ST_AsText(ST_LineFromText('LINESTRING EMPTY'))` will produce `'MULTILINESTRING EMPTY'` and `ST_AsText(ST_Polygon('POLYGON EMPTY'))` will produce `'MULTIPOLYGON EMPTY'`.

ST\_GeometryFromText(_varchar_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_GeometryFromText)

Returns a geometry type object from WKT representation.

ST\_GeomFromBinary(_varbinary_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_GeomFromBinary)

Returns a geometry type object from WKB representation.

geometry\_from\_hadoop\_shape(_varbinary_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#geometry\_from\_hadoop\_shape)

Returns a geometry type object from Spatial Framework for Hadoop representation.

ST\_LineFromText(_varchar_) → LineString[#](https://trino.io/docs/current/functions/geospatial.html#ST\_LineFromText)

Returns a geometry type linestring object from WKT representation.

ST\_LineString(_array(Point)_) → LineString[#](https://trino.io/docs/current/functions/geospatial.html#ST\_LineString)

Returns a LineString formed from an array of points. If there are fewer than two non-empty points in the input array, an empty LineString will be returned. Array elements must not be `NULL` or the same as the previous element. The returned geometry may not be simple, e.g. may self-intersect or may contain duplicate vertexes depending on the input.

ST\_MultiPoint(_array(Point)_) → MultiPoint[#](https://trino.io/docs/current/functions/geospatial.html#ST\_MultiPoint)

Returns a MultiPoint geometry object formed from the specified points. Returns `NULL` if input array is empty. Array elements must not be `NULL` or empty. The returned geometry may not be simple and may contain duplicate points if input array has duplicates.

ST\_Point(double, double) -> Point

Returns a geometry type point object with the given coordinate values.

ST\_Polygon(_varchar_) → Polygon[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Polygon)

Returns a geometry type polygon object from WKT representation.

to\_spherical\_geography(_Geometry_) → SphericalGeography[#](https://trino.io/docs/current/functions/geospatial.html#to\_spherical\_geography)

Converts a Geometry object to a SphericalGeography object on the sphere of the Earth’s radius. This function is only applicable to `POINT`, `MULTIPOINT`, `LINESTRING`, `MULTILINESTRING`, `POLYGON`, `MULTIPOLYGON` geometries defined in 2D space, or `GEOMETRYCOLLECTION` of such geometries. For each point of the input geometry, it verifies that `point.x` is within `[-180.0, 180.0]` and `point.y` is within `[-90.0, 90.0]`, and uses them as (longitude, latitude) degrees to construct the shape of the `SphericalGeography` result.

to\_geometry(_SphericalGeography_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#to\_geometry)

Converts a SphericalGeography object to a Geometry object.

### Relationship tests[#](https://trino.io/docs/current/functions/geospatial.html#relationship-tests) <a href="#relationship-tests" id="relationship-tests"></a>

ST\_Contains(Geometry, Geometry) -> boolean

Returns `true` if and only if no points of the second geometry lie in the exterior of the first geometry, and at least one point of the interior of the first geometry lies in the interior of the second geometry.

ST\_Crosses(Geometry, Geometry) -> boolean

Returns `true` if the supplied geometries have some, but not all, interior points in common.

ST\_Disjoint(Geometry, Geometry) -> boolean

Returns `true` if the give geometries do not _spatially intersect_ – if they do not share any space together.

ST\_Equals(Geometry, Geometry) -> boolean

Returns `true` if the given geometries represent the same geometry.

ST\_Intersects(Geometry, Geometry) -> boolean

Returns `true` if the given geometries spatially intersect in two dimensions (share any portion of space) and `false` if they do not (they are disjoint).

ST\_Overlaps(Geometry, Geometry) -> boolean

Returns `true` if the given geometries share space, are of the same dimension, but are not completely contained by each other.

ST\_Relate(Geometry, Geometry) -> boolean

Returns `true` if first geometry is spatially related to second geometry.

ST\_Touches(Geometry, Geometry) -> boolean

Returns `true` if the given geometries have at least one point in common, but their interiors do not intersect.

ST\_Within(Geometry, Geometry) -> boolean

Returns `true` if first geometry is completely inside second geometry.

### Operations[#](https://trino.io/docs/current/functions/geospatial.html#operations) <a href="#operations" id="operations"></a>

geometry\_nearest\_points(Geometry, Geometry) -> row(Point, Point)

Returns the points on each geometry nearest the other. If either geometry is empty, return `NULL`. Otherwise, return a row of two Points that have the minimum distance of any two points on the geometries. The first Point will be from the first Geometry argument, the second from the second Geometry argument. If there are multiple pairs with the minimum distance, one pair is chosen arbitrarily.

geometry\_union(_array(Geometry)_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#geometry\_union)

Returns a geometry that represents the point set union of the input geometries. Performance of this function, in conjunction with `array_agg()` to first aggregate the input geometries, may be better than `geometry_union_agg()`, at the expense of higher memory utilization.

ST\_Boundary(_Geometry_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Boundary)

Returns the closure of the combinatorial boundary of this geometry.

ST\_Buffer(_Geometry_, _distance_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Buffer)

Returns the geometry that represents all points whose distance from the specified geometry is less than or equal to the specified distance. If the points of the geometry are extremely close together (`delta < 1e-8`), this might return an empty geometry.

ST\_Difference(Geometry, Geometry) -> Geometry

Returns the geometry value that represents the point set difference of the given geometries.

ST\_Envelope(_Geometry_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Envelope)

Returns the bounding rectangular polygon of a geometry.

ST\_EnvelopeAsPts(_Geometry_)[#](https://trino.io/docs/current/functions/geospatial.html#ST\_EnvelopeAsPts)

Returns an array of two points: the lower left and upper right corners of the bounding rectangular polygon of a geometry. Returns `NULL` if input geometry is empty.

ST\_ExteriorRing(_Geometry_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_ExteriorRing)

Returns a line string representing the exterior ring of the input polygon.

ST\_Intersection(Geometry, Geometry) -> Geometry

Returns the geometry value that represents the point set intersection of two geometries.

ST\_SymDifference(Geometry, Geometry) -> Geometry

Returns the geometry value that represents the point set symmetric difference of two geometries.

ST\_Union(Geometry, Geometry) -> Geometry

Returns a geometry that represents the point set union of the input geometries.

See also: `geometry_union()`, `geometry_union_agg()`

### Accessors# <a href="#accessors" id="accessors"></a>

ST\_Area(_Geometry_) → double[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Area)

Returns the 2D Euclidean area of a geometry.

For Point and LineString types, returns 0.0. For GeometryCollection types, returns the sum of the areas of the individual geometries.

ST\_Area(_SphericalGeography_) → double

Returns the area of a polygon or multi-polygon in square meters using a spherical model for Earth.

ST\_Centroid(_Geometry_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Centroid)

Returns the point value that is the mathematical centroid of a geometry.

ST\_ConvexHull(_Geometry_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_ConvexHull)

Returns the minimum convex geometry that encloses all input geometries.

ST\_CoordDim(_Geometry_) → bigint[#](https://trino.io/docs/current/functions/geospatial.html#ST\_CoordDim)

Returns the coordinate dimension of the geometry.

ST\_Dimension(_Geometry_) → bigint[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Dimension)

Returns the inherent dimension of this geometry object, which must be less than or equal to the coordinate dimension.

ST\_Distance(Geometry, Geometry) -> double

Returns the 2-dimensional cartesian minimum distance (based on spatial ref) between two geometries in projected units.

ST\_Distance(SphericalGeography, SphericalGeography) -> double

Returns the great-circle distance in meters between two SphericalGeography points.

ST\_GeometryN(_Geometry_, _index_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#ST\_GeometryN)

Returns the geometry element at a given index (indices start at 1). If the geometry is a collection of geometries (e.g., GEOMETRYCOLLECTION or MULTI\*), returns the geometry at a given index. If the given index is less than 1 or greater than the total number of elements in the collection, returns `NULL`. Use `ST_NumGeometries()` to find out the total number of elements. Singular geometries (e.g., POINT, LINESTRING, POLYGON), are treated as collections of one element. Empty geometries are treated as empty collections.

ST\_InteriorRingN(_Geometry_, _index_) → Geometry#

Returns the interior ring element at the specified index (indices start at 1). If the given index is less than 1 or greater than the total number of interior rings in the input geometry, returns `NULL`. The input geometry must be a polygon. Use `ST_NumInteriorRing()` to find out the total number of elements.

ST\_GeometryType(_Geometry_) → varchar#

Returns the type of the geometry.

ST\_IsClosed(_Geometry_) → boolean#

Returns `true` if the linestring’s start and end points are coincident.

ST\_IsEmpty(_Geometry_) → boolean#

Returns `true` if this Geometry is an empty geometrycollection, polygon, point etc.

ST\_IsSimple(_Geometry_) → boolean[#](https://trino.io/docs/current/functions/geospatial.html#ST\_IsSimple)

Returns `true` if this Geometry has no anomalous geometric points, such as self intersection or self tangency.

ST\_IsRing(_Geometry_) → boolean[#](https://trino.io/docs/current/functions/geospatial.html#ST\_IsRing)

Returns `true` if and only if the line is closed and simple.

ST\_IsValid(_Geometry_) → boolean[#](https://trino.io/docs/current/functions/geospatial.html#ST\_IsValid)

Returns `true` if and only if the input geometry is well formed. Use `geometry_invalid_reason()` to determine why the geometry is not well formed.

ST\_Length(_Geometry_) → double#

Returns the length of a linestring or multi-linestring using Euclidean measurement on a two dimensional plane (based on spatial ref) in projected units.

ST\_Length(_SphericalGeography_) → double

Returns the length of a linestring or multi-linestring on a spherical model of the Earth. This is equivalent to the sum of great-circle distances between adjacent points on the linestring.

ST\_PointN(_LineString_, _index_) → Point[#](https://trino.io/docs/current/functions/geospatial.html#ST\_PointN)

Returns the vertex of a linestring at a given index (indices start at 1). If the given index is less than 1 or greater than the total number of elements in the collection, returns `NULL`. Use `ST_NumPoints()` to find out the total number of elements.

ST\_Points(_Geometry_)[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Points)

Returns an array of points in a linestring.

ST\_XMax(_Geometry_) → double[#](https://trino.io/docs/current/functions/geospatial.html#ST\_XMax)

Returns X maxima of a bounding box of a geometry.

ST\_YMax(_Geometry_) → double[#](https://trino.io/docs/current/functions/geospatial.html#ST\_YMax)

Returns Y maxima of a bounding box of a geometry.

ST\_XMin(_Geometry_) → double[#](https://trino.io/docs/current/functions/geospatial.html#ST\_XMin)

Returns X minima of a bounding box of a geometry.

ST\_YMin(_Geometry_) → double[#](https://trino.io/docs/current/functions/geospatial.html#ST\_YMin)

Returns Y minima of a bounding box of a geometry.

ST\_StartPoint(_Geometry_) → point[#](https://trino.io/docs/current/functions/geospatial.html#ST\_StartPoint)

Returns the first point of a LineString geometry as a Point. This is a shortcut for `ST_PointN(geometry, 1)`.

simplify\_geometry(_Geometry_, _double_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#simplify\_geometry)

Returns a “simplified” version of the input geometry using the Douglas-Peucker algorithm. Will avoid creating derived geometries (polygons in particular) that are invalid.

ST\_EndPoint(_Geometry_) → point[#](https://trino.io/docs/current/functions/geospatial.html#ST\_EndPoint)

Returns the last point of a LineString geometry as a Point. This is a shortcut for `ST_PointN(geometry, ST_NumPoints(geometry))`.

ST\_X(_Point_) → double[#](https://trino.io/docs/current/functions/geospatial.html#ST\_X)

Returns the X coordinate of the point.

ST\_Y(_Point_) → double[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Y)

Returns the Y coordinate of the point.

ST\_InteriorRings(_Geometry_)[#](https://trino.io/docs/current/functions/geospatial.html#ST\_InteriorRings)

Returns an array of all interior rings found in the input geometry, or an empty array if the polygon has no interior rings. Returns `NULL` if the input geometry is empty. The input geometry must be a polygon.

ST\_NumGeometries(_Geometry_) → bigint[#](https://trino.io/docs/current/functions/geospatial.html#ST\_NumGeometries)

Returns the number of geometries in the collection. If the geometry is a collection of geometries (e.g., GEOMETRYCOLLECTION or MULTI\*), returns the number of geometries, for single geometries returns 1, for empty geometries returns 0.

ST\_Geometries(_Geometry_)[#](https://trino.io/docs/current/functions/geospatial.html#ST\_Geometries)

Returns an array of geometries in the specified collection. Returns a one-element array if the input geometry is not a multi-geometry. Returns `NULL` if input geometry is empty.

ST\_NumPoints(_Geometry_) → bigint[#](https://trino.io/docs/current/functions/geospatial.html#ST\_NumPoints)

Returns the number of points in a geometry. This is an extension to the SQL/MM `ST_NumPoints` function which only applies to point and linestring.

ST\_NumInteriorRing(_Geometry_) → bigint[#](https://trino.io/docs/current/functions/geospatial.html#ST\_NumInteriorRing)

Returns the cardinality of the collection of interior rings of a polygon.

line\_interpolate\_point(_LineString_, _double_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#line\_interpolate\_point)

Returns a Point interpolated along a LineString at the fraction given. The fraction must be between 0 and 1, inclusive.

line\_interpolate\_points(_LineString_, _double_, _repeated_)[#](https://trino.io/docs/current/functions/geospatial.html#line\_interpolate\_points)

Returns an array of Points interpolated along a LineString. The fraction must be between 0 and 1, inclusive.

line\_locate\_point(_LineString_, _Point_) → double[#](https://trino.io/docs/current/functions/geospatial.html#line\_locate\_point)

Returns a float between 0 and 1 representing the location of the closest point on the LineString to the given Point, as a fraction of total 2d line length.

Returns `NULL` if a LineString or a Point is empty or `NULL`.

geometry\_invalid\_reason(_Geometry_) → varchar[#](https://trino.io/docs/current/functions/geospatial.html#geometry\_invalid\_reason)

Returns the reason for why the input geometry is not valid. Returns `NULL` if the input is valid.

great\_circle\_distance(_latitude1_, _longitude1_, _latitude2_, _longitude2_) → double[#](https://trino.io/docs/current/functions/geospatial.html#great\_circle\_distance)

Returns the great-circle distance between two points on Earth’s surface in kilometers.

to\_geojson\_geometry(_SphericalGeography_) → varchar[#](https://trino.io/docs/current/functions/geospatial.html#to\_geojson\_geometry)

Returns the GeoJSON encoded defined by the input spherical geography.

from\_geojson\_geometry(_varchar_) → SphericalGeography[#](https://trino.io/docs/current/functions/geospatial.html#from\_geojson\_geometry)

Returns the spherical geography type object from the GeoJSON representation stripping non geometry key/values. Feature and FeatureCollection are not supported.

### Aggregations[#](https://trino.io/docs/current/functions/geospatial.html#aggregations) <a href="#aggregations" id="aggregations"></a>

convex\_hull\_agg(_Geometry_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#convex\_hull\_agg)

Returns the minimum convex geometry that encloses all input geometries.

geometry\_union\_agg(_Geometry_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#geometry\_union\_agg)

Returns a geometry that represents the point set union of all input geometries.

### Bing tiles[#](https://trino.io/docs/current/functions/geospatial.html#bing-tiles) <a href="#bing-tiles" id="bing-tiles"></a>

These functions convert between geometries and [Bing tiles](https://msdn.microsoft.com/library/bb259689.aspx).

bing\_tile(_x_, _y_, _zoom\_level_) → BingTile[#](https://trino.io/docs/current/functions/geospatial.html#bing\_tile)

Creates a Bing tile object from XY coordinates and a zoom level. Zoom levels from 1 to 23 are supported.

bing\_tile(_quadKey_) → BingTile

Creates a Bing tile object from a quadkey.

bing\_tile\_at(_latitude_, _longitude_, _zoom\_level_) → BingTile[#](https://trino.io/docs/current/functions/geospatial.html#bing\_tile\_at)

Returns a Bing tile at a given zoom level containing a point at a given latitude and longitude. Latitude must be within `[-85.05112878, 85.05112878]` range. Longitude must be within `[-180, 180]` range. Zoom levels from 1 to 23 are supported.

bing\_tiles\_around(_latitude_, _longitude_, _zoom\_level_)[#](https://trino.io/docs/current/functions/geospatial.html#bing\_tiles\_around)

Returns a collection of Bing tiles that surround the point specified by the latitude and longitude arguments at a given zoom level.

bing\_tiles\_around(_latitude_, _longitude_, _zoom\_level_, _radius\_in\_km_)

Returns a minimum set of Bing tiles at specified zoom level that cover a circle of specified radius in km around a specified (latitude, longitude) point.

bing\_tile\_coordinates(_tile_) → row\<x, y>[#](https://trino.io/docs/current/functions/geospatial.html#bing\_tile\_coordinates)

Returns the XY coordinates of a given Bing tile.

bing\_tile\_polygon(_tile_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#bing\_tile\_polygon)

Returns the polygon representation of a given Bing tile.

bing\_tile\_quadkey(_tile_) → varchar[#](https://trino.io/docs/current/functions/geospatial.html#bing\_tile\_quadkey)

Returns the quadkey of a given Bing tile.

bing\_tile\_zoom\_level(_tile_) → tinyint[#](https://trino.io/docs/current/functions/geospatial.html#bing\_tile\_zoom\_level)

Returns the zoom level of a given Bing tile.

geometry\_to\_bing\_tiles(_geometry_, _zoom\_level_)[#](https://trino.io/docs/current/functions/geospatial.html#geometry\_to\_bing\_tiles)

Returns the minimum set of Bing tiles that fully covers a given geometry at a given zoom level. Zoom levels from 1 to 23 are supported.

### Encoded polylines[#](https://trino.io/docs/current/functions/geospatial.html#encoded-polylines) <a href="#encoded-polylines" id="encoded-polylines"></a>

These functions convert between geometries and [encoded polylines](https://developers.google.com/maps/documentation/utilities/polylinealgorithm).

to\_encoded\_polyline(_Geometry_) → varchar[#](https://trino.io/docs/current/functions/geospatial.html#to\_encoded\_polyline)

Encodes a linestring or multipoint to a polyline.

from\_encoded\_polyline(_varchar_) → Geometry[#](https://trino.io/docs/current/functions/geospatial.html#from\_encoded\_polyline)

Decodes a polyline to a linestring.
