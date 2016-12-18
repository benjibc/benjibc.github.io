---
layout: post
title: [KDD2016][Google] Robust Large-Scale Machine Learning in the Cloud
---

# Robust Large-Scale Machine Learning in the Cloud

[link | Robust Large-Scale Machine Learning in the Cloud]

This paper describes the algorithm to do scale machine learning training using the new scalable coordinate descent.

> In this paper, we describe a new scalable coordinate descent (SCD) algorithm for generalized linear models whose convergence behavior is always the same, regardless of how much SCD is scaled out and regardless of the computing environment. 

Reason why you want to do this is that sometimes you have soo much data that training on a single instance is no longer feasible. Why you want that much data is probably because you have a sparse model setup.

> We also show that SCD can learn a model for 20 billion training examples in two hours for about $10.

The $10 here is not that important. The 20 billion training examples is important

# Introduction/Main contributions
> Because of these scaling problems, some authors [9] have argued that it is better to scale out ML algorithms using just a few ‘fat’ servers with lots of memory, networking cards, and GPUs. While this may be an appealing approach for some problems, it has obvious scaling limitations in terms of I/O bandwidth.

The IO bandwidth they are talking about here is probably the memory bandwidth and the cost of syncing parameters to the parameter servers, if they are talking about the setup that CMU had earlier.

> A key observation of our work is that, by using a natural partitioning of parameters into blocks, updates can be performed
in parallel a block at a time without compromising convergence. In fact, on many real-world problems, SCD has the same convergence behavior as the popular single-machine coordinate descent algorithm.

> In addition to the SCD algorithm, we describe a distributed system that addresses the specific challenges of scaling SCD in a cloud computing environment. Straggler handling ends up being the biggest challenge to achieving linear scaling with SCD.

# Problem statement
### Machine Learning Environment
> We focus on generalized linear models with large sparse datasets.

(Screenshot for linear model)

They are probably oly interested in logistic regression.

(Screenshot for optimization task)

> We focus on datasets with potentially trillions of training examples, that is, |S| ∈ O(10^12). Consequently, the training data does not fit in memory. We also focus on models with billions of features, that is, p ∈ O(10^9). The model will usually fit in memory, but our proposed solution can also handle models that are larger than memory.

You probably never want it to not fit into memory anyways. If it does not fit into memory, it probably involves a lot of weird tricks that you might not want to do in production on the prediction path. Also, throwing money at adding more memory will probably be good enough, although I don't know how the memory bandwidth would work out.

> The reason for high sparsity is usually because of one-hot encoded categorical variables. For example, part of the feature vector x might contain a country variable that is represented by a binary vector with as many entries as there are countries.

The paper also talked about the example for video ID and user ID, and that is when you have really, really high sparsity.

### Computing Environment
They assume the cloud has the following features: Shared Machine (stack multiple instances on a single machine); Distributed File Systems (they talk about GFS, but we all have HDFS); Preemptible VMs (include grace period that is long enough for the application to save its states to the file system for fault tolerance, if necessary); Machine failures.

### Goals of Distributed Learning
- Robust Distribution: same convergence behavior regardless of scale
- Linear scale out
- Linear speed up

# Learning Algorithms (finally the good part)
###  Coordinate Descent

(Screenshot for SCD)

y here is prediction.

> ** For sparse data, the computation for Tj and T'j can be accelerated by only iterating over examples x where x != 0.**

(Screenshot for algorithm 1)

So can we just have a barrier between each iteration and use that on top of algorithm 1?

> A straightforward distributed version of Algorithm 1 would require a system-wide synchronization barrier just after aggregating
the sufficient statistics Tj and T'j (lines 8-11). In general, distributing work only pays off if the overhead in terms of communication and barriers is small compared to the amount of work that is parallelized. However, with a large number p of parameters and a high sparsity (NZ (x) << p) per feature vector x, there is relatively little work to be done in an iteration of CD. Consequently, a straightforward distributed version of Algorithm 1 would neither scale-out nor speed-up.

## Scalable Coordinate Descent

(screenshot for scalable coordinate descent)
