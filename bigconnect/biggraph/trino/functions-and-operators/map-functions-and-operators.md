# Map functions and operators

### Subscript operator: \[][#](https://trino.io/docs/current/functions/map.html#subscript-operator) <a href="#subscript-operator" id="subscript-operator"></a>

The `[]` operator is used to retrieve the value corresponding to a given key from a map. This operator throws an error if the key is not contained in the map. See also `element_at` function that returns `NULL` in such case.

```
SELECT name_to_age_map['Bob'] AS bob_age;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Map functions[#](https://trino.io/docs/current/functions/map.html#map-functions) <a href="#map-functions" id="map-functions"></a>

cardinality(_x_) → bigint

Returns the cardinality (size) of the map `x`.

element\_at(_map(K_, _V)_, _key_) → V

Returns value for given `key`, or `NULL` if the key is not contained in the map.

map() → map\<unknown, unknown>[#](https://trino.io/docs/current/functions/map.html#map)

Returns an empty map.

```
SELECT map();
-- {}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)map(_array(K)_, _array(V)) -> map(K_, _V_)

Returns a map created using the given key/value arrays.

```
SELECT map(ARRAY[1,3], ARRAY[2,4]);
-- {1 -> 2, 3 -> 4}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

See also [`map_agg()`](https://trino.io/docs/current/functions/aggregate.html#map\_agg) and [`multimap_agg()`](https://trino.io/docs/current/functions/aggregate.html#multimap\_agg) for creating a map as an aggregation.

map\_from\_entries(_array(row(K_, _V))) -> map(K_, _V_)[#](https://trino.io/docs/current/functions/map.html#map\_from\_entries)

Returns a map created from the given array of entries.

```
SELECT map_from_entries(ARRAY[(1, 'x'), (2, 'y')]);
-- {1 -> 'x', 2 -> 'y'}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)multimap\_from\_entries(_array(row(K_, _V))) -> map(K_, _array(V)_)[#](https://trino.io/docs/current/functions/map.html#multimap\_from\_entries)

Returns a multimap created from the given array of entries. Each key can be associated with multiple values.

```
SELECT multimap_from_entries(ARRAY[(1, 'x'), (2, 'y'), (1, 'z')]);
-- {1 -> ['x', 'z'], 2 -> ['y']}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)map\_entries(_map(K_, _V)) -> array(row(K_, _V)_)[#](https://trino.io/docs/current/functions/map.html#map\_entries)

Returns an array of all entries in the given map.

```
SELECT map_entries(MAP(ARRAY[1, 2], ARRAY['x', 'y']));
-- [ROW(1, 'x'), ROW(2, 'y')]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)map\_concat(_map1(K_, _V)_, _map2(K_, _V)_, _..._, _mapN(K_, _V)) -> map(K_, _V_)[#](https://trino.io/docs/current/functions/map.html#map\_concat)

Returns the union of all the given maps. If a key is found in multiple given maps, that key’s value in the resulting map comes from the last one of those maps.

map\_filter(_map(K_, _V)_, _function(K_, _V_, _boolean)) -> map(K_, _V_)[#](https://trino.io/docs/current/functions/map.html#map\_filter)

Constructs a map from those entries of `map` for which `function` returns true:

```
SELECT map_filter(MAP(ARRAY[], ARRAY[]), (k, v) -> true);
-- {}

SELECT map_filter(MAP(ARRAY[10, 20, 30], ARRAY['a', NULL, 'c']),
                  (k, v) -> v IS NOT NULL);
-- {10 -> a, 30 -> c}

SELECT map_filter(MAP(ARRAY['k1', 'k2', 'k3'], ARRAY[20, 3, 15]),
                  (k, v) -> v > 10);
-- {k1 -> 20, k3 -> 15}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)map\_keys(_x(K_, _V)) -> array(K_)[#](https://trino.io/docs/current/functions/map.html#map\_keys)

Returns all the keys in the map `x`.

map\_values(_x(K_, _V)) -> array(V_)[#](https://trino.io/docs/current/functions/map.html#map\_values)

Returns all the values in the map `x`.

map\_zip\_with(_map(K_, _V1)_, _map(K_, _V2)_, _function(K_, _V1_, _V2_, _V3)) -> map(K_, _V3_)[#](https://trino.io/docs/current/functions/map.html#map\_zip\_with)

Merges the two given maps into a single map by applying `function` to the pair of values with the same key. For keys only presented in one map, NULL will be passed as the value for the missing key.

```
SELECT map_zip_with(MAP(ARRAY[1, 2, 3], ARRAY['a', 'b', 'c']),
                    MAP(ARRAY[1, 2, 3], ARRAY['d', 'e', 'f']),
                    (k, v1, v2) -> concat(v1, v2));
-- {1 -> ad, 2 -> be, 3 -> cf}

SELECT map_zip_with(MAP(ARRAY['k1', 'k2'], ARRAY[1, 2]),
                    MAP(ARRAY['k2', 'k3'], ARRAY[4, 9]),
                    (k, v1, v2) -> (v1, v2));
-- {k1 -> ROW(1, null), k2 -> ROW(2, 4), k3 -> ROW(null, 9)}

SELECT map_zip_with(MAP(ARRAY['a', 'b', 'c'], ARRAY[1, 8, 27]),
                    MAP(ARRAY['a', 'b', 'c'], ARRAY[1, 2, 3]),
                    (k, v1, v2) -> k || CAST(v1 / v2 AS VARCHAR));
-- {a -> a1, b -> b4, c -> c9}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)transform\_keys(_map(K1_, _V)_, _function(K1_, _V_, _K2)) -> map(K2_, _V_)[#](https://trino.io/docs/current/functions/map.html#transform\_keys)

Returns a map that applies `function` to each entry of `map` and transforms the keys:

```
SELECT transform_keys(MAP(ARRAY[], ARRAY[]), (k, v) -> k + 1);
-- {}

SELECT transform_keys(MAP(ARRAY [1, 2, 3], ARRAY ['a', 'b', 'c']),
                      (k, v) -> k + 1);
-- {2 -> a, 3 -> b, 4 -> c}

SELECT transform_keys(MAP(ARRAY ['a', 'b', 'c'], ARRAY [1, 2, 3]),
                      (k, v) -> v * v);
-- {1 -> 1, 4 -> 2, 9 -> 3}

SELECT transform_keys(MAP(ARRAY ['a', 'b'], ARRAY [1, 2]),
                      (k, v) -> k || CAST(v as VARCHAR));
-- {a1 -> 1, b2 -> 2}

SELECT transform_keys(MAP(ARRAY [1, 2], ARRAY [1.0, 1.4]),
                      (k, v) -> MAP(ARRAY[1, 2], ARRAY['one', 'two'])[k]);
-- {one -> 1.0, two -> 1.4}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)transform\_values(_map(K_, _V1)_, _function(K_, _V1_, _V2)) -> map(K_, _V2_)[#](https://trino.io/docs/current/functions/map.html#transform\_values)

Returns a map that applies `function` to each entry of `map` and transforms the values:

```
SELECT transform_values(MAP(ARRAY[], ARRAY[]), (k, v) -> v + 1);
-- {}

SELECT transform_values(MAP(ARRAY [1, 2, 3], ARRAY [10, 20, 30]),
                        (k, v) -> v + k);
-- {1 -> 11, 2 -> 22, 3 -> 33}

SELECT transform_values(MAP(ARRAY [1, 2, 3], ARRAY ['a', 'b', 'c']),
                        (k, v) -> k * k);
-- {1 -> 1, 2 -> 4, 3 -> 9}

SELECT transform_values(MAP(ARRAY ['a', 'b'], ARRAY [1, 2]),
                        (k, v) -> k || CAST(v as VARCHAR));
-- {a -> a1, b -> b2}

SELECT transform_values(MAP(ARRAY [1, 2], ARRAY [1.0, 1.4]),
                        (k, v) -> MAP(ARRAY[1, 2], ARRAY['one', 'two'])[k]
                          || '_' || CAST(v AS VARCHAR));
-- {1 -> one_1.0, 2 -> two_1.4}
```
