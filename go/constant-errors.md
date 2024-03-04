Sentinal errors are exported errors that can be used by consumers of a package. Therefore, they need to be maintained as applications may rely on them being present. 

According to Dave Cheney (https://dave.cheney.net/2016/04/07/constant-errors), sentinal errors are generally bad due to the source and runtime coupling they create. 

There are some exceptions, such as `io.EOF`.

Errors are not constants, they behaviour more like a singleton, which presents weird behaviour...

```
err := errors.New("EOF")   // io/io.go line 38
fmt.Println(io.EOF == err) // false
```

This can cause problems that are frustrating to debug.

Dave suggests that we shouldn't be using sentinal errors, and to assert errors for behaviour, not type. 

More info in this article: https://dave.cheney.net/2014/12/24/inspecting-errors