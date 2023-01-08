Struct methods are a great way to expand functionality of simple tasks, like incrementing a value and tracking history of the value.

Here we're tracking the maximum and minimum value(s) assigned to this struct, and ensuring it's accuracy with a mutex.

```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
)

type someNumber struct {
	value int

	max int
	min int

	initialised bool

	mu sync.Mutex
}

func (num *someNumber) UpdateValue(newNum int) {
	num.mu.Lock()
	defer num.mu.Unlock()

	defer func() { // only for logging, can be removed
		fmt.Println("Val: ", num.value)
		fmt.Println("Max: ", num.max)
		fmt.Println("Min: ", num.min)
	}()

	num.value = newNum

	// If it's the first update, set both min and max to the new value
	if !num.initialised {
		num.min = num.value
		num.max = num.value
		num.initialised = true
		return
	}

	if num.value > num.max {
		num.max = num.value
	}
	if num.value < num.min {
		num.min = num.value
	}
}

func main() {
	var someNum someNumber
	var wg sync.WaitGroup

	for i := 0; i < 5; i++ {
		wg.Add(1)
		go func() {
			someNum.UpdateValue(rand.Int())
			wg.Done()
		}()
	}

	wg.Wait()
}
```