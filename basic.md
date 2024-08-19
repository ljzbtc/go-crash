
# Basic

## 1. Exported Names

- Names starting with a capital letter are exported
- Only exported names can be referenced from outside the package

## 2. Functions

- For consecutive parameters of the same type, the type can be omitted for all but the last parameter

## 3. Variables

- Declared using `var`, with the type at the end
- Can be declared at package or function level
- Can have named return values
- Can include initializers in the declaration, allowing type to be omitted

## 4. Short Variable Declarations

- `:=` can be used for short variable declarations within functions
- Outside functions, each statement must begin with a keyword

## 5. Basic Types

- Include bool, string, various int and uint, byte, rune, float, complex

## 6. Zero Values

- Variables declared without an initial value are given their zero value

## 7. Type Conversions

- Use `T(v)` syntax for explicit type conversion

## 8. Type Inference

- When using `:=` or `var =`, type can be inferred from the right-hand side value

## 9. Constants

- Declared using the `const` keyword
- Cannot be declared using `:=` syntax

## 10. Numeric Constants

- High-precision values
- Untyped constants take the type needed by their context

## 11. For Loops

- Go has only one looping construct: the for loop
- Basic syntax: `for initialization; condition; post { }`
- Initialization and post statements are optional
- Can simulate while loops: `for condition { }`

This summary covers the basic concepts and syntactic features of Go, providing a concise overview for beginners.
