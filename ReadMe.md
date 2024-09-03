# Go Learning Journey

## Plan

- Focus: Go Tour (https://go.dev/tour/welcome/1)
- Time: 40 minutes daily
- Phases: 1) Basics, 2) Projects

## Progress

### Go Tour Sections

- [X] Basics-08-19
- [X] Basics-08-22
- [X] Basics-08-23
- [X] Basics-08-26
- [X] Basics-08-27
- [X] Basics-08-29
- [X] Basics-08-30
- [X] Basics-09-01
- [X] Basics-09-03

# Summary of Key Go Language Concepts

## 1. Stringer Interface

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

## 2. Type Assertions and Type Switches

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

## 3. Error Handling

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

### Projects

(To be added)

## Resources

- [Go Tour](https://go.dev/tour/welcome/1)

## Goals

- Complete Go Tour
- Build small projects

## Log

---

This README will be updated as I progress through my Go learning journey.
