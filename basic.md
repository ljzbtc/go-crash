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
The `range` form of the `for` loop iterates over a slice or map.
When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.
## 18. Range continued
You can skip the index or value by assigning to `_`.
```
for i, _ := range pow
for _, value := range pow
```
If you only want the index, you can omit the second variable.
```
for i := range pow
```
## 19. Pic Function Example
```go
package main

import (
	"golang.org/x/tour/pic"
)

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

func main() {
	pic.Show(Pic)
}
```

This example demonstrates the use of slices, range loops, and function passing in Go, creating a simple image generation function.