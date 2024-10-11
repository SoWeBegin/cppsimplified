## What
Reference collapsing rules are 4 rules that decide how references to references are collapsed.
```cpp
T& & => becomes T&
T& && => becomes T&
T&& & => becomes T&
T&& && => becomes T&&
```

## Why
Reference collapsing rules allow having references to references, e.g. through templates and typedefs:
```cpp
int main()
{
  typedef int&  lref;
  typedef int&& rref;
  int n;

  // Thanks to reference collapsing:
  lref&  r1 = n; // type of r1 is int&
  lref&& r2 = n; // type of r2 is int&
  rref&  r3 = n; // type of r3 is int&
  rref&& r4 = 1; // type of r4 is int&&
}
```

This set of rules allow `std::forward` (in particular, *perfect forwarding*) to work.
