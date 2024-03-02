In this context, the execution order is variable declaration, init(), then main(). If we populate a global variable, we can run a function before init() is called;

```go
var WhatIsThe = AnswerToLife()

func AnswerToLife() int { // 1
    return 42
}

func init() { // 2
    WhatIsThe = 0
}

func main() { // 3
    if WhatIsThe == 0 {
        fmt.Println("It's all a lie.")
    }
}
```

We can also expand on this by using an underscore (_) to simply run the func without any assignment;

```go
var _ = GenerateSomething()
```

This could be useful in cases where you need to pull environment data or state before init() can operate.