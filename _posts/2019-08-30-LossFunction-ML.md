---
layout: post
title:  "Loss Function in ML"
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





