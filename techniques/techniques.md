---
type: technique
---

# Techniques

We judge an approximation by its *approximation ratio*

Let $$\lambda_{max}(H_G)$$ denote the maximum energy of some Hamiltonian $$H_G$$. Suppose we can find efficiently computable functions $$\ell$$, $$u$$ such that for all graphs $$G$$

$$
0 \le \ell(G) \le \lambda_{max}(H_G) \le  u(G)\,.
$$

Then, the approximation ratio $$\alpha$$ is at least

$$
\alpha \ge \min_G \frac{\ell(G)}{\lambda_{max}(H_G)} \ge  \min_G \frac{\ell(G)}{u(G)}\, \quad\quad (1).
$$

In practice, the lower bound $$\ell$$ is provided by an *algorithm* that achieves at least energy $$\ell(G)$$ on a graph $$G$$. Thus, $$\alpha$$ gives us a guarantee on the fraction of the optimal energy that an algorithm can achieve.

In order to obtain better approximation ratios then, there are a few options 

1. Lower Bounds: find better algorithms, leading to larger $$\ell(G)$$
2. Upper Bounds: find better (tighter) upper bounds $$u(g)$$
3. Analysis: Eq. (1) involves a minimization problem over the uncountably infinite set of weighted graphs. Somehow we need to make this minimization tractable by finding better ways to relate $$\ell(G)$$ and $$u(G)$$ in analysis.

We have a dedicated page for each of these, which can be explored by following the corresponding links.
