# C++17 in Detail: A Deep Dive

## F: General Language Features
==================================================================

https://www.educative.io/courses/cpp-17-in-detail-a-deep-dive/introduction-RMLyJ51MGRK

### Structured Binding 

- Declarations
- Syntax
- Modifiers & Limitations
- Binding

### Init Statements for **if** and **switch**

- if(init, condition)
- switch (init, condition)

- **if** in a Search Statement

``` c++
int main() {
  const std::string myString = "My Hello World Wow";
  if (const auto pos = myString.find("World"); pos != std::string::npos)
    std::cout << pos << " World\n";
  else
    std::cout << pos << " not found!!\n";
}
```

### Inline Variables

### constexpr Lambda functions

```c++
int main () {
    constexpr auto SquareLambda = [] (int n) { return n*n; };
    static_assert(SquareLambda(3) == 9, "");
}
```

### Nested Namespaces

### __has_include Preprocessor Expression


## G: Standard Attributes
==================================================================

https://www.educative.io/courses/cpp-17-in-detail-a-deep-dive/introduction-7n1j4vW5oXr

### Attributes in C++11/C++14

- [[noreturn]]
- [[carries_dependency]]
- [[deprecated]]
- [[deprecated("reason")]]

### Additional Attributes C++17

- [[fallthrough]]
- [[maybe_unused]]
- [[nodiscard]]

### Summary Page of all Attributes available in c++17 plus descriptions
https://www.educative.io/courses/cpp-17-in-detail-a-deep-dive/summary-RMlDxYpMzAV


