# C++17 in Detail: A Deep Dive

## J: std::any
==================================================================

https://www.educative.io/courses/cpp-17-in-detail-a-deep-dive/introduction-mE7nl3YAqrR

###  Why use std::any?

With std::optional you can represent a regular Type values or mark it as empty. With std::variant you can wrap several type alternatives into one entity.

C++17 gives us one more wrapper type: std::any which can hold anything in a type-safe way.

``` c++
#include <string>
#include <iostream>
#include <any>
#include <map>
using namespace std;


int main()
{
    auto a = std::any(12);

    // set any value:
    a = std::string("Hello!");
    a = 16;
    // reading a value:

    // we can read it as int
    std::cout << std::any_cast<int>(a) << '\n';

    // but not as string:
    try
    {
        std::cout << std::any_cast<std::string>(a) << '\n';
    }
    catch(const std::bad_any_cast& e)
    {
        std::cout << e.what() << '\n';
    }

    // reset and check if it contains any value:
    a.reset();
    if (!a.has_value())
    {
        std::cout << "a is empty!" << '\n';
    }

    // you can use it in a container:
    std::map<std::string, std::any> m;
    m["integer"] = 10;
    m["string"] = std::string("Hello World");
    m["float"] = 1.0f;

    for (auto &[key, val] : m)
    {
        if (val.type() == typeid(int))
            std::cout << "int: " << std::any_cast<int>(val) << '\n';
        else if (val.type() == typeid(std::string))
            std::cout << "string: " << std::any_cast<std::string>(val) << '\n';
        else if (val.type() == typeid(float))
            std::cout << "float: " << std::any_cast<float>(val) << '\n';
    }
}
```

