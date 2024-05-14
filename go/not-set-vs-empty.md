A common practice I've come across is using pointers in struct fields to determine if a value was set (sent), because without, it'd be a bit difficult to determine if a zero value was set, or if the variable was just initialised. 

```
type some struct {
    i int
}
``

The above would give us zero for `i`, whereas the below would actually give us a nil pointer, meaning it was never set, rather than just being "empty".

```
type some struct {
    i *int
}

See here: https://stackoverflow.com/questions/59964619/difference-using-pointer-in-struct-fields

