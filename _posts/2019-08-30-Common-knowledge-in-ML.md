---
layout: post
title:  "Network Layer in ML"
author: "eypidan"
---
<style>
.tablelines table, .tablelines td, .tablelines th {
        border: 1px solid black;
        }
</style>


## Network Layer

- Softmax layer

### Softmax Layer
- Softmax turns arbitrary real values into probabilities

- Softmax performs the following transform on n numbers:$x_1...x_n:$

  $$ s(x_i) = \frac{e^{x_i}}{\sum_{j=1}^ne^{x_j}} $$
  
  - The outputs of the Softmax transform are always in the range [0, 1][0,1] and add up to 1. Hence, they form a **probability distribution**.
  
- A example: a network would have it ouput 2 real number, one representing dog and the other cat, and apply Softmax on these values. Now this network outputs is [-1,2]. (example from [victorzhou.blog](https://victorzhou.com/blog/softmax/))

| Animal | x    | $e^x$ | Probability |
| ------ | ---- | ----- | ----------- |
| dog    | -1   | 0.368 | 0.047       |
| cat    | 2    | 7.39  | 0.953       |

