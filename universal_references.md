## What
Universal / Forwarding references are references that **preserve the value category of function arguments**.
This means that passing a rvalue to the function will keep it a rvalue, while passing a lvalue will keep it a lvalue.

## Why
They're mainly needed to allow perfect forwarding through `std::forward`.

## Syntax
There are 2 ways to create an universal/forwarding reference:
1. By creating a function template, where
   - The function parameter is a **rvalue reference to the type template parameter (of the function itself)**
   - This parameter **must not be const or volatile**
```cpp
template<typename T>
void foo(T&& t) // rvalue reference to the type template parameter of the function itself, non const, non volatile
{
  // ...
}
```

2. Using `auto&&`. An exception is using a brace initializer list, in which case this is not considered a forwarding reference (see examples)
```cpp
int main()
{
  auto&& ex = getValue(); // assume getValue() could be either rvalue or lvalue
}
```

## Examples
```cpp
template<typename T>
void foo(const T&& t) {} // not forwarding reference: cannot use const or volatile for the reference

template<typename T>
struct Foo
{
  void foo(T&& t) {} // not forwarding reference: the rvalue reference must refer to the function's type template parameter
};

template<typename T>
void foo(T&& t) const noexcept {} // forwarding reference, "const noexcept" aren't relevant to allow or disallow it

int main()
{
  auto&& no = {1, 2, 3}; // exception: this is not a forwarding reference
}
```
