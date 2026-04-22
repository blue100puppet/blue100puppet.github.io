---
title: Move Semantics
date: 2026-04-18 04:27:35
aliases: ["move constructor", "move assignment"]
tags: [cpp, move-semantics, performance]
---

# Context

Transferring resources from one object to another without deep copying — "pilfering" pointers, buffers, file handles.

## Overview

Move constructor: `T(T&& other)`
Move assignment: `T& operator=(T&& other)`

The source (`other`) is left in a valid but unspecified state (usually empty).

## Rule of Five

If you define any of: destructor, copy ctor/assign, move ctor/assign — you likely need all five.

## Example

```cpp
class Buffer {
    int* data;
public:
    Buffer(Buffer&& other) noexcept : data(other.data) {
        other.data = nullptr;  // steal, nullify source
    }
};
```

## When Used

- Returning by value (if RVO not applied)
- Container reallocation (vector moving elements)
- `std::unique_ptr` transfer
- `std::swap`

## noexcept Matters

Containers prefer moves that are `noexcept`. Declare move operations `noexcept` if possible!

## See Also

- [[oqfb_std_move]]
- [[vf1m_perfect_forwarding]]
- [[Rule of Five]]
