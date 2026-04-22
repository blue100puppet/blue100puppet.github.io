---
title: Universal and Forwarding References
date: 2026-04-18 04:27:35
aliases: ["universal reference", "forwarding reference"]
tags: [cpp, references, templates]
---

# Context

A `T&&` parameter where `T` is a deduced template parameter can bind to both lvalues and rvalues. This is a **forwarding reference** (aka universal reference).

## Overview

```cpp
template<typename T>
void f(T&& arg);  // forwarding reference — T is deduced

void g(MyType&& arg);  // rvalue reference — MyType is fixed, not deduced
```

- `f` can accept **both** lvalues and rvalues.
- Deduction: lvalue → `T = X&` → parameter becomes `X&`; rvalue → `T = X` → parameter becomes `X&&`.

## Reference Collapsing

Rules make `T& &&` collapse to `T&`, enabling lvalue binding.

## Perfect Forwarding

Use `std::forward<T>(arg)` inside to preserve the original value category.

## See Also

- [[vf1m_perfect_forwarding]]
- [[r63t_ref_collapsing]]
- [[dknz_value_categories]]
