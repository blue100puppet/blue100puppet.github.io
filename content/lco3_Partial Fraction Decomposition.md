---
title: Partial Fraction Decomposition
date: 2025-12-03 18:48:32
aliases: []
tags: [math/calculus/integration]
---

# Partial Fraction Decomposition

## Example

Consider

$$
\frac{P(x)}{Q(x)}
$$

Where

$$
Q(x) = (x + a)(x^2 + b)(x^2 + c)^2(x + d)^3
$$

And $\text{deg}(P) < \text{deg}(Q)$.

Our partial fraction decomposition is:

$$
\frac{P(x)}{Q(x)} = \frac{A_1}{x + a} + \frac{A_2 x + A_3}{x^2 + b} + \frac{A_4 x + A_5}{x^2 + c}
$$

## Remarks

- Useful for integrals
- Useful for solving ODEs when using [[uu87_Laplace Transform]]
