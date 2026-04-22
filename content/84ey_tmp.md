---
title: Template Metaprogramming (TMP)
date: 2026-04-18 04:27:35
aliases: ["TMP", "template metaprogramming"]
tags: [cpp, templates, metaprogramming]
---

# Context

Using templates to compute types and values at compile time. Turing-complete.

## Basic Pattern

```cpp
template<int N>
struct Factorial {
    static constexpr int value = N * Factorial<N-1>::value;
};
template<>
struct Factorial<0> { static constexpr int value = 1; };
```

## Type Traits Are TMP

Most `<type_traits>` are implemented with template specialization.

## Modern TMP

C++17 `if constexpr` simplifies many TMP patterns. Prefer it over partial specialization when possible.

## Compile-Time Sequences

`std::integer_sequence` (C++14) generates packs of integers.

## Cost

Heavy TMP slows compilation. Use judiciously.

## See Also

- [[wdj9_type_traits]]
- [[if constexpr]]
- [[Variadic Templates]]
