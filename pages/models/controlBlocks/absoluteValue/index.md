---
layout: page
title: Absolute value of input
tags: ["control block", "discrete"]
---

## Diagram

$$  x_j = |x_i | $$

![absDiagram](/pages/models/controlBlocks/absoluteValue/absDiagram.svg)

Syntax:  

- input state : $x_i$
- output state: $x_j$
- function abs

Internal states : none

Discrete variable :
$$ z \in \{-1,1\} $$

## Equations

$$
0 = \left\{
    \begin{array}{ll}
        x_j - x_i & if & z=1 \\
        x_i - x_j & if & z=-1
    \end{array}
\right.
$$

## Discrete transitions

```

if z = 1 then
    if xi < 0 then
        z ← −1
    end if
else
    if xi > 0 then
        z ← 1
    end if
end if
```

## Initialization of discrete variables

```

if xi > 0 then
z ← 1
else
z ← 0
end if
```