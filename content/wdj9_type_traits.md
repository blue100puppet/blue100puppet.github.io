---
title: Type Traits (C++11+)
date: 2026-04-18 04:27:35
aliases: ["type traits", "std::is_*"]
tags: [cpp, templates, metaprogramming]
---

# Context

Compile-time type introspection in `<type_traits>`. Foundation for TMP and SFINAE.

## Queries

```cpp
std::is_integral_v<T>
std::is_pointer_v<T>
std::is_same_v<T, U>
std::is_base_of_v<Base, Derived>
std::is_constructible_v<T, Args...>
```

## Transformations

```cpp
std::remove_reference_t<T>
std::add_const_t<T>
std::decay_t<T>  // remove ref, cv, array/function decay
std::conditional_t<Cond, T, F>
```

## enable_if

```cpp
template<typename T,
         std::enable_if_t<std::is_integral_v<T>, int> = 0>
void f(T);
```

## See Also

- [[yxzq_sfinae]]
- [[u2my_concepts]]
- [[84ey_tmp]]
