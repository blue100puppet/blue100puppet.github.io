---
title: Laplace Transform
date: 2026-04-15 22:31:26
aliases: []
tags: [math/calculus/transform-methods]
---

# Laplace Transform

## Definition
The Laplace transform of a function $f(t)$ is defined as:

$$
F(s) = \mathcal{L}\{f(t)\} = \int_{0}^{\infty} e^{-st} f(t)\,dt
$$

where $s = \sigma + i\omega$ is a complex number.

## Key Properties

- Linearity: $\mathcal{L}\{af(t) + bg(t)\} = aF(s) + bG(s)$
- Shifting: $\mathcal{L}\{f(t - a)u(t - a)\} = e^{-as}F(s)$
- Differentiation: $\mathcal{L}\{f'(t)\} = sF(s) - f(0)$

## Applications

- Solving differential equations — transforms them into algebraic equations
- In control theory for system analysis
- In circuit theory for analyzing RLC networks
- Converts convolution products into simple multiplication

## Inverse

The inverse Laplace transform recovers $f(t)$:

$$
f(t) = \mathcal{L}^{-1}\{F(s)\}
$$

## Connections

- Useful for ODEs, which connects to [[lco3_Partial Fraction Decomposition]] for rational transforms
- Related to Fourier transform when $s = i\omega$

## Remarks

- Transforms functions from time domain to complex frequency (s-domain)
