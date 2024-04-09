Multiplexing channels in Go is as simple as taking a number of channels, and passing them to the same channel for processing. Here is a fanIn func;

```go
func fanIn(chans ...<-chan string) <-chan string {
	c := make(chan string)
	for i := range chans {
		ch := chans[i]
		go func() {
			for {
				c <- <-ch
			}
		}()
	}
	return c
}
```

This can be alsod achieved by using `select`;

```go
func fanIn(input1, input2 <-chan string) <-chan string {
	c := make(chan string)
	go func() {
		for {
			select {
				case s := <-input1: c <- s
				case s := <-input2: c <- s
			}
		}
	}()
	return c
}
```