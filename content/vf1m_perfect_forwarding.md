---
title: Perfect Forwarding
date: 2026-04-18 04:27:35
aliases: ["perfect forwarding idiom"]
tags: [cpp, templates, forwarding]
---

# Context

Preserving the lvalue/rvalue nature of a function template's argument when passing it onward.

## Problem

```cpp
template<typename T>
void wrapper(T&& arg) { foo(arg); }  // arg is always an lvalue inside!
```

## Solution

```cpp
template<typename T>
void wrapper(T&& arg) { foo(std::forward<T>(arg)); }
```

`std::forward<T>` casts back to the original value category using the deduced `T`.

## Classic Example

```cpp
template<typename T1, typename T2>
class Pair {
    T1 first; T2 second;
public:
    template<typename U1, typename U2>
    Pair(U1&& a, U2&& b)
        : first(std::forward<U1>(a)),
          second(std::forward<U2>(b)) {}
};
```

## See Also

- [[hi6f_universal_refs]]
- [[std::forward]]
- [[r63t_ref_collapsing]]
