# parameter packs

I wa watchin *Lightning Talk: A Pack as an Optional Parameter*
* https://www.youtube.com/watch?v=dl8Fdw5F8_U&t=205s

I din't really follow, I am not well versed in packs so I asked ChatGPT:
> how does argument packs work in c++?

Argument packs (usually called **parameter packs**) in C++ are a core feature of variadic templates—they let you write functions or classes that accept **any number of arguments of potentially different types** in a type-safe way.

---

## 🔹 The basic idea

A parameter pack is written with `...` and represents **zero or more parameters**.

### Example:

```cpp
template<typename... Args>
void func(Args... args) {
    // args is a parameter pack
}
```

* `Args...` → a **template parameter pack** (types)
* `args...` → a **function parameter pack** (values)

---

## 🔹 Expanding a parameter pack

You don’t use packs directly—you **expand** them using `...`.

### 1. Simple expansion

```cpp
template<typename... Args>
void printAll(Args... args) {
    (std::cout << ... << args);  // fold expression (C++17)
}
```

This expands into something like:

```cpp
std::cout << arg1 << arg2 << arg3;
```

---

## 🔹 Fold expressions (C++17)

Before C++17, expanding packs was messy. Fold expressions simplify it.

### Syntax:

```cpp
(... op pack)        // unary left fold
(pack op ...)        // unary right fold
(init op ... op pack) // binary fold
```

### Example:

```cpp
template<typename... Args>
auto sum(Args... args) {
    return (args + ...); // adds all arguments
}
```

Call:

```cpp
sum(1, 2, 3); // → 6
```

---

## 🔹 Recursive unpacking (pre-C++17 style)

Before fold expressions, recursion was used:

```cpp
void print() {}

template<typename T, typename... Rest>
void print(T first, Rest... rest) {
    std::cout << first << " ";
    print(rest...);
}
```

---

## 🔹 Perfect forwarding

Parameter packs are often used with **forwarding references**:

```cpp
template<typename... Args>
void wrapper(Args&&... args) {
    func(std::forward<Args>(args)...);
}
```

This preserves whether each argument is:

* lvalue
* rvalue

---

## 🔹 Pack expansion in different contexts

You can expand packs in many places:

### Function arguments:

```cpp
func(args...);
```

### Initializer lists:

```cpp
int arr[] = {args...};
```

### Base classes:

```cpp
template<typename... Bases>
class Derived : public Bases... {};
```

---

## 🔹 Example: Variadic constructor

```cpp
#include <iostream>

class Logger {
public:
    template<typename... Args>
    void log(Args... args) {
        (std::cout << ... << args) << '\n';
    }
};
```

Usage:

```cpp
Logger l;
l.log("Value: ", 42, ", pi: ", 3.14);
```

---

## 🔹 Key points to remember

* `...` means “zero or more”
* Packs must always be **expanded**
* Expansion happens where `...` appears
* Fold expressions make pack usage much cleaner
* Often combined with templates and forwarding

---

## 🔹 When to use argument packs

Use them when:

* You want flexible APIs (like `printf` but type-safe)
* You’re writing generic libraries
* You need to forward arguments (e.g., factory functions)

---

If you want, I can show:

* common mistakes with parameter packs
* how `std::tuple` and `std::apply` use them
* or a step-by-step mental model of how the compiler expands them
