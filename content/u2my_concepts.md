---
title: Concepts (C++20)
date: 2026-04-18 04:27:35
aliases: ["concepts", "requires clause"]
tags: [cpp, cpp20, templates]
---

# Context

Compile-time predicates that constrain template parameters. Much clearer than SFINAE.

## Basic

```cpp
template<std::integral T>
T add(T a, T b) { return a + b; }
```

## requires Expression

```cpp
template<typename T>
concept Addable = requires(T a, T b) { a + b; };

template<Addable T>
T sum(T a, T b) { return a + b; }
```

## Standard Concepts

`std::integral`, `std::floating_point`, `std::same_as`, `std::derived_from`, `std::ranges::input_range`, etc.

## Why Better Than SFINAE

- Readable constraints
- Good diagnostics
- Part of function signature

## See Also

- [[yxzq_sfinae]]
- [[wdj9_type_traits]]
