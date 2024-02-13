Idempotence is an operation that has doesn’t have a different outcome when provided the same input, no matter how many times it is executed.

```go
package main

import "fmt"

func main() {
	s := map[int]string{1: "1", 2: "2", 3: "3"}

	removeZeroKey := func(s map[int]string) { delete(s, 1) }

	// Given the same input, these two calls result in the same output, no matter how many times removeZeroKey() is executed.
	removeZeroKey(s)
	fmt.Println(s)

	removeZeroKey(s)
	fmt.Println(s)
}
```

Parameter mutating is an operation that modifies it’s input. Generally, it’s a good practice to reduce the amount of modification, for readability.