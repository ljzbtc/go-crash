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

## 40. Readers (io.Reader interface)

The `io.Reader` interface represents the read end of a stream of data. Here's the interface definition:

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

## 41.Exercise: Infinite 'A' Reader

Implement a `Reader` type that emits an infinite stream of the ASCII character 'A'.

```go
package main

import "golang.org/x/tour/reader"

type MyReader struct{}

func (r MyReader) Read(p []byte) (int, error) {
    for i := range p {
        p[i] = 'A'
    }
    return len(p), nil
}

func main() {
    reader.Validate(MyReader{})
}
```

## 42. Exercise: rot13Reader

Implement a `rot13Reader` that implements `io.Reader` and reads from an `io.Reader`, modifying the stream by applying the rot13 substitution cipher to all alphabetical characters.

```go
package main

import (
    "io"
    "os"
    "strings"
)

type rot13Reader struct {
    r io.Reader
}

func (r rot13Reader) Read(p []byte) (n int, err error) {
    n, err = r.r.Read(p)
    for i := 0; i < n; i++ {
        if (p[i] >= 'A' && p[i] <= 'Z') || (p[i] >= 'a' && p[i] <= 'z') {
            // deal UpperCase
            if p[i] >= 'A' && p[i] <= 'Z' {
                p[i] = 'A' + (p[i]-'A'+13)%26
            }
            // deal lowerCase
            if p[i] >= 'a' && p[i] <= 'z' {
                p[i] = 'a' + (p[i]-'a'+13)%26
            }
        }
    }
    return
}

func main() {
    s := strings.NewReader("Lbh penpxrq gur pbqr!")
    r := rot13Reader{s}
    io.Copy(os.Stdout, &r)
}
```

## 43. Exercise: Custom Image Type

Implement a custom `Image` type that satisfies the `image.Image` interface.

```go
package main

import (
    "image"
    "image/color"
    "golang.org/x/tour/pic"
)

type Image struct {
    Width, Height int
}

func (img Image) ColorModel() color.Model {
    return color.RGBAModel
}

func (img Image) Bounds() image.Rectangle {
    return image.Rect(0, 0, img.Width, img.Height)
}

func (img Image) At(x, y int) color.Color {
    v := uint8((x + y) / 2)
    return color.RGBA{v, v, 255, 255}
}

func main() {
    m := Image{256, 256}
    pic.ShowImage(m)
}
```

## 44. Type Parameters

- Feature: Enables writing functions that work with multiple types.
- Provides type safety and code reuse.
- Example:
  ```go
  func Index[T comparable](s []T, x T) int {
      for i, v := range s {
          if v == x {
              return i
          }
      }
      return -1
  }
  ```

## 45. Generic Types

- Feature: Allows creation of flexible, reusable data structures.
- Enhances type safety while reducing code duplication.
- Example:
  ```go
  type List[T any] struct {
      Head *Node[T]
      Tail *Node[T]
  }
  ```

## 46. Goroutines

- Feature: Lightweight, concurrent threads of execution.
- Enables efficient parallel processing with low overhead.
- Example:
  ```go
  go func() {
      // This function runs concurrently
  }()
  ```

## 47. Channels

- Feature: Provides a way for goroutines to communicate and synchronize.
- Helps avoid race conditions and simplifies concurrent programming.
- Example:
  ```go
  ch := make(chan int)
  ch <- 42    // Send to channel
  value := <-ch  // Receive from channel
  ```

## 48. Buffered Channels

- Feature: Channels with a capacity to hold multiple values.
- Allows for asynchronous communication, reducing blocking.
- Example:
  ```go
  ch := make(chan int, 100)
  ```

## 49. Range and Close

- Feature: Iterate over channel values and signal when no more data will be sent.
- Simplifies channel usage and helps manage resources.
- Example:
  ```go
  for v := range ch {
      // Process v
  }
  close(ch)  // Close the channel
  ```

## 50. Select Statement

- Feature: Allows a goroutine to wait on multiple channel operations.
- Enables complex synchronization and communication patterns.
- Example:
  ```go
  select {
  case msg1 := <-ch1:
      // Use msg1
  case msg2 := <-ch2:
      // Use msg2
  }
  ```

## 51. Default Selection

- Feature: Provides a way to make non-blocking channel operations.
- Useful for polling and preventing deadlocks.
- Example:
  ```go
  select {
  case msg := <-ch:
      // Got a message
  default:
      // No message available
  }
  ```


## 52. Equivalent Binary Trees Exercise

- Implement `Walk` function: Traverse the tree, sending values to a channel.
- Implement `Same` function: Use `Walk` to determine if two trees store the same values.
- Key points: Recursive traversal, using channels, concurrent comparison.

## 53. sync.Mutex

- Purpose: Ensure only one goroutine accesses a variable at a time.
- Methods: `Lock()` and `Unlock()`
- Usage: Surround critical sections with `Lock()` and `Unlock()`
- Tip: Use `defer` to ensure unlocking.

## 54. Concurrent Web Crawler Exercise

Objective: Modify `Crawl` function to fetch URLs in parallel without duplicates.

Key implementation points:

```go
type SafeCache struct {
    mu    sync.Mutex
    cache map[string]bool
}

func Crawl(url string, depth int, fetcher Fetcher, cache *SafeCache, wg *sync.WaitGroup) {
    defer wg.Done()
    if depth <= 0 || cache.Check(url) {
        return
    }
    // Fetch URL...
    for _, u := range urls {
        wg.Add(1)
        go Crawl(u, depth-1, fetcher, cache, wg)
    }
}

func main() {
    cache := &SafeCache{cache: make(map[string]bool)}
    var wg sync.WaitGroup
    wg.Add(1)
    go Crawl("https://golang.org/", 4, fetcher, cache, &wg)
    wg.Wait()
}
```

- Use `SafeCache` with `sync.Mutex` for thread-safe URL caching
- Implement concurrent crawling with goroutines
- Use `sync.WaitGroup` to wait for all goroutines to finish
