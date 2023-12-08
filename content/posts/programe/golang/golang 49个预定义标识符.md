---
title: golang 49个预定义标识符
date: 2023-12-08T15:43:00
tags:
  - golang
categories:
  - Programe
---

 [Go's predeclared identifier](https://pkg.go.dev/builtin)

|   |   |   |
|---|---|---|
|Predeclared Identifiers|Type|Explanation|
|nil|Variable|空值// Type must be a pointer, channel, func, interface, map, or slice type|
|iota|Constant|计数器|
|false|Constant|布尔值 false|
|true|Constant|布尔值 true|  
|append(slice []Type, elems ...Type) []Type|function|Appends the elements to the slice.|   
|cap(v Type) int|function|Returns the capacity of the slice.|   
|clear[T ~[]Type \| ~map[Type]Type1](t T)|function|Clears the contents of a map or slice.|   
|close(c chan<- Type)|function|Closes the channel.|   
|complex(r, i FloatType) ComplexType|function|Creates a complex number with the given real and imaginary parts.|  
|copy(dst, src []Type) int|function|Copies elements from the source slice to the destination slice.|   
|delete(m map[Type]Type1, key Type)|function|Deletes the element with the given key from the map.|   
|imag(c ComplexType) FloatType|function|Returns the imaginary part of the complex number.|  
|len(v Type) int|function|Returns the length of the slice, string, or map.|  
|make(t Type, size ...IntegerType) Type|function|Creates a slice, map, or channel.|  
|max[T cmp.Ordered](x T, y ...T) T|function|Returns the maximum of the given elements.|  
|[min[T cmp.Ordered](x T, y ...T) T]|function|Returns the minimum of the given elements.|   
|new(Type) \*Type|function|Allocates memory for a new value of the given type and returns a pointer to it.|   
|panic(v any)|function|Stops the execution of the current goroutine and starts a panic.|   
|print(args ...Type)|function|Prints the given arguments to the standard output.|   
|println(args ...Type)|function|Prints the given arguments to the standard output, followed by a newline character.|   
|real(c ComplexType) FloatType|function|Returns the real part of the complex number.|   
|recover() any|function|Attempts to recover from a panic started by the current goroutine.|   
|ComplexType|type|Represents a complex number.|   
|FloatType|type|Represents a floating-point number.|   
|IntegerType|type|Represents an integer number.|   
|Type|type|Represents a type.|   
|Type1|type|Represents a type.|   
|any|type|Represents any type.|   
|bool|type|Represents a boolean value (true or false).|   
|byte|type|Represents an 8-bit unsigned integer.|   
|comparable|type|Represents a type that can be compared using the == and != operators.|   
|complex128|type|Represents a 128-bit complex number.|   
|complex64|type|Represents a 64-bit complex number.|   
|error|type|Represents an error value.|   
|float32|type|Represents a 32-bit floating-point number.|   
|float64|type|Represents a 64-bit floating-point number.|   
|int|type|Represents an integer number.|   
|int16|type|Represents a 16-bit signed integer.|   
|int32|type|Represents a 32-bit signed integer.|   
|int64|type|Represents a 64-bit signed integer.|   
|int8|type|Represents an 8-bit signed integer.|   
|rune|type|Represents a Unicode character.|   
|string|type|Represents a sequence of characters.|   
|uint|type|Represents an unsigned integer number.|   
|uint16|type|Represents a 16-bit unsigned integer.|   
|uint32|type|Represents a 32-bit unsigned integer.|   
|uint64|type|Represents a 64-bit unsigned integer.|   
|uint8|type|Represents an 8-bit unsigned integer.|   
|uintptr|type|Represents an unsigned integer that represents a memory address.|   