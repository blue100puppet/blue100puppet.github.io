---
title: "std::forward and Perfect Forwarding"
date: 2026-04-18 03:49:38
aliases: ["perfect forwarding", "reference collapsing"]
tags: [cpp, templates, perfect-forwarding]
---

# Context

C++ template programming, universal/forwarding references, and parameter forwarding.

## Overview

`std::forward` is a **cast operator** that preserves the value category (lvalue/rvalue) and cv-qualifiers of a function template's forwarding reference parameter when passing it to another function. It enables **perfect forwarding** — arguments are forwarded exactly as they were received.

---

# Main Content

## The Problem

Consider a generic wrapper function:

```cpp
template<typename T>
void wrapper(T&& arg) {
    // How to forward arg to another function while preserving lvalue/rvalue nature?
    foo(arg);  // BAD: always treats arg as lvalue (even if original was rvalue)
}
```

If `wrapper(42)` is called (rvalue), `arg` is an lvalue inside `wrapper` (named variables are always lvalues). So `foo(arg)` will bind to `foo(const T&)` or `foo(T&)`, not `foo(T&&)` as intended.

## The Solution: Perfect Forwarding

Use `std::forward<T>` to cast `arg` back to its original value category:

```cpp
template<typename T>
void wrapper(T&& arg) {
    foo(std::forward<T>(arg));  // Preserves lvalue/rvalue, const, etc.
}
```

### How It Works

- `T` is deduced as either `U&` (for lvalues) or `U` (for rvalues).
- `T&&` with a deduced `T` becomes a **forwarding reference** (formerly "universal reference").
- `std::forward<T>` uses **reference collapsing rules**:
  - `T = U&` → `static_cast<U&>(arg)` (lvalue)
  - `T = U` → `static_cast<U&&>(arg)` (rvalue)
  - `T = const U&` → `static_cast<const U&>(arg)` (const lvalue)
  - `T = const U` → `static_cast<const U&&>(arg)` (const rvalue)

### Complete Example

```cpp
#include <utility>
#include <iostream>

void process(int& i)  { std::cout << "lvalue ref\n"; }
void process(int&& i) { std::cout << "rvalue ref\n"; }
void process(const int& i)  { std::cout << "const lvalue ref\n"; }
void process(const int&& i) { std::cout << "const rvalue ref\n"; }

template<typename T>
void forwarder(T&& arg) {
    process(std::forward<T>(arg));
}

int main() {
    int x = 0;
    forwarder(x);          // prints "lvalue ref"      (T = int&)
    forwarder(42);         // prints "rvalue ref"      (T = int)
    forwarder(std::move(x)); // prints "rvalue ref"   (T = int)
}
```

## Key Points

- **Never** use `std::forward` with a hardcoded type like `std::forward<int>(arg)`. The template parameter `T` must be the *deduced* parameter type.
- `std::forward` is typically used only in **forwarding functions** (constructors, proxy functions, factory wrappers).
- It does **nothing** if you don't have a forwarding reference (`T&&` where `T` is a template parameter).
- `std::move` is different: it *unconditionally* casts to an rvalue reference. Use `std::move` when you *want* to treat a named object as an rvalue (e.g., to indicate "I'm done with this, you can steal resources").

## Forwarding Reference Detection Rule

A function parameter of the form `T&&` is a forwarding reference **iff** `T` is a deduced template parameter. Otherwise it's just an rvalue reference:

```cpp
template<typename T> void f1(T&& t);  // forwarding reference (T deduced)
void f2(MyType&& t);                   // rvalue reference (MyType fixed)
```

Only `f1` can use `std::forward<T>` correctly.

---

# Remarks

## Common Pitfalls

1. **Omitting std::forward** → arguments become lvalues inside the forwarding function, preventing move semantics.
2. **Using std::forward with wrong T** → if you forward with a different type than the parameter's deduced type, you break reference collapsing and get undefined or surprising behavior.
3. **Overloading on value category** — perfect forwarding lets you write a single function that correctly dispatches to lvalue/rvalue overloads.

## When to Use

- Generic factory/constructor that forwards arguments to another constructor or function
- `emplace_back` in containers (constructs in-place with perfect forwarding)
- Any wrapper that needs to preserve argument semantics

## When NOT to Use

- When you want to *force* an lvalue → use `static_cast<T&>` or just pass the variable
- When you want to *force* an rvalue → use `std::move` or `std::move<T>`
- In non-template code or when `T` is not deduced

## Performance

Perfect forwarding enables move semantics to work through layers of abstraction, avoiding unnecessary copies. Without it, generic code would default to lvalue references, inhibiting moves and potentially causing extra allocations.

## See Also

- `[[oqfb_std_move]]` — unconditional cast to rvalue
- Reference collapsing rules (C++11 §8.5.3)
- Perfect forwarding idiom (Effective Modern C++ Item 24)
