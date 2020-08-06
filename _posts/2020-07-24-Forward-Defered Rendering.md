---
layout: post
title:  "Forward-Deferred Rendering"
author: "eypidan"

---

<style>
.tablelines table, .tablelines td, .tablelines th {
        border: 1px solid black;
        }
</style>
## Forward and Deferred Rendering

### Forward Rendering

![image-20200724163924525](../assets/image-20200724163924525.png)

#### Advantage

- conveninent to draw with multiple materials
- only one "pass" to draw

#### Disadavantage

- overdraw
- `O(M*L)`, which limited the numbers of lights
- lack of information about depth, which limited the post-processing

### Deferred Rendering

![image-20200724163914010](../assets/image-20200724163914010.png)

Lighting compuations are not done within fragment shader(pixcel processing). 

**G-buffer**:  the collective term of all textures used to store lighting-relevant data for the final lighting pass.

- G-buffer contains:

![image-20200727204507862](../assets/image-20200727204507862.png)

#### Advantage

- Make available lights increase.

- Lighting Computations are only considered on visible fragments.

  

#### Disadavantage

- High bandwidth consumption
- can't use MSAA
- Not suitable for transparent object.



