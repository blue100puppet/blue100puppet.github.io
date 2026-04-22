---
title: SFINAE — Substitution Failure Is Not An Error
date: 2026-04-18 04:27:35
aliases: ["SFINAE", "enable_if"]
tags: [cpp, templates, metaprogramming]
---

# Context

If template argument substitution fails, the candidate is removed from overload resolution rather than causing a hard error.

## Overview

```cpp
template<typename T>
auto f(T t) -> decltype(t.size()) { return t.size(); }

template<typename T>
void f(T);  // fallback

f(42);  // first fails (no .size()), second chosen
```

## enable_if

```cpp
template<typename T,
         typename = std::enable_if_t<std::is_integral_v<T>>>
void process(T value);
```

Only participates if condition true.

## Where SFINAE Applies

- Function parameter types
- Return type
- Default template arguments

NOT in function body (checked later).

## See Also

- [[u2my_concepts]] — modern alternative
- [[wdj9_type_traits]]
