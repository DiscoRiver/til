Polymorphism is the ability for an object to take multiple forms. A real world example is providing a single interface for multiple types.

There are multiple types of Polomorphism;

## Subtype Polymorphism (Runtime)

This is the most common type of polymorphism. It's used for objects that a single interface (for example, Car), and multiple subtypes (for example Ford, Nissan etc.). All subtypes can be referenced individually by using the "Car" object.

The polymorphism happens at runtime, when the type is evaluated and methods are performed on it. 

## Parametric Polymorphism (Overloading)

This type of polymorphism provides a way to use one function to interact with multiple types. 

A good example is lists. You can process values in a list regardless of it's type, using the same function in the code. In Go, for example, although a slice, is simple with generics. This function will work with any slice, regardless of the type;

```go
func printValues[T any](s []T) {
    for i := range s {
        fmt.Println(s[i])
    }
}
```

## Ad-hoc Polymorphism (Compile-time)

Here, functions with the same name act differently for different types. For example, in Python;

```
3 + 4
"Foo" + "bar"

//Output
7
Foobar
```

## Coercion Polymorphism (Casting)

This is the action of direct transformation from one type to another. It happens when a type gets cast into another type. For example, in Python;

```
int(43.2) #Converts a double to an int
float(45) #Converts an int to a float
```
