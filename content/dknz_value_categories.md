---
title: "C++ Value Categories: lvalue, rvalue, xvalue, prvalue, glvalue"
date: 2026-04-18 04:27:35
aliases: ["lvalue rvalue", "value categories"]
tags: [cpp, language-basics]
---

# Context

Understanding whether an expression is an lvalue, rvalue, or something else is key to mastering references, moves, and overload resolution.

## Overview

| Category | Description | Example |
|----------|-------------|---------|
| **lvalue** | Has identity/memory address | `x`, `*ptr`, `arr[0]` |
| **xvalue** | Expiring value (has identity, can be moved) | `std::move(x)`, `T&&` return |
| **prvalue** | Pure rvalue, no identity (temporary) | `42`, `3.14`, `T()` |
| **glvalue** | lvalue OR xvalue (has identity) |
| **rvalue** | xvalue OR prvalue (can bind to `&&`) |

Simplified: **lvalue = named object, rvalue = temporary**.

## Reference Binding

- `T&` → lvalues only
- `const T&` → any category
- `T&&` → rvalues only (prvalue or xvalue)

```cpp
int x = 0;
int&  r1 = x;     // OK
int&& r2 = 42;    // OK
int&& r3 = x;     // ERROR (x is lvalue)
int&& r4 = std::move(x); // OK (xvalue)
```

## Takeaway

- Move semantics rely on xvalues (objects we can pilfer).
- Perfect forwarding preserves the original value category.

## See Also

- [[vf1m_perfect_forwarding]]
- [[oqfb_std_move]]
- [[hi6f_universal_refs]]
