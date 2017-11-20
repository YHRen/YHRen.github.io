---
layout: post
title: Learning PyTorch Part I 
published: true
date: 2017-11-13
tag: [deep learning, pytorch]
---

### Introduction
Currently, I am participating the deep learning part1v2 course as an "international
fellow". This course is taught by Jeremy Howard from
[fast.ai](http://http://www.fast.ai/). The course is not available to the
public yet, but it will be in future.  During the course, Jeremy introduced
PyTorch and the fastai package built on top of PyTorch.  Before this, I only
used Tensorflow and Keras. PyTorch is quite different (in a good way). I am
very impressed by the elegant and flexible design of PyTorch. I would like to
introduce some features I think interesting.

### Tensor, Variable and Numpy Array
[`torch.Tensor`](http://pytorch.org/docs/master/tensors.html) is the container
of data. One can convert to and from numpy array using: `.numpy()` and
`.from_numpy(X)`. `Tensor` supports many mathematical operations, and the cool
part is that most of them have the "in place" counterparts. For example, `Y.add(X)` 
will return a new Tensor, but `Y.add_(X)` will update the value of `Y` in
place.  

`Variable` is a wrapper of `Tensor`, which can be accessed by `.data`. 
In addition to the data, the `Variable` also contains the `.grad` tracking
the gradient of the corresponding data and the `.grad_fn` tracking the
functional relations with other Variables. If you are familiar with the
computation graph (directed acyclic graph to be more specific) in Keras,
TensorFlow and Theano, Variables are nodes in this graph and `.grad_fn` are
edges to its neighbors. I will refer Keras, TensorFlow and Theano as static
frameworks, as one needs to construct the computation graph and *compile* it
before any computation. Being different from the static frameworks, PyTorch does not
compile the computation graph. It computes the gradients on the fly via
[`.backward()`](http://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html)
method according to the current computation graph structure. In my opinion,
this is extremely powerful. This potentially enables us to "learn" the
optimal architecture during training, rather than manually design an
architecture beforehand. For example, one can start with a relatively simple
architecture and add more neurons and/or layers during training. 
Another cool part about `Variable` is that it can be moved to gpu memory
via `.cuda()` and to host memory via `.cpu()`. If you have experience with CUDA programming, 
you may find this feature is very intuitive. 

### Optimization 
The optimization part is similar to other frameworks.  First, we need choose a
[loss
function](http://pytorch.org/docs/master/_modules/torch/nn/modules/loss.html)
based on our needs.  Then, after each minibatch, compute the loss and compute
the gradients of all `Variables` via `loss.backward()`. Then, use the
optimizer's `.step()` function to update the changes.  This may seem very
cumbersome at the beginning, but it provides a lot of flexibility.  For
example, one can change the learning rate of the optimizer, or even change to a
different optimizer entirely. Another caveat is that, before training each
minibatch, one should call `.zero_grad()` function to set all variables `grad`
to zero. The optimizer only updates the variables using their `grads`, but does
not reset the `grads` to zero.

### At the End
To get started, I highly recommend the [official A 60 mins
Blitz](http://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)
and a wonderful PyTorch hands on tutorial [yunjey's PyTorch tutorial on
github](https://github.com/yunjey/pytorch-tutorial).

