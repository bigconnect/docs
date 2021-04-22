# Decimal

### Decimal Literals <a id="decimal-literals"></a>

Use the `DECIMAL 'xxxxxxx.yyyyyyy'` syntax to define a decimal literal.

The precision of a decimal type for a literal will be equal to the number of digits in the literal \(including trailing and leading zeros\). The scale will be equal to the number of digits in the fractional part \(including trailing zeros\).

| Example literal | Data type |
| :--- | :--- |
| `DECIMAL '0'` | `DECIMAL(1)` |
| `DECIMAL '12345'` | `DECIMAL(5)` |
| `DECIMAL '0000012345.1234500000'` | `DECIMAL(20, 10)` |

### Binary Arithmetic Decimal Operators <a id="binary-arithmetic-decimal-operators"></a>

Standard mathematical operators are supported. The table below explains precision and scale calculation rules for result. Assuming `x` is of type `DECIMAL(xp, xs)` and `y` is of type `DECIMAL(yp, ys)`.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Operation</th>
      <th style="text-align:left">Result type precision</th>
      <th style="text-align:left">Result type scale</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><code>x + y</code>
        </p>
        <p>and</p>
        <p><code>x - y</code>
        </p>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left"><code>max(xs, ys)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>x * y</code>
      </td>
      <td style="text-align:left"><code>min(38, xp + yp)</code>
      </td>
      <td style="text-align:left"><code>xs + ys</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>x / y</code>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left"><code>max(xs, ys)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>x % y</code>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left"><code>max(xs, ys)</code>
      </td>
    </tr>
  </tbody>
</table>

If the mathematical result of the operation is not exactly representable with the precision and scale of the result data type, then an exception condition is raised: `Value is out of range`.

When operating on decimal types with different scale and precision, the values are first coerced to a common super type. For types near the largest representable precision \(38\), this can result in Value is out of range errors when one of the operands doesn’t fit in the common super type. For example, the common super type of decimal\(38, 0\) and decimal\(38, 1\) is decimal\(38, 1\), but certain values that fit in decimal\(38, 0\) cannot be represented as a decimal\(38, 1\).

### Comparison Operators <a id="comparison-operators"></a>

All standard [Comparison Functions and Operators](trino-comparison-fx.md) work for the decimal type.

### Unary Decimal Operators <a id="unary-decimal-operators"></a>

The `-` operator performs negation. The type of result is same as type of argument.

