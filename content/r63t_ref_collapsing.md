---
title: Reference Collapsing Rules
date: 2026-04-18 04:27:35
aliases: ["reference collapsing"]
tags: [cpp, references, templates]
---

# Context

When a reference is formed from another reference (e.g., `T& &&`), C++ collapses it.

## Rules

| Pattern | Result |
|---------|--------|
| `T& &`  | `T&`   |
| `T& &&` | `T&`   |
| `T&& &` | `T&`   |
| `T&& &&`| `T&&`  |

If **any** `&` appears, result is `&`.

## Why Needed

Template deduction with `T&&` for lvalues yields `T = X&`, making parameter type `X& &&` → collapses to `X&`, so lvalues bind correctly.

## In Perfect Forwarding

`std::forward<T>(arg)` does `static_cast<T&&>(arg)`. Collapsing ensures:
- `T = X&` → `X& &&` → `X&` (lvalue)
- `T = X` → `X&&` (rvalue)

## See Also

- [[hi6f_universal_refs]]
- [[vf1m_perfect_forwarding]]
