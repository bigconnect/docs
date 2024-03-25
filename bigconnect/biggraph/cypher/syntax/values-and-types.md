# Values and types

Cypher® provides first class support for a number of data types.

These fall into several categories which will be described in detail in the following subsections:

* Property types
* Structural types
* Composite types

### Property types

* &#x20;Can be returned from Cypher queries
* &#x20;Can be used as parameters
* &#x20;Can be stored as properties
* &#x20;Can be constructed with Cypher literals

Property types comprise:

* Number, an abstract type, which has the subtypes _Integer_ and _Float_
* String
* Boolean
* The spatial type _Point_
* Temporal types: _Date_, _Time_, _LocalTime_, _DateTime_, _LocalDateTime_ and _Duration_

The adjective _numeric_, when used in the context of describing Cypher functions or expressions, indicates that any type of Number applies (Integer or Float).

Homogeneous lists of simple types can also be stored as properties, although lists in general (see Composite types) cannot be stored.

Cypher also provides pass-through support for byte arrays, which can be stored as property values. Byte arrays are _not_ considered a first class data type by Cypher, so do not have a literal representation.

| <p>Sorting of special characters</p><p>Strings that contain characters that do not belong to the <a href="https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane"><em>Basic Multilingual Plane</em></a> (<em>BMP</em>) can have inconsistent or non-deterministic ordering in Neo4j. BMP is a subset of all characters defined in Unicode. Expressed simply, it contains all common characters from all common languages.</p><p>The most significant characters <em>not</em> in BMP are those belonging to the <a href="https://en.wikipedia.org/wiki/Plane_(Unicode)#Supplementary_Multilingual_Plane"><em>Supplementary Multilingual Plane</em></a> or the <a href="https://en.wikipedia.org/wiki/Plane_(Unicode)#Supplementary_Ideographic_Plane"><em>Supplementary Ideographic Plane</em></a>. Examples are:</p><ul><li>Historic scripts and symbols and notation used within certain fields such as: Egyptian hieroglyphs, modern musical notation, mathematical alphanumerics.</li><li>Emojis and other pictographic sets.</li><li>Game symbols for playing cards, Mah Jongg, and dominoes.</li><li>CJK Ideograph that were not included in earlier character encoding standards.</li></ul> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Structural types

* &#x20;Can be returned from Cypher queries
* &#x20;Cannot be used as parameters
* &#x20;Cannot be stored as properties
* &#x20;Cannot be constructed with Cypher literals

Structural types comprise:

* Nodes, comprising:
  * Id
  * Label(s)
  * Map (of properties)
* Relationships, comprising:
  * Id
  * Type
  * Map (of properties)
  * Id of the start and end nodes
* Paths
  * An alternating sequence of nodes and relationships

### Composite types

* &#x20;Can be returned from Cypher queries
* &#x20;Can be used as parameters
* &#x20;Cannot be stored as properties
* &#x20;Can be constructed with Cypher literals

Composite types comprise:

* **Lists** are heterogeneous, ordered collections of values, each of which has any property, structural or composite type.
* **Maps** are heterogeneous, unordered collections of (key, value) pairs, where:
  * the key is a String
  * the value has any property, structural or composite type

| Composite values can also contain `null`. |
| ----------------------------------------- |

***

Special care must be taken when using `null` (see Working with `null`).
