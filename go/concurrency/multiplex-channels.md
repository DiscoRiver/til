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