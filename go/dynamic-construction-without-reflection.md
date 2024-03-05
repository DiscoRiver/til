This is an expanded explanation from my answer [here](https://stackoverflow.com/questions/78100054/using-polymorphism-vs-switch-in-golang/78101278#78101278).

Sometimes we're faced with the situation where we need to interpret an input, and create `<type>` based on it. Rightly so, the service providing that input doesn't (and shouldn't) care about our internal type structure, so we need to figure out how to interpret the input so we're executing the correct actions.

In Python, we can achieve this using the `getattr()` method to do some cool meta-programming, where we can use the value of string to get a matching object;

```python
import sys

def str_to_class(classname):
    return getattr(sys.modules[__name__], classname)
```

In Go, we don't have access to such a method, and there is no global registry of types we can readily access. So how do we achieve it?

We could build out own registry of types;

```go
var typeRegistry = make(map[string]reflect.Type)

func init() {
    myTypes := []interface{}{MyString{}}
    for _, v := range myTypes {
        // typeRegistry["MyString"] = reflect.TypeOf(MyString{})
        typeRegistry[fmt.Sprintf("%T", v)] = reflect.TypeOf(v)
    }
}

func makeInstance(name string) interface{} {
    v := reflect.New(typeRegistry[name]).Elem()
    // Maybe fill in fields here if necessary
    return v.Interface()
}
```

The problem with this is that compile-time errors now become run-time errors, aside from the fact that the code is very implicit and has poor readability in my opinion.  

Now, consider we have the following interface;

```go
type Fruit interface {
	GetVariety() string
}

type Apple struct {
	variety string
}

func NewApple(variety string) *Apple {
	return &Apple{variety: variety}
}

func (a *Apple) GetVariety() string {
	return a.variety
}

type Pear struct {
	variety string
}

func NewPear(variety string) *Pear {
	return &Pear{variety: variety}
}

func (p *Pear) GetVariety() string {
	return p.variety
}
```

What I want to do is take a message, perhaps something as simple as this;

```json
{
	fruit: "Apple",
	variety: "Golden Delicious"
}
```

And be able to construct the correct object (in this case Apple), which it turns out is very simple by linking the constructor funcs to a map key;

```go
var (
	fruits = map[string]func(string) Fruit{
		"Apple": func(s string) Fruit { return NewApple(s) },
		"Pear":  func(s string) Fruit { return NewPear(s) },
	}
)
```
Which we can then call in a few useful ways;

```go
someApple := fruits["Apple"]("Golden Delicious")
fruitVariety := someApple.GetVariety()

fmt.Println(fruitVariety)

// OR

fmt.Println(fruits["Apple"]("Golden Delicious").GetVariety())
```

Obviously we would want some error handling for `Fruit`s that don't exist, but that's another discussion. 