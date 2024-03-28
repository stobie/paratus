# C++17 in Detail: A Deep Dive

## M: String Conversions
==================================================================

https://www.educative.io/courses/cpp-17-in-detail-a-deep-dive/introduction-YV77pRYOx7W



###  C++17 API and New Functions


 The new C++17 API addresses all of the above issues. Rather than providing many functionalities, they focus on giving very low-level support. That way, you can have the maximum speed and tailor them to your needs.

The new functions are guaranteed to be:

- non-throwing - in case of some error they won't throw any exception (as stoi)
- non-allocating - the whole processing is done in place, without any extra memory allocation
- no locale support - the string is parsed universally as if used with default ("C") locale

- memory safety - input and output range are specified to allow for buffer overrun checks

- no need to pass string formats of the numbers

- error reporting additional information about the conversion outcome

All in all, with C++17, you have two sets of functions:

<span style="color: red;">from_chars</span> - for conversion from strings into numbers, integer and floating points.

<span style="color: red;">to_chars</span> - for converting numbers into string.


### The Benchmark - g++ vs clang vs msvc

https://www.educative.io/courses/cpp-17-in-detail-a-deep-dive/the-benchmark


### Summary

This chapter showed how to use two sets of functions <span style="color: red;">from_chars</span> - to convert strings into numbers, and <span style="color: red;">to_chars</span> that converts numbers into their textual representations.

The functions might look very raw and even C-style. This is a "price” you have to pay for having such low-level support, performance, safety, and flexibility. The advantage is that you can provide a simple wrapper that exposes only the needed parts that you want.

Extra Info: The change was proposed in: <span style="color: blue;">P0067</span>.



