# Go Language Basic Concepts Notes

## 1. Packages and Exporting

- Names starting with a capital letter are exported
- Only exported names can be referenced from outside the package

## 2. Functions

- For consecutive parameters of the same type, the type can be omitted for all but the last parameter
- Can have named return values

## 3. Variables

- Declared using `var`, with the type at the end
- Can be declared at package or function level
- Can include initializers in the declaration, allowing type to be omitted
- Short variable declarations using `:=` within functions
- Outside functions, each statement must begin with a keyword

## 4. Basic Types

- Include bool, string, various int and uint, byte, rune, float, complex

## 5. Zero Values

- Variables declared without an initial value are given their zero value

## 6. Type Conversions

- Use `T(v)` syntax for explicit type conversion

## 7. Type Inference

- When using `:=` or `var =`, type can be inferred from the right-hand side value

## 8. Constants

- Declared using the `const` keyword
- Cannot be declared using `:=` syntax
- Numeric constants are high-precision values
- Untyped constants take the type needed by their context

## 9. Control Structures

### For Loops
- Go has only one looping construct: the for loop
- Basic syntax: `for initialization; condition; post { }`
- Initialization and post statements are optional
- Can simulate while loops: `for condition { }`

### If Statements
- Expression doesn't need parentheses, but braces are required
- Can execute a short statement before the condition
- Variables declared in the if statement are also available in any else blocks

### Switch Statements
- A concise way to write if-else chains
- Only runs the selected case, no break needed
- Cases don't need to be constants or integers
- Switch without a condition is the same as `switch true`

## 10. Defer

- A defer statement defers the execution of a function until the surrounding function returns
- Arguments to the deferred function are evaluated immediately
- Multiple defer statements are executed in last-in-first-out (LIFO) order

## 11. Additional Notes

- Go language emphasizes simplicity and practicality
- Many features (like defer) reflect Go's unique design philosophy
- Understanding these basic concepts is crucial for mastering Go programming