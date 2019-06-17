---
layout: post
title: Guides On Choosing Deep Learning Server  
author: Yihui (Ray) Ren
published: true
date: 2019-06-10
tag: [hardware] [deep-learning]
---

## Introduction
Choosing the right GPU server for deep learning is the first problem presented
to the research teams among industry and academia. 
This article is to introduce a few tips in picking the right hardware for your team.

If the purpose of the server is mainly for development, a RTX server would be the most cost effective. 
It is for production, namely to go though TB to PB of data, it is better to use high-end scalable servers. 
So one can train a single model in parallel efficiently.

## GPUs
I would recommend NVIDIA V100, RTX6000 and RTX 2080Ti. (as the time of writing this article, mid of 2019.)

**V100** is the flagship model for deep learning, and is incorporated in all
leading-edge high-end deep learning systems. It has 6 NVLink ports for scalable
deep learning training. It also has 32GB HBM2 GPU memory. The downside is that
it costs about $10k.

**RTX 2080Ti** has about 90% of FLOPS of V100, but only has 2 NVLink ports (and
often not utilized) and 11GB GDDR6 GPU memory. It costs only about $1.3k.

**RTX 6000** is new. Similar (if not exactly the same) to Titan RTX, it
slightly better than RTX 2080Ti in terms of FLOPS. But it has 24GB GDDR6 GPU
memory. It costs about $4k~6k.

GPU memory matters when train large models in parallel, because each training
batch can accommodate more data and the communication per data point is
therefore reduced.

I would not recommend AMD GPUs due to the lack of software/library support,
although the new architecture is very promising.


## CPU and host memory
I would recommend x86-based CPUs due to better software/library support.
The CPU memory should be about 3 times of the total GPU memory. 

## Several popular options
An **RTX server** can have upto 8 GPUs ranging from $25k (8 RTX-2080Ti) to $45k
(8 RTX-6000).  The GPUs are connected via the PCIe bus, so parallel training
would be slow. (How slow depends on the communication workload and the ratio
between communication and computation.) But due to its cost-effectiveness, I
would certainly recommend it for teams trying to dive into the deep learning. 

The **DGX-1V** with 8 V100 GPUs costs about $150k. GPUs are connected via
NVLinks in a hybrid cube-mesh topology, resulting very efficient parallel
training when all 8 GPUs are in use.  But the P2P bandwidth is limited to
one/two NVLink fabrics.

The **DGX-2** with 16 V100 GPUs costs about $400k.  It brings the in-node
scalability into a different level due to the NVSwitches.  Any form of
communication, either collectively or P2P, would fully use the 6 NVLink ports
on the V100 GPUs.

In summary, a system similar to DGX-1V would strike a good balance between
development and training (that's essentially what AWS p3dn instances are). For
the best performance, DGX-2 should be the clear winner. For a cost-effective
solution, customizing a 8 GPU server might be good enough, if the performance
of training model in parallel is not a major concern. A detailed study on the
leading-edge deep learning systems by the author and his colleagues can be
found [here: arxiv:1905.08764](https://arxiv.org/abs/1905.08764)



