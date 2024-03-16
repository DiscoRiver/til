A struct is a user-defined type consisting of a number of elements,

```
struct Vector {
       double* elem;  // pointer to elements
       int sz;        // number of elements
};
```

A variable of type `Vector` can be defined like this;

```
Vector v;
```

Now, `v` contains no elements, so let's give it something to point to;

```
void vector_init(Vector& v, int s)    // initialize a Vector
{
        v.elem = new double[s];  // allocate an array of s doubles
        v.sz = s;
}
```

Here, we're passing the `Vector` as `Vector&` which indicates that we pass by reference, meaning we can modify the passed `Vector`, and not a copy of it that is bound to the scope of the function. 

Accessing struct members can be done in several ways;

```
void f(Vector v, Vector& rv, Vector* pv)
{
        int i1 = v.sz;             // access through name
        int i2 = rv.sz;            // access through reference
        int i3 = pv->sz;         // access through pointer
}
```

