Function overloading is the simple idea of multiple functions that share the same name, but have different parameters.

In C++, for example, we might have this code;

```C++
void print(int i) {
    std::cout << i << "\n";
}

void print(double d) {
    std::cout << d << "\n";
}

void print(string s) {
    std::cout << s << "\n";
}
```

The compiler will determine which function is the most approrpiate based on it's arguments;

```C++
int main() {
    print(1); // uses print(int)
    print(1.1); // uses print(double)
    print("hello"); // uses print(string)
}
```