
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
