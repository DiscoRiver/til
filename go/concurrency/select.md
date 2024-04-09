The select statement is a control statement, like a switch, but for channels. Here is a simple example;

```go
select {
    case v1 := <- c1:
        fmt.Printf("received %v from c1\n", v1)
    case v2 := <- c2:
        fmt.Printf("received %v from c2\n", v2)
    case c3 <- 23:
        fmt.Printf("sent %v to c3\n", 23)      
    default:
        fmt.Printf("no one was ready to communicate\n")   
}
```

Communication blocks until one communication can proceed, which then does.

When multiple can proceed, select chooses one pseudo-randomly, meaning that you cannot rely on the ordering of these conditions.

The default clause executes when no other communication is ready, and is not required. If it isn't present, the select blocks forever until a communication is ready.