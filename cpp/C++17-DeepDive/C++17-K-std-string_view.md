# C++17 in Detail: A Deep Dive

## K: std::string_view
==================================================================

https://www.educative.io/courses/cpp-17-in-detail-a-deep-dive/introduction-B810Dv70Avk

###  Why use std::string_view?

How many string copies are created in the below example?

``` c++
  // string function:
std::string StartFromWordStr(const std::string& strArg, const std::string& word) {
  return strArg.substr(strArg.find(word)); // substr creates a new string 
}

int main() {

  // call:
  std::string str {"Hello Amazing Programming Environment" }; 
  
  auto subStr = StartFromWordStr(str, "Programming Environment");
  std::cout << subStr << '\n';
}
```

Can you count them all?

The answer is 3 or 5 depending on the compiler, but usually, it should be 3.

- The first one is for <span style="color: red;">str</span>.
- The second one is for the second argument in <span style="color: red;">StartFromWordStr</span> - the argument is <span style="color: red;">const string&</span> so since we pass <span style="color: red;">const char*</span> it will create a new string.
- The third one comes from <span style="color: red;">substr</span> which returns a new <span style="color: red;">string</span>.
- Then we might also have another copy or two - as the object is returned from the function. But usually, the compiler can optimize and elide the copies (especially since C++17 when Copy Elision became mandatory in that case).
- If the string is short, then there might be no heap allocation as Small String Optimisation.


<table><tr><td>Small String Optimisation is not defined in the C++ Standard, but it's a common optimisation across popular compilers. Currently, it's 15 characters in MSVC (VS 2017)/GCC (8.1) or 22 characters in Clang (6.0)</td></tr></table>

### Solution

``` c++
std::string_view StartFromWord(std::string_view str, std::string_view word)
{
  return str.substr(str.find(word)); // substr creates only a new view 
}

int main() {
  // call:
  std::string str {"Hello Amazing Programming Environment"}; 
  
  auto subView = StartFromWord(str, "Programming Environment");
  std::cout << subView << '\n';
}
```