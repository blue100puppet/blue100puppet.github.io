---
title: Simple Backpropagation
date: 2026-04-15 22:47:59
aliases: []
tags: [machine-learning/neural-networks, calculus]
---

# Simple Backpropagation

## Overview
Backpropagation (backward propagation of errors) is the algorithm used to train neural networks by computing gradients of the loss function with respect to each weight.

## Key Idea
1. **Forward pass**: Input flows through network → output computed
2. **Loss calculation**: Compare output to expected target
3. **Backward pass**: Compute derivatives from output layer back to input layer
4. **Gradient descent**: Adjust weights in direction that minimizes loss

## Chain Rule in Action
For a simple neuron:

$$
\frac{\partial L}{\partial w} = \frac{\partial L}{\partial y} \cdot \frac{\partial y}{\partial z} \cdot \frac{\partial z}{\partial w}
$$

where:
- $L$ = loss
- $y$ = neuron output
- $z$ = pre-activation ($w \cdot x + b$)
- $w$ = weight

## Simple Example (1 neuron)

Assume neuron: $y = \sigma(w x + b)$
Loss: $L = \frac{1}{2}(y - t)^2$

Then:
$$
\frac{\partial L}{\partial w} = (y - t)\ \sigma'(w x + b)\ x
$$

## Steps in Code
1. Record all intermediate values during forward pass
2. Compute derivative of loss w.r.t. final output
3. Work backwards through the computational graph
4. Accumulate gradients for each weight parameter
5. Update weights: $w \leftarrow w - \alpha \, \frac{\partial L}{\partial w}$

## Common Pitfalls

- **Vanishing gradients**: Gradients become extremely small in deep nets (sigmoid/tanh saturation)
- **Exploding gradients**: Gradients blow up (causes unstable training)
- **Local minima**: Non-convex loss landscape can trap near-optimal solutions
- **Careful initialization**: Xavier/Glorot, He init help avoid these

## Applications

- Training feedforward neural networks
- Fine-tuning deep networks (with techniques like dropout, batch norm)
- Fundamentally used by practically all deep learning systems

## Related Mathematics

- [[uu87_Laplace Transform]] — used in signal-processing-based neural architectures
- Chain rule from calculus (multivariable differentiation)
- Optimization theory (gradient descent variants)

## Remarks

Backpropagation depends on:
- Differentiable activation functions ($\sigma$, ReLU, tanh, etc.)
- Clear computational graph
- Smooth loss surfaces

