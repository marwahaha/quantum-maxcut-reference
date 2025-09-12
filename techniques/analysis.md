---
type: technique
---


There are a few ways to relate lower and upper bounds, leading to improved approximation ratios.

## Minimization over graph invariants

Suppose there exists some efficiently-computable function $$b$$ that maps a graph $$G$$ to a real number $$b(G)$$. If $$b(G)$$ doesn't depend on the names of vertices (i.e. it is identical for isomorphic graphs), then the function $$b$$ encodes a *graph invariant*. 

Now, consider that the lower and upper bounds $$\ell(G)$$ and $$u(G)$$ can be written *in terms* of this invariant $$b(G)$$. That is we have $$u(G) = u'(b(G))$$ and $$\ell(G) = \ell'(b(G))$$.

The approximation ratio $$\alpha$$ can then be expressed as 

$$
\alpha \ge  \min_G \frac{\ell(G)}{u(G)} = \min_G \frac{\ell'(b(G))}{u'(b(G))} = \min_{b'\in B} \frac{\ell'(b')}{u'(b')}\,,
$$

where $$B$$ is the codomain of $$b$$. In the last line, we notice that instead of optimizing over the set of all weighted graphs, we can reduce to an optimization over the *codomain b*, which is in most cases a simple set such as real numbers. We may then numerically or analytically minimize over $$b'$$ in order to obtain an approximation ratio.

**Example**: As a simple example for QMC, consider the graph invariant of a *maximum weighted matching*. In the lower bounds section (Match State), we show that we can always prepare a state with energy 

$$
\ell(G) = \frac{3M(G)+W(G)}{2}\,,
$$

where $$M(G)$$ is the weight of the maximum matching and $$W(G)$$ is the total weight of the graph.

In the upper bounds section (Match Bound), we conjecture that the maximum energy is at most

$$
u(G) = W(G)+M(G)\,.
$$

Thus we can compute

$$
\alpha \ge  \min_G \frac{\frac{3M(G)+W(G)}{2}}{ W(G)+M(G)} = \min_{m\in [0,1]} \frac{\frac{3m+1}{2}}{1+m},
$$

where in the last line we divide the numerator and denominator by $$W(G)$$ and use $$m = M(G)/W(G)$$. We have thus reduced the minimization into that over a closed, continuous interval. We then compute the minimum is $$1/2$$ at $$m=0$$, so under this conjecture the algorithm of preparing a Match State achieves a $$1/2$$-approximation. 

Other examples of graph invariants include properties as simple as the total weight or number of vertices (used in e.g. [[AGM20]]({{site.baseurl}}/bib#AGM20)), or as complicated as the optimized SDP values of the quantum moment-SoS hierarchy as in [[GP19]]({{site.baseurl}}/bib#GP19)).


## Reduction to worst-case edge

Suppose that not only do we have some efficiently-computable graph invariant $$b$$, but that this invariant returns a value *for each edge in the graph*. Then, suppose our lower and upper bounds $$\ell(G)$$ and $$u(G)$$ are given as the sum of the bounds for each edge

$$
\ell(G) = \sum_{e\in E(G)}\, w_e\, \ell(b(e))\,,
$$

where $$\ell(b(e))$$ is the lower bound on the energy of *a specific edge $$e$$* and $$E(G)$$, $$w_e$$ are the edges and edge weights of $$G$$.


 We then have

$$
\alpha \ge  \min_G \frac{\sum_{e\in E(G)}\, w_e\, \ell(b(e))}{\sum_{e\in E(G)}\, w_e \,u(b(e))}\,.
$$

Since the ratio is at least the ratio of the worst edge, we can then write

$$
\alpha \ge  \min_e \frac{\ell(b(e))}{u(b(e))}\,.
$$

Then, much like in the previous case, we can directly sum over the codomain of $$b$$

$$
\alpha \ge  \min_{b \in B} \frac{\ell(b)}{u(b)}\,.
$$

This minimization problem is then tractable.

**Example**: One example of a worst-case reduction is given in  [[ALMPS25]]({{site.baseurl}}/bib#ALMPS25), Sec 4.1. There, the edge-wise invariant $$b(e)$$ is the value of a fractional matching on an edge. The lower bounds $$\ell(b(e))$$ follow from an analysis of AGM circuits via monogamy of entanglement and the upper bounds $$u(b(e))$$ follow from the fractional matching bound.


## Combining bounds

In certain cases, there may be multiple lower and upper bounds for every graph, which each depend on a different graph invariant. Say we have the lower bounds

$$
\ell_1(b_1(G)), \; \ell_2(b_2(G)), \;\ldots\;, \;\ell_k(b_k(G))\,,
$$

and similarly for upper bounds, where $$b_1$$ through $$b_k$$ are $$k$$ different graph invariants. We can then write 

$$
\alpha \ge  \min_G \frac{\text{max}\,(\ell_1(b_1(G)), \; \ell_2(b_2(G)), \;\ldots\;, \;\ell_k(b_k(G)))}{ \text{min}\,(u_1(b_1(G)), \; u_2(b_2(G)), \;\ldots\;, \;u_k(b_k(G)))} \, .
$$

We can then, as before, replace the minimization with


$$
\alpha \ge  \min_{b_1, b_2,\ldots, b_k} \frac{\text{max}\,(\ell_1(b_1, \; \ell_2(b_2), \;\ldots\;, \;\ell_k(b_k))}{ \text{min}\,(u_1(b_1), \; u_2(b_2), \;\ldots\;, \;u_k(b_k))} \, .
$$

In some cases this leads to tighter bounds. For instance, see [[ALMPS25]]({{site.baseurl}}/bib#ALMPS25), Eq. 52 for XY, which uses maximum weighted matchings and maximum weighted cuts as graph invariants.
