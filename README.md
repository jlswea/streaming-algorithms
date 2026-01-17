# Streaming Algorithms

![Cover](data-stream.png)

---

# Introduction

The relatively new field of Data Streaming Algorithms in Computer Science is concerned with studying, developing and improving algorithms that work on data streams as their input.

A data stream is defined as “a sequence of digitally encoded used to represent information in transmission”([Federal Standard 1037C](https://www.its.bldrdoc.gov/fs-1037/dir-010/_1451.htm)). The need to process data as a stream rather than as a static set can result from either:

1.  The data being too large to practically store on any currently accessible storage media, or

2.  the data doesn’t exist as a set (yet) but is being generated over a non deterministic time horizon.

The first point especially applies to computing devices on the edge like IP-routers, IoT-devices or even satellites. For these devices it might be desirable or even necessary to process data locally while also being resource constrained in terms of storage and/or bandwidth.

Furthermore, a data stream is also characterized by the data input arriving at such a high rate that it stresses current communication and computing infrastructure. More formally this means that any data stream algorithm has to cope with at least one of the following three parts of the currently available Transmission, Computation, and Storage (TCS) capacity:

- Transmission (T) of the complete data set is not possible

- Computation (C) of some desired output is not possible at the rate the input is presented

- Storage (S) of the complete data either temporarily for further analysis or long term is not possible

It follows directly from these TCS constraints, that streaming algorithms must operate on sublinear space and time complexity. This can be described more formally with the following model for data streams.

## Data Stream Model

A data stream describes a signal $`S`$ that is in turn comprised of a sequence of items $`s_i`$:

``` math
S = s_1, s_2, s_3, ..., s_m, ...
```

Each $`s_i`$ is in universe $`U`$ with $`|U| = n`$, i.e $`U`$ represents $`n`$ possible values for each $`s_i`$. The index $`i`$ represents the sequential order at which the items are conceived by algorithm $`A`$. This order can also be interpreted as time, implying $`S_i`$ to be the signal $`S`$ at time $`i`$ after item $`s_i`$ was processed by the algorithm. $`A`$ takes $`S`$ as input and computes a function $`f`$ of $`S`$, i.e. $`A: S \mapsto f(S)`$.

Building on this common foundation, there are three main models for data streams. They vary in the way $`s_i`$’s are interpreted and how they describe $`S`$ in the aggregate:

- Time Series Model: The signal $`S`$ takes the value of an item $`s`$ until the next item arrives. For every $`i \in \mathbb{N}:S_i = s_i`$

- Cash Register Model: Here $`s_i`$’s are increments to a given segment $`j`$ of $`S`$: $`s_i = (j, I_i)`$ with $`I_i \geq 0`$ and $`S_i[j] = S_{i-1}[j] + I_i`$. Multiple $`s_i`$’s can increment the same segment $`S[j]`$

- Turnstile Model: This is similar to the Cash Register Model but an item $`s_i`$ contains an update $`U_i`$ that is either $`positive`$ or $`negative`$: $`s_i = (j, U_i)`$ with $`S_i[j] = S_{i-1}[j] + U_i`$

## Complexity Requirements

To satisfy the TCS constraints above the following conditions must hold in a streaming scenario:

1.  $`A \in o(n)`$ as the complete data cannot be stored, hence $`|U| = n`$ is a upper bound for space

2.  Processing time for every item $`s_i`$ must be in $`o(min(t_i))`$ with $`t_i = time(i) - time(i-1)`$ and $`time()`$ being an exact time stamp, to guarantee that data stream items are processed as they arrive

The above conditions can be summarized as $`A \in o(n, t)`$ and may also include $`f(S) \in o(n, t)`$, i.e. the requirement for computations on all items to be sublinear in terms of space and time. These requirements for sublinearity are often specified to use $`polylog`$ complexity instead where $`polylog(n, t) = O(log^k(n, t))`$ for every $`k \in \mathbb{N}`$.

## Common Techniques

To adhere to these requirements it is common for a streaming algorithm to use techniques for reducing the need for space and/or time such as Sampling or Sketching. Random Sampling decreases the load on TCS systems by simply dropping items $`s_i`$ randomly. With a large enough universe $`U`$ this can still yield satisfactory results. In Sketching on the other hand, $`s_i`$’s are not stored directly but are rather used to update a given data structure - also called a *sketch* - that typically contains some form of aggregated statistic or information on $`S`$.

While these and other techniques help making difficult computing problems possible, they also inadvertently introduce an error $`\epsilon`$. Therefore most streaming algorithms cannot provide exact solutions but rather $`1+\epsilon`$ approximations of the problem.
