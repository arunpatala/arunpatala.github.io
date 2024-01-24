---
layout: post
title: "Understanding Deep Learning"
date:   2024-01-22 10:53:18 +0530
categories: deep-learning ai
---

Deep learning is a subset of machine learning where neural networks — algorithms inspired by the human brain — learn from large amounts of data. 

## Key Concepts

### Neural Networks

A neural network consists of layers of interconnected nodes, or neurons, each performing a simple computation. The output of one layer becomes the input of the next.

### The Math Behind It

The core operation in a neuron is the dot product. Given a vector of inputs \( \mathbf{x} \) and a vector of weights \( \mathbf{w} \), the dot product is:

\[ \mathbf{w} \cdot \mathbf{x} = \sum_{i=1}^{n} w_i x_i \]

Where:
- \( \mathbf{x} \) is the input vector.
- \( \mathbf{w} \) is the weights vector.
- \( n \) is the number of elements in the vectors.

### Activation Functions

After the dot product, an activation function is applied. A common activation function is the sigmoid, defined as:

\[ \sigma(x) = \frac{1}{1 + e^{-x}} \]

It "squashes" the input to a range between 0 and 1.

### Backpropagation and Loss Functions

Backpropagation is the key algorithm for training neural networks. It involves computing the gradient of the loss function with respect to each weight. The loss function measures the difference between the network's prediction and the actual target values. A common loss function is the mean squared error (MSE):

\[ MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 \]

Where:
- \( y_i \) are the true values.
- \( \hat{y}_i \) are the predicted values.
- \( n \) is the number of samples.

---

Deep learning has revolutionized many fields, including image and speech recognition, natural language processing, and more.
