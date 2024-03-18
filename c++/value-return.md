Returning values is a universal process, but in C++ there's some interesting things to note. 

When returning small values, returning a copy is ideal, but we return "by reference" only when we want to grant the caller access to something that is not local to the function. 

```c++
class Vector {
public:
        // ...
        double& operator[](int i) { return elem[i]; }       // return reference to ith element
private:
        double* elem;          // elem points to an array of sz
        // ...
};
```

Here the `i`th element of a `Vector` exists independently of the call to the subscript operator, so we can return a reference to it.

However, a local variable disappeares when the function returns, so we shouldn't return a reference;

```c++
int& bad()
{
        int x;
        // ...
        return x;  // bad: return a reference to the local variable x
}
```

The C++ compiler should catch this error.

But, returning either value or reference for small types is efficient, how do we pass large amounts of data?

```c++
Matrix operator+(const Matrix& x, const Matrix& y)
{
        Matrix res;
        // ... for all res[i,j], res[i,j] = x[i,j]+y[i,j] ...
        return res;
}

Matrix m1, m2;
// ...
Matrix m3 = m1+m2;         // no copy
```

A `Matrix` maybe be large and expensive to copy. We give `Matrix` a move contructor that moves it out of `operator+`. Even if we don't define a move constructor, the compiler is often able to optimise away the copy, and construct the `Matrix` exactly where it is needed. This is called `copy elision`.

We should not do manual memory management;

```c++
Matrix* add(const Matrix& x, const Matrix& y)         // complicated and error-prone 20th century style
{
        Matrix* p = new Matrix;
        // ... for all *p[i,j], *p[i,j] = x[i,j]+y[i,j] ...
        return p;
}

Matrix m1, m2;
// ...
Matrix* m3 = add(m1,m2);        // just copy a pointer
// ...
delete m3;                                 // easily forgotten
```