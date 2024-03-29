A union is a struct in which all members are allocated at the same address so only as much memory as the largest member is occupied. 

A union can hold the value for only one member at a time. 

Here is an example from https://learn.microsoft.com/en-us/cpp/cpp/unions?view=msvc-170

```c++
// declaring_a_union.cpp
union RecordType    // Declare a simple union type
{
    char   ch;
    int    i;
    long   l;
    float  f;
    double d;
    int *int_ptr;
};

int main()
{
    RecordType t;
    t.i = 5; // t holds an int
    t.f = 7.25; // t now holds a float
}
```

Code that accesses the union needs to know which member holds the data. The most common solution is called a _discriminated union_. This encloses the union in a struct, with an enum member indicating the member type currently stored in the union;

```c++
#include <queue>

using namespace std;

enum class WeatherDataType
{
    Temperature, Wind
};

struct TempData
{
    int StationId;
    time_t time;
    double current;
    double max;
    double min;
};

struct WindData
{
    int StationId;
    time_t time;
    int speed;
    short direction;
};

struct Input
{
    WeatherDataType type;
    union
    {
        TempData temp;
        WindData wind;
    };
};

// Functions that are specific to data types
void Process_Temp(TempData t) {}
void Process_Wind(WindData w) {}

void Initialize(std::queue<Input>& inputs)
{
    Input first;
    first.type = WeatherDataType::Temperature;
    first.temp = { 101, 1418855664, 91.8, 108.5, 67.2 };
    inputs.push(first);

    Input second;
    second.type = WeatherDataType::Wind;
    second.wind = { 204, 1418859354, 14, 27 };
    inputs.push(second);
}

int main(int argc, char* argv[])
{
    // Container for all the data records
    queue<Input> inputs;
    Initialize(inputs);
    while (!inputs.empty())
    {
        Input const i = inputs.front();
        switch (i.type)
        {
            case WeatherDataType::Temperature:
                Process_Temp(i.temp);
                break;
            case WeatherDataType::Wind:
                Process_Wind(i.wind);
                break;
            default:
                break;
        }
        inputs.pop();

    }
    return 0;
}
```

To explain this for my own brain, we have an `enum` outlining `temperature` and `wind`, the corresponding structs for them, and then a union in which we have our `enum`, telling us which type of value the union currently holds.

A solution to the problem of assigning `WeatherDataType` every time we set a value, we can encapsulate the union in a type field, in a class, and offer access only through member functions that use the union correctly (setting `WeatherDataType` for us). The use of `naked unions` is best minimised. 

Use of `variant` can help us eliminate most direct uses of a union. A variant stores a value of one of a set of alternative types. For example, a `variant<Node*, int>` can only either a `Node*` or an `int`. We could rewrite `Input` above like this;

```c++
struct Input {
        variant<TempData, WindData> v;
};

void f(Input* input)
{
        if (holds_alternative<TempData>(input->v))   // does *input hold TempData? (see §15.4.1)
                cout << get<TempData>(input->v);         // get the value
        // ...
}
```

