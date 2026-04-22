---
title: "std::move — Unconditional Cast to Rvalue"
date: 2026-04-18 04:27:35
aliases: ["std::move", "move semantics"]
tags: [cpp, move-semantics, stl]
---

# Context

`std::move` is a **cast**, not a move operation. It unconditionally converts an expression to an xvalue (expiring value).

## Signature

```cpp
template<typename T>
constexpr std::remove_reference_t<T>&& move(T&& t) noexcept;
```

## Usage

```cpp
std::string s = "hello";
std::string t = std::move(s);  // s becomes valid but unspecified
```

The actual move happens in `std::string`'s move constructor, triggered because `std::move(s)` is an xvalue.

## Pitfalls

- Don't `std::move` a const object — move ctor requires non-const.
- Don't `std::move` something you still need.
- Don't `std::move` temporaries (they're already rvalues).
- Don't `return std::move(local)` — inhibits NRVO.

## See Also

- [[0u18_move_semantics]]
- [[vf1m_perfect_forwarding]]
- [[dknz_value_categories]]
