We can further control our execution by implementing wait groups. A wait group allows us to postpone execution until all goroutines (those that are affected by the wait group) have completed.

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int) {
    fmt.Printf("Worker %d starting\n", id)

    time.Sleep(time.Second)
    fmt.Printf("Worker %d done\n", id)
}

func main() {

    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1)

        go func() {
            defer wg.Done() // The work does not need to be aware of the concurrency primitives we're using, so we wrap this in a closure.
            worker(i)
        }()
    }

    wg.Wait()

}
```

This approach has no straightforward way to propagate errors from workers, in this case consider using `errgroup`.
