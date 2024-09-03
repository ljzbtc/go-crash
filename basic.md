# Go Language Basic Concepts

## 1. Packages and Exporting

- Capitalized names are exported
- Only exported names are accessible from outside the package

## 2. Functions

- Can have named return values
- Type can be omitted for consecutive parameters of the same type

## 3. Variables

- Declared using `var`, type at the end
- Short declaration `:=` within functions
- Example: `var x int` or `x := 5`

## 4. Basic Types

- bool, string, int, uint, byte, rune, float, complex

## 5. Zero Values

- Variables without initial value get their zero value

## 6. Type Conversions

- Syntax: `T(v)`

## 7. Constants

- Declared using `const`
- Cannot use `:=` syntax

## 8. Control Structures

- For loop: `for i := 0; i < 10; i++ { }`
- If with short statement: `if v := x; v < 10 { }`
- Switch: Cases don't need to be constants or integers

## 9. Defer

- Defers execution until surrounding function returns
- Multiple defers executed in LIFO order

## 10. Pointers

- `&` generates a pointer, `*` dereferences
- Example:
  ```go
  i := 42
  p := &i
  *p = 21
  ```

## 11. Structs

- Example:
  ```go
  type Vertex struct {
      X, Y int
  }
  v := Vertex{1, 2}
  ```

## 12. Arrays

- Fixed size
- Example: `var a [10]int`

## 13. Slices

- Dynamic size, flexible view into arrays
- Example: `a[low : high]`
- Length and capacity: `len(s)` and `cap(s)`

## 14. Creating slices with make

- Example: `a := make([]int, 5)`

## 15. Slices of slices

- Example:
  ```go
  board := [][]string{
      []string{"_", "_", "_"},
      []string{"_", "_", "_"},
      []string{"_", "_", "_"},
  }
  ```

## 16. Appending to a slice

- Use `append` function
- Example: `s = append(s, 2, 3, 4)`

## 17. Range

- The `range` form of the `for` loop iterates over a slice or map
- When ranging over a slice, two values are returned for each iteration
- First is the index, second is a copy of the element at that index

## 18. Range continued

- You can skip the index or value by assigning to `_`
- Examples:
  ```go
  for i, _ := range pow
  for _, value := range pow
  ```
- If you only want the index, you can omit the second variable:
  ```go
  for i := range pow
  ```

## 19. Pic Function Example

```go
func Pic(dx, dy int) [][]uint8 {
    picture := make([][]uint8, dx)
    for x := range picture {
        picture[x] = make([]uint8, dy)
        for y := range picture[x] {
            picture[x][y] = uint8(x/2 + y/3)
        }
    }
    return picture
}
```

## 20. Maps

- A map maps keys to values
- The zero value of a map is `nil`
- The `make` function returns a map of the given type, initialized and ready for use

## 21. Map literals

- Map literals are like struct literals, but the keys are required

## 22. Mutating Maps

- Insert or update: `m[key] = elem`
- Retrieve an element: `elem = m[key]`
- Delete an element: `delete(m, key)`
- Test that a key is present: `elem, ok = m[key]`

## 23. WordCount Function Example

```go
func WordCount(s string) map[string]int {
    wordMap := make(map[string]int)
    slice_index := 0
    elem := 0
    for index, value := range s {
        if value == ' ' {
            key := s[slice_index:index]
            key = strings.TrimSpace(key)
            elem, _ = wordMap[key]
            wordMap[key] = 1 + elem
            slice_index = index
        } else if index == len(s)-1 {
            key := s[slice_index+1 : index+1]
            elem, _ = wordMap[key]
            wordMap[key] = 1 + elem
        }
    }
    return wordMap
}
```

## 24. Function values

- Functions are values too
- They can be passed around just like other values
- Function values may be used as function arguments and return values

## 25. Exercise: Fibonacci closure

Implement a `fibonacci` function that returns a function (a closure) that returns successive fibonacci numbers.

## 26. Fibonacci closure example

```go
func fibonacci() func() int {
    a, b := 0, 1
    return func() int {
        ret := a
        a, b = b, a+b
        return ret
    }
}
```

## 27. Methods

- Go doesn't have classes, but you can define methods on types
- Methods have a special receiver argument between `func` keyword and method name
- Example:
  ```go
  type Vertex struct {
      X, Y float64
  }

  func (v Vertex) Abs() float64 {
      return math.Sqrt(v.X*v.X + v.Y*v.Y)
  }

  v := Vertex{3, 4}
  fmt.Println(v.Abs())  // Output: 5
  ```

## 28. Methods are Functions

- A method is just a function with a receiver argument
- Example:
  ```go
  func Abs(v Vertex) float64 {
      return math.Sqrt(v.X*v.X + v.Y*v.Y)
  }

  v := Vertex{3, 4}
  fmt.Println(Abs(v))  // Output: 5
  ```

## 29. Methods on Non-struct Types

- You can declare methods on non-struct types
- Can only declare methods on types defined in the same package
- Example:
  ```go
  type MyFloat float64

  func (f MyFloat) Abs() float64 {
      if f < 0 {
          return float64(-f)
      }
      return float64(f)
  }

  f := MyFloat(-math.Sqrt2)
  fmt.Println(f.Abs())  // Output: 1.4142135623730951
  ```

## 30. Pointer Receivers

- Methods with pointer receivers can modify the value the receiver points to
- Syntax: `func (v *Vertex) Scale(f float64)`
- Pointer receivers are more common than value receivers
- Example:
  ```go
  func (v *Vertex) Scale(f float64) {
      v.X = v.X * f
      v.Y = v.Y * f
  }

  v := Vertex{3, 4}
  v.Scale(10)
  fmt.Println(v)  // Output: {30 40}
  ```

## 31. Methods and Pointer Indirection

- Methods with pointer receivers take either a value or a pointer when called
- Go interprets `v.Scale(5)` as `(&v).Scale(5)` for convenience
- Example:
  ```go
  var v Vertex
  v.Scale(5)   // OK
  p := &v
  p.Scale(10)  // Also OK
  ```

## 32. Value vs Pointer Receiver

- Use pointer receiver to modify the value or avoid copying on each method call
- Generally, all methods on a given type should use either value or pointer receivers consistently
- Example:
  ```go
  type Vertex struct {
      X, Y float64
  }

  func (v *Vertex) Scale(f float64) {
      v.X = v.X * f
      v.Y = v.Y * f
  }

  func (v Vertex) Abs() float64 {
      return math.Sqrt(v.X*v.X + v.Y*v.Y)
  }

  v := Vertex{3, 4}
  v.Scale(2)
  fmt.Println(v.Abs())  // Output: 10
  ```

## 33. Interfaces

- An interface type is defined as a set of method signatures.
- A value of interface type can hold any value that implements those methods.

## 34. Implicit Interface Implementation

- A type implements an interface by implementing its methods.
- No explicit declaration of intent, no "implements" keyword.
- Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

## 35. Interface Values

- Under the hood, interface values can be thought of as a tuple of a value and a concrete type:
  ```
  (value, type)
  ```
- An interface value holds a value of a specific underlying concrete type.
- Calling a method on an interface value executes the method of the same name on its underlying type.

## 36. Interface Values with Nil Underlying Values

- If the concrete value inside the interface itself is nil, the method will be called with a nil receiver.
- In some languages this would trigger a null pointer exception, but in Go it is common to write methods that gracefully handle being called with a nil receiver.
- Note that an interface value that holds a nil concrete value is itself non-nil.


## 37. Stringer Interface

The Stringer interface is one of the most ubiquitous interfaces in Go, defined in the `fmt` package:

```go
type Stringer interface {
    String() string
}
```

- Purpose: Allows types to define their string representation.
- Implementation: Any type that implements the `String() string` method satisfies this interface.
- Application: The `fmt` package (and others) look for this interface when printing values.

### Example: Stringer Implementation for IP Address

```go
type IPAddr [4]byte

func (ip IPAddr) String() string {
    return fmt.Sprintf("%d.%d.%d.%d", ip[0], ip[1], ip[2], ip[3])
}
```

## 38. Type Assertions and Type Switches

### Type Assertion

Used to check if an interface value holds a specific type.

Syntax: `value, ok := interface.(Type)`

### Type Switch

Allows you to execute different code based on the concrete type of an interface value.

Syntax:

```go
switch v := i.(type) {
case T:
    // v has type T
case S:
    // v has type S
default:
    // no match; v has the same type as i
}
```

## 39. Error Handling

Go uses the `error` interface for handling error states:

```go
type error interface {
    Error() string
}
```

### Custom Error Types

You can create custom error types to provide more context:

```go
type ErrNegativeSqrt float64

func (e ErrNegativeSqrt) Error() string {
    return fmt.Sprintf("cannot Sqrt negative number: %v", float64(e))
}
```

### Returning Errors from Functions

Functions typically return a result and an error:

```go
func Sqrt(x float64) (float64, error) {
    if x < 0 {
        return 0, ErrNegativeSqrt(x)
    }
    // Calculate square root...
    return result, nil
}
```

## Important Notes

1. Using `fmt.Sprint(e)` inside an `Error()` method may cause infinite recursion. Use `fmt.Sprint(float64(e))` instead.
2. Error handling is a crucial part of Go programming. Always check and handle returned errors carefully.
3. Type assertions and switches provide powerful ways to handle interface values, but use them cautiously to avoid runtime errors.
4. Implementing the Stringer interface can greatly improve the readability and debuggability of your types.
5. Custom error types allow you to include more context in your errors, which is valuable for debugging and error handling.
