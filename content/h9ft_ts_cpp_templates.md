---
title: TypeScript Generics vs C++ Templates
date: 2026-04-18 04:27:35
aliases: ["TS generics vs C++ templates", "template vs generic"]
tags: [cpp, templates, typescript, comparison]
---

# Context

If you know TypeScript generics, C++ templates may feel familiar but have **critical differences** in when and how they operate.

## Overview

| Aspect | TypeScript Generics | C++ Templates |
|--------|-------------------|---------------|
| **When** | Compile-time type erasure; single JS output | Compile-time code generation; unique code per instantiation |
| **Purpose** | Type hints only; no runtime impact | Generates actual machine code for each type |
| **Specialization** | No (except function overloads) | Yes — full partial/explicit specialization |
| **Metaprogramming** | Limited (keyof, typeof, conditional types) | Turing-complete (constexpr if, SFINAE, recursion, concepts) |
| **Error location** | Point of use (after erasure) | Point of definition (instantiation) |
| **Compilation** | Fast (single pass) | Slow (header bloat, repeated parsing) |

## TypeScript Generics (Erasure)

```typescript
function identity<T>(arg: T): T { return arg; }
const n = identity<number>(42);  // same JS code for any T
```

- Type parameters exist **only at compile time**.
- No code generation per type; single JavaScript emitted.
- Type constraints: `T extends SomeInterface`

## C++ Templates (Code Generation)

```cpp
template<typename T>
T identity(T arg) { return arg; }
int n = identity(42);   // instantiates `int identity<int>(int)`
double d = identity(3.14); // instantiates `double identity<double>(double)`
```

- Each instantiation is a separate function/class.
- Type parameters are **real types** — can use `sizeof(T)`, `is_pointer_v<T>`.

## Key Takeaway

- TS generics = type-level placeholders, erased at runtime.
- C++ templates = compile-time code generation, zero-cost abstraction.

## See Also

- [[vf1m_perfect_forwarding]]
- [[dknz_value_categories]]
