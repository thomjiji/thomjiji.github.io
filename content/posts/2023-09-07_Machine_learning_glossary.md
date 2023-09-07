---
title: "Machine Learning Glossary"
date: 2023-09-07
draft: false
tags: ["Machine Learning"]
---

## activation function

A function that enables neural networks to learn non-linear (complex) relationships
between features and the label. Popupar activation functions include:

- ReLU
- Sigmoid
- Tanh

## backpropagation

The algorithm that implements gradient descent in neural networks.

Training a neural network involves many iterations of the following two-pass cycle:

1. During the **forward pass**, the system processes a batch of examples to yild
   prediction(s). The system compares each prediction to each label value. The
   difference between the prediction and the lable value is the loss for that example.
   The system aggregates the losses for all the examples to compute the total loss for
   the current batch.
2. During the **backward pass** (backpropagation), the system reduces loss by adjusting
   the weights of all the neurons in all the hidden layer(s).

Neural networks often contain many neurons across many hidden layers. Each of those
neurons contribute to the overall loss in difference ways. Backpropagation determines
whether to increase or decrease the weights applied to particular neurons.

The learning rate is a multiplier that controls the degree the which each backward pass
increase or decreases each weight. A larget learning rate will increase or decrease each
weight more than a small learning rate.

In calculus terms, backpropagation inplements calculus' chain rule. That is,
backpropagation calculuates the partial derivative of the error with respect to each
parameter.

## model

In general, any mathematical construct that processes input and returns output. Phrased
differently, a model is the set of parameters and structure needed for a system to make
predictions. In supervised machine learning, a model takes an example as input and
infers a prediction as output. Wthin supervised machine learning, models differ
somewhat. For example:

- A linear regression model consists of a set of weights and a bias.
- A neural network model consists of:
  - A set of hidden layers, each containing one or more neurons.
  - The weights and bias associated with each neuron.
- A decision tree model consists of:
  - The shape of the tree; that is, the pattern in which the conditions and leaves are
    connected.
  - The conditions and leaves.

You can save, restore, or make copies of a model.

Unsupervised machine learning also generates models, typically a function that can map
an input example to the most appropriate cluster.