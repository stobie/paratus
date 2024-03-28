# C++17 in Detail: A Deep Dive

## L: Searchers and String Matching
==================================================================

https://www.educative.io/courses/cpp-17-in-detail-a-deep-dive/introduction-xV20YyQL7MP

###  Updates and New Features


<span style="color: red;">C++17</span> updated <span style="color: red;">std::search</span> algorithm in two ways:

- You can now use execution policy to run the default version of the algorithm in a parallel way.
- You can provide a Searcher object that handles the search.

In <span style="color: red;">C++17</span> we have three searchers:

- <span style="color: red;">default_searcher</span> - same as the version before C++17, usually meaning the naive approach. Operates on Forward Iterators.
- <span style="color: red;">boyer_moore_searcher</span> - uses Boyer Moore Algorithm - the full version, with two rules: bad character rule and good suffix rule. Operates on Random Access Iterators.
- <span style="color: red;">boyer_moore_horspool_searcher</span> - Simplified version of Boyer-Moore that uses only Bad Character Rule, but still has good average complexity. Operates on Random Access Iterators.


<span style="color: red;">std::search</span> with a searcher cannot be used along with execution policy.


### Example - Using Searchers - Performance Experiment


``` c++
#include <iostream>
#include <string>
#include <algorithm>
#include <functional>
#include <chrono>
#include <fstream>
#include <string_view>
#include <numeric>
#include <sstream>
#include "simpleperf.h"

using namespace std::literals;

const std::string_view LoremIpsumStrv{ "Lorem ipsum dolor sit amet, consectetur adipiscing elit, "
  "sed do eiusmod tempor incididuntsuperlongwordsuper ut labore et dolore magna aliqua. Ut enim ad minim veniam, "
  "quis nostrud exercitation ullamco laboris nisi ut aliquipsuperlongword ex ea commodo consequat. Duis aute "
  "irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. "
  "Excepteur sint occaecat cupidatatsuperlongword non proident, sunt in culpa qui officia deserunt mollit anim id est laborum." };

std::string GetNeedleString(int argc, const char** argv, const std::string &testString)
{
  const auto testStringLen = testString.length();
  if (argc > 3)
  {
    const size_t tempLen = atoi(argv[3]);
    if (tempLen == 0) // some word?
    {
      std::cout << "needle is a string...\n";
      return argv[3];
    }
    else
    {
      const size_t PATTERN_LEN = tempLen > testStringLen ? testStringLen : tempLen;
      const int pos = argc > 4 ? atoi(argv[4]) : 0;

      if (pos == 0)
      {
        std::cout << "needle from the start...\n";
        return testString.substr(0, PATTERN_LEN);

      }
      else if (pos == 1)
      {
        std::cout << "needle from the center...\n";
        return testString.substr(testStringLen / 2 - PATTERN_LEN / 2 - 1, PATTERN_LEN);
      }
      else
      {
        std::cout << "needle from the end\n";               
        return testString.substr(testStringLen - PATTERN_LEN - 1, PATTERN_LEN);
      }
    }
  }

  // just take the 1/4 of the input string from the end...
  return testString.substr(testStringLen - testStringLen/4 - 1, testStringLen/4);
}

int main(int argc, const char** argv)
{
  std::string testString{ LoremIpsumStrv };

  std::cout << "string length: " << testString.length() << '\n';

    const size_t ITERS = argc > 2 ? atoi(argv[2]) : 1000;
    std::cout << "test iterations: " << ITERS << '\n';

    const auto needle = GetNeedleString(argc, argv, testString);
    std::cout << "pattern length: " << needle.length() << '\n';

    RunAndMeasure("string::find", [&]() {
        for (size_t i = 0; i < ITERS; ++i)
        {
            std::size_t found = testString.find(needle);
            if (found == std::string::npos)
                std::cout << "The string " << needle << " not found\n";
        }
        return 0;
    });

    RunAndMeasure("default searcher", [&]() {
        for (size_t i = 0; i < ITERS; ++i)
        {
            auto it = std::search(testString.begin(), testString.end(),
                std::default_searcher(
                    needle.begin(), needle.end()));
            if (it == testString.end())
                std::cout << "The string " << needle << " not found\n";
        }
        return 0;
    });

    RunAndMeasure("boyer_moore_searcher init only", [&]() {
        for (size_t i = 0; i < ITERS; ++i)
        {
            std::boyer_moore_searcher b(needle.begin(), needle.end());
            DoNotOptimizeAway(&b);
        }
        return 0;
    });

    RunAndMeasure("boyer_moore_searcher", [&]() {
        for (size_t i = 0; i < ITERS; ++i)
        {
            auto it = std::search(testString.begin(), testString.end(),
                std::boyer_moore_searcher(
                    needle.begin(), needle.end()));
            if (it == testString.end())
                std::cout << "The string " << needle << " not found\n";
        }
        return 0;
    });

    RunAndMeasure("boyer_moore_horspool_searcher init only", [&]() {
        for (size_t i = 0; i < ITERS; ++i)
        {
            std::boyer_moore_horspool_searcher b(needle.begin(), needle.end());
            DoNotOptimizeAway(&b);
        }
        return 0;
    });

    RunAndMeasure("boyer_moore_horspool_searcher", [&]() {
        for (size_t i = 0; i < ITERS; ++i)
        {
            auto it = std::search(testString.begin(), testString.end(),
                std::boyer_moore_horspool_searcher(
                    needle.begin(), needle.end()));
            if (it == testString.end())
                std::cout << "The string " << needle << " not found\n";
        }
        return 0;
    });
}

```


```
Output

string length: 489
test iterations: 1000
pattern length: 122
string::find: 0.032944 ms
default searcher: 10.7519 ms
boyer_moore_searcher init only: 13.6508 ms
boyer_moore_searcher: 18.9295 ms
boyer_moore_horspool_searcher init only: 3.68987 ms
boyer_moore_horspool_searcher: 5.38516 ms

```