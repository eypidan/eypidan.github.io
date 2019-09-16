---
layout: post
title:  "Common knowledge in ML"
author: "eypidan"
---
<style>
.tablelines table, .tablelines td, .tablelines th {
        border: 1px solid black;
        }
</style>
## Loss Function

- Cross-Entropy

### Cross-Entropy

- Cross-entropy loss : log loss

- ```python
  def CrossEntropy(yHat, y):
      if y == 1:
        return -log(yHat)
      else:
        return -log(1 - yHat)
  ```

- Math

  - if classes M = 2

    $$cross-entropy = -(y\log(p)+(1-y))\log(1-p)$$

  - if classes M > 2
    $$cross-entropy  =  \sum_{c=1}^M{y_{o,c}log(p_{o,c})}$$

    - p - predicted probability observation o is of class c
    - y - binary indicator (0 or 1) if class label c is the correct classification for observation o





## Network layer

- Softmax layer

### Softmax layer
- Softmax turns arbitrary real values into probabilities

- Softmax performs the following transform on n numbers:$x_1...x_n:$

  $$ s(x_i) = \frac{e^{x_i}}{\sum_{j=1}^ne^{x_j}} $$
  
  - The outputs of the Softmax transform are always in the range [0, 1][0,1] and add up to 1. Hence, they form a **probability distribution**.
  
- A example: a network would have it ouput 2 real number, one representing dog and the other cat, and apply Softmax on these values. Now this network outputs is [-1,2]. (example from [victorzhou.blog](https://victorzhou.com/blog/softmax/))

| Animal | x    | $e^x$ | Probability |
| ------ | ---- | ----- | ----------- |
| dog    | -1   | 0.368 | 0.047       |
| cat    | 2    | 7.39  | 0.953       |
{: .tablelines}
