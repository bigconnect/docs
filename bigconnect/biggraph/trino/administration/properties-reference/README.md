# Properties reference

This section describes the most important configuration properties and (where applicable) their corresponding session properties, that may be used to tune Trino or alter its behavior when required. Unless specified otherwise, configuration properties must be set on the coordinator and all worker nodes.

The following pages are not a complete list of all configuration and session properties available in Trino, and do not include any connector-specific catalog configuration properties. For more information on catalog configuration properties, refer to the connector documentation.

* General
* Resource management
* Query management
* Catalog management
* SQL environment
* Spilling
* Exchange
* Task
* Write partitioning
* Writer scaling
* Node scheduler
* Optimizer
* Logging
* Web UI
* Regular expression function
* HTTP client

### Property value types <a href="#property-value-types" id="property-value-types"></a>

Trino configuration properties support different value types with their own allowed values and syntax. Additional limitations apply on a per-property basis, and disallowed values result in a validation error.

#### `boolean` <a href="#boolean" id="boolean"></a>

The properties of type `boolean` support two values, `true` or `false`.

#### `data size` <a href="#data-size" id="data-size"></a>

The properties of type `data size` support values that describe an amount of data, measured in byte-based units. These units are incremented in multiples of 1024, so one megabyte is 1024 kilobytes, one kilobyte is 1024 bytes, and so on. For example, the value `6GB` describes six gigabytes, which is (6 \* 1024 \* 1024 \* 1024) = 6442450944 bytes.

The `data size` type supports the following units:

* `B`: Bytes
* `kB`: Kilobytes
* `MB`: Megabytes
* `GB`: Gigabytes
* `TB`: Terabytes
* `PB`: Petabytes

#### `double` <a href="#double" id="double"></a>

The properties of type `double` support numerical values including decimals, such as `1.6`. `double` type values can be negative, if supported by the specific property.

#### `duration` <a href="#duration" id="duration"></a>

The properties of type `duration` support values describing an amount of time, using the syntax of a non-negative number followed by a time unit. For example, the value `7m` describes seven minutes.

The `duration` type supports the following units:

* `ns`: Nanoseconds
* `us`: Microseconds
* `ms`: Milliseconds
* `s`: Seconds
* `m`: Minutes
* `h`: Hours
* `d`: Days

A duration of `0` is treated as zero regardless of the unit that follows. For example, `0s` and `0m` both mean the same thing.

Properties of type `duration` also support decimal values, such as `2.25d`. These are handled as a fractional value of the specified unit. For example, the value `1.5m` equals one and a half minutes, or 90 seconds.

#### `integer` <a href="#integer" id="integer"></a>

The properties of type `integer` support whole numeric values, such as `5` and `1000`. Negative values are supported as well, for example `-7`. `integer` type values must be whole numbers, decimal values such as `2.5` are not supported.

Some `integer` type properties enforce their own minimum and maximum values.

#### `string` <a href="#string" id="string"></a>

The properties of type `string` support a set of values that consist of a sequence of characters. Allowed values are defined on a property-by-property basis, refer to the specific property for its supported and default values.
