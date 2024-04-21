Source: https://pkg.go.dev/errors

Something I hadn't looked at until recently is error trees, that is to say, I didn't know that an `error` can wrap multiple errors. 

Let's examine a simple piece of code;

```go
type sampleError struct {
}

func (e sampleError) Error() string {
	return "this is a sample error"
}

func main() {
	var s sampleError
	_, err := badFunction()
	if errors.Is(err, s) { // If using errors.Is, do not use a pointer as it will be a pointer type
		fmt.Println("error is of correct type")
	} else {
		fmt.Println("error is NOT of correct type")
	}
}

func badFunction() (int, error) {
	return 0, sampleError{}
}
```

Here, we are creating a custom error `sampleError`, and using `errors.Is()` to ensure it is of the correct type.

What happened if `badFunction` calls another function that returns `sampleError`, and we customise it for logging purposes? Well, we can wrap error using `fmt.Errorf` like this;

```go
type sampleError struct {
}

func (e sampleError) Error() string {
	return "this is a sample error"
}

func main() {
	var s sampleError
	err := badFunction()
	if errors.Is(err, s) {
		fmt.Println("error is of correct type")
	} else {
		fmt.Println("error is NOT of correct type")
	}
}

func badFunction() error {
	err := badFunction2()
	if err != nil {
		return fmt.Errorf("this is a custom error message: %w", err)
	}
	return nil
}

func badFunction2() error {
	return sampleError{}
}
```

Note the `%w` format directive, which is how we wrap the error. This allows us to provide custom messaging and error formats, while still accessing the underlying error types. 

Further, we can wrap multiple errors and detect them in the tree, like this;

```go
type sampleError struct {
}

func (e sampleError) Error() string {
	return "this is a sample error"
}

type sampleError2 struct {
}

func (e sampleError2) Error() string {
	return "this is a different sample error"
}

func main() {
	var s sampleError
	var s2 sampleError2

	err := badFunction()
	if errors.Is(err, s) && errors.Is(err, s2) { // If using errors.Is, do not use a pointer as it will be a pointer type
		fmt.Println("error is of correct types")
	} else {
		fmt.Println("error is NOT of correct type")
	}
}

func badFunction() error {
	err := badFunction2()
	if err != nil {
		return errors.Join(sampleError{}, err) // This how we wrap errors.
	}
	return nil
}

func badFunction2() error {
	return sampleError2{}
}
```

This allows us to use `errors.Is` to identify that an error of the specified type exists in the tree. If we need to capture that error, we can use `errors.As` with a pointer value such as `errors.As(err, &s)` to assign the first errors of type `sampleError2` to `s`. 

It's typically not preferable to use the following syntax;

```go
if err, ok := badFunction().(sampleError); ok {
    // do something
}
```

Because this doesn't indicate if `err` _wraps_ `sampleError`, which is important.

There is a lot more nuance to error handling, and you should look at this article: https://go.dev/doc/effective_go#errors


