# Operators

The following table lists the operators that you can use:

| Category    | Description                                                                                                                                                                                                                                                                                       |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Arithmetic  | <ul><li>+</li><li>-</li><li>*</li><li>/ or "div"</li><li>% or "mod"</li></ul>                                                                                                                                                                                                                     |
| Logical     | <ul><li>and</li><li>&#x26;&#x26;</li><li>or</li><li>||</li><li>not</li><li>!</li></ul>                                                                                                                                                                                                            |
| Relational  | <p>You can use the following operators to compare against other values, or against boolean, string, integer, or floating point literals.</p><ul><li>==</li><li>eq</li><li>!=</li><li>ne</li><li>&#x3C;</li><li>lt</li><li>></li><li>gt</li><li>&#x3C;=</li><li>ge</li><li>>=</li><li>le</li></ul> |
| Empty       | <p><code>empty</code></p><p>The empty operator is a prefix operation that can be used to determine whether a value is null or empty.</p>                                                                                                                                                          |
| Conditional | <p>?</p><p>For example, <code>A ? B : C</code> states that if A is true, then B. If A is not true, then C.</p>                                                                                                                                                                                    |

### Operator Precedence <a href="#ariaid-title2" id="ariaid-title2"></a>

The precedence of operators highest to lowest, left to right is as follows:

* \[ ]
* ( ) - Used to change the precedence of operators
* \- (unary) `not ! empty`
* \* / div % mod
* \+ - (binary)
* < > <= >= lt gt le ge
* \== != eq ne
* && and
* || or
* ? :
