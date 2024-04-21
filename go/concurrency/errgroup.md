Package errgroup provides synchronization, error propagation, and Context cancelation for groups of goroutines working on subtasks of a common task.

This package can be used in place of `sync.WaitGroup` to add handling of tasks that might return errors. 

Source: https://pkg.go.dev/golang.org/x/sync/errgroup