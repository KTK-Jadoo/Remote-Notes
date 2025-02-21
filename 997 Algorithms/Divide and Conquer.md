---
tags:
  - algorithms
date: 2024-09-20
source: "[[COMPSCI 170]]"
---
# Divide and Conquer Algorithms

The divide-and-conquer strategy solves a problem by:
1. Breaking it into subproblems that are themselves smaller instances of the same type of problem
2. Recursively solving these subproblems
3. Appropriately combining their answers

The real work is done piecemeal, in three different places: in the partitioning of problems into subproblems; at the very tail end of the recursion, when the subproblems are so small that they are solved outright; and in the gluing together of partial answers. Recurrence relations are very important to calculation runtime in most DNC problems. [[Master Theorem]] can be used to solve most recurrence relations.

### Solving Recurrences
Example 1: $T(n) = T(n-1) + \sqrt{n}$
(Unroll the recursion)

$$
T(n) = T(n-1) + \sqrt{n}
$$
$$
= T(n-2) + \sqrt{n-1} + \sqrt{n}
$$
$$
= T(1) + \sqrt{2} + \sqrt{3} + \dots + \sqrt{n}
$$

- Each term $\leq \sqrt{n}$, so:
  $$
  T(n) \leq n \cdot \sqrt{n} = n^{1.5}
  $$

- Consider the last $\frac{n}{2}$ terms:
  $$
  T(n) \geq \sqrt{\frac{n}{2}} + \sqrt{\frac{n}{2} + 1} + \dots + \sqrt{n}
  $$
  $$
  \geq \frac{n}{2} \cdot \sqrt{\frac{n}{2}} = \frac{n^{1.5}}{2\sqrt{2}}
  $$

So:
$$
n^{1.5} \geq T(n) \geq \frac{n^{1.5}}{2\sqrt{2}}
$$

Example 2: $T(n) = 2T\left(\frac{n}{3}\right) + n$
(Unroll the recursion)
$$
T(n) = 2 \left[T\left(\frac{n}{3}\right)\right] + n
$$
$$
= 2 \left[ 2T\left(\frac{n}{9}\right) + \frac{n}{3} \right] + n
$$
$$
= 2^2 T\left(\frac{n}{3^2}\right) + 2 \cdot \frac{n}{3} + n
$$
$$
= 2^3 T\left(\frac{n}{3^3}\right) + 2^2 \cdot \frac{n}{3^2} + 2 \cdot \frac{n}{3} + n
$$
$$
= 2^k T\left(\frac{n}{3^k}\right) + \left( \frac{2^k n}{3^{k-1}} + \dots + \frac{2n}{3} + n \right)
$$
Let $k = \log_3 n$ (so $3^k = n$):
$$
T(n) = 2^{\log_3 n} + \Theta(n)
$$
$$
= n^{\log_3 2} + \Theta(n)
$$
$$
= \Theta(n)
$$


## Matrix Multiplication

For two $n \times n$ matrices, $X$ and $Y$, we want to compute $Z=XY$. To compute the entry $Z_{i,j}$,
$$
Z_{i, j} = \sum_{k = 1}^{n} X_{i, k} Y_{k, j}
$$
__Naive Approach__: Calculate $Z_{i,j}$ by calculating the inner product, which is $O(n)$. Then, their are $n^{2}$ entries in the matrix, so: $O(n^3)$. Using a [[Divide and Conquer|Divide and Conquer]] Strategy, divide the original matrices in 4 parts, int 8 $\frac{n}{2} \times \frac{n}{2}$ matrices.

![[Screenshot 2024-09-20 at 11.43.37 PM.png]]

### Strassen's Algorithm
_Matrix-Multiply_ (X, Y : n × n matrices)
- Think of $X, Y$ as a $2 \times 2$ array of $\frac{n}{2} \times \frac{n}{2}$ matrices:
$$
X = \begin{bmatrix} A & B \\ C & D \end{bmatrix}, \quad
Y = \begin{bmatrix} E & F \\ G & H \end{bmatrix}
$$
- Matrix product $XY$:

$$
XY = \begin{bmatrix} AE + BG & AF + BH \\ CE + DG & CF + DH \end{bmatrix}
$$
- Compute matrix products (using recursion):
$$
P_1 = A(F - H), \quad P_2 = (A + B)H, \quad P_3 = (C + D)E
$$
$$
P_4 = D(G - E), \quad P_5 = (A + D)(E + H), \quad P_6 = (B - D)(G + H), \quad P_7 = (A - C)(E + F)
$$
- Output:
$$
\begin{bmatrix}
P_5 + P_4 - P_2 + P_6 & P_1 + P_2 \\
P_3 + P_4 & P_1 + P_5 - P_3 - P_7
\end{bmatrix}
$$

