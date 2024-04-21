We can call methods using reflection like this;

```go
package main

import (
	"fmt"
	"reflect"
)

type Foo struct {
}

func (f *Foo) Hello() {
	fmt.Println("Hello")
}

func main() {
	// Without reflection
	f := Foo{}
	f.Hello()

	// WIth reflection
	fT := reflect.TypeOf(Foo{})
	fV := reflect.New(fT)

	m := fV.MethodByName("Hello")
	if m.IsValid() {
		m.Call(nil)
	}
}
```