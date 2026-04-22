---
title: Template Argument Deduction
date: 2026-04-18 04:27:35
aliases: ["TAD", "template argument deduction"]
tags: [cpp, templates, compiler]
---

# Context

When calling a function template without explicit template args, compiler deduces `T` from arguments.

## Overview

Deduction compares parameter type with argument type:

| Parameter | Lvalue Arg | Rvalue Arg |
|-----------|-----------|------------|
| `T` | `T = X` (decay) | `T = X` |
| `T&` | `T = X` → param `X&` | can't bind |
| `T&&` (forwarding ref) | `T = X&` → param `X&` | `T = X` → param `X&&` |

## Key Points

- Top-level `const`/`&` are dropped for by-value `T`.
- Arrays decay to pointers unless `T&` or `T&&`.
- All `T`s must deduce to same type in a single call.
- Deduction failure is SFINAE in function templates.

## See Also

- [[hi6f_universal_refs]] — special T&& rule
- [[yxzq_sfinae]]
- [[Class Template Argument Deduction (CTAD)]]
