---
type: technique
---

# Token Graphs

The QMC, EPR, and XY Hamiltonians in this reference are defined as *quantum optimization* problems over $$n$$ qubits. These $$n$$ qubits form an exponentially large Hilbert space. One way to think about these problems, then, is as a maximum eigenvalue problem on an exponentially large matrix. This is a fully classical problem, albiet of exponential size. Here we discuss the structure of the exponeitally large matrices, specifically in via the language of *token graphs*. We describe the formalism of token graphs and some results here. For more details, see [[APS25]]({{site.baseurl}}/bib#APS25). 

## Definition

Given a graph $$G(V,E,w)$$, with vertices $$V = [n]$$, edges $$E \in V \times V$$, $$m = |E|$$, and weights $$w\in \mathbb{R}_{\ge 0}^{m}$$ and some integer $$1\le k \le \lfloor n/2\rfloor$$, define the $k$-th token graph of $$G$$ as the graph with

* Vertices: vertices are $$\binom{[n]}{k}$$, the set of $$k$$-tuples of the set $$[n]$$, which contains $$\binom{n}{k}$$ elements.
* Edges: vertices $A$ and $B$ are connected by an edge if and only if their symmetric difference $$A\triangle B =\{a,b\}$$, where $$a\in A$$, $$b\in B$$, and $$(a,b)\in E$$.
* Weights: the weight of edge $$(A,B)$$ is $$w_{ab}$$, where $$a$$ and $$b$$ are defined as above.

## Relation to Hamiltonians

We demonstrate the relation between token graphs and QMC here. This result is folklore, but for a concise explanation we suggest [[APS25]]({{site.baseurl}}/bib#APS25) (Section 3.1)

We first expand the definition of the QMC Hamiltonian in the computational basis:

$$
H^{QMC}(G) = \sum_{(i,j) \in E} 
\big( \lvert 01 \rangle \langle 01 \rvert \big)_{ij}
+ \big( \lvert 10 \rangle \langle 10 \rvert \big)_{ij}
- \big( \lvert 01 \rangle \langle 10 \rvert \big)_{ij}
- \big( \lvert 10 \rangle \langle 01 \rvert \big)_{ij}.
$$

In this form, we see that $$H^{QMC}(G)$$ preserves Hamming weight in the computational basis. Thus, the Hamiltonian can be block-diagonalized into $$n + 1$$ blocks, each corresponding to a Hamming-weight-$$k$$ sector for $$k \in \{0, 1, 2, \ldots, n\}$$.

We may then analyze the action of $$H^{QMC}(G)$$ in a fixed Hamming-weight sector.  
For a subset of vertices $$X \subseteq V$$ of size $$k$$, let $$x \in \{0,1\}^n$$ be the length-$$n$$ bitstring where $$x_i = 1$$ if and only if $$i \in X$$.  
Let $$\lvert x \rangle$$ be the quantum state encoding bitstring $$x$$ in the computational basis.  
Let $$h(x)$$ denote the Hamming weight of $$x$$.  
Then, the $$\binom{n}{k}$$ states

$$
\{\lvert x \rangle : x \in \{0,1\}^n,\, h(x) = k\}
$$

span the Hilbert space of $$n$$-qubit states with fixed Hamming weight $$k$$.

With this notation, the action of $$H^{QMC}(G)$$ on a fixed basis state $$\lvert x \rangle$$ can be expressed as:

$$
H^{QMC}(G)\lvert x \rangle =
\sum_{(i,j) \in E} w_{ij}\, \mathbf{1}\{x_i \neq x_j\}
\big( \lvert x \rangle \langle x \rvert - \lvert x^{(i,j)} \rangle \langle x \rvert \big),
$$

where $$x_i, x_j$$ denote the bits of $$x$$ at indices $$i$$ and $$j$$, and $$x^{(i,j)}$$ denotes the bitstring obtained by interchanging bits $$x_i$$ and $$x_j$$.

We may then compute the inner products:

$$
M^{QMC}_{x,y}(G) = \langle y \lvert H^{QMC}(G) \rvert x \rangle =
\begin{cases}
 \sum_{(i,j) \in E} w_{ij}\, \mathbf{1}\{x_i \ne x_j\}, & y = x, \\
-w_{ij}, & y = x^{(i,j)}.
\end{cases}
$$

In this form, we can see that the diagonal entry $$M^{QMC}_{x,x}(G)$$ is the sum of the weights of edges $$(i,j)$$ for which $$x_i \neq x_j$$.  
The off-diagonal elements are nonzero if and only if the symmetric difference between the sets $$X$$ and $$Y$$ corresponding to $$x$$ and $$y$$ is a pair $$(i,j) \in E$$.  
In that case, the edge in the token graph has weight $$-w_{ij}$$.

This shows that the inner-product matrix $$M^{QMC}_{x,y}(G)$$ corresponds exactly to the Laplacian matrix of the $$k$$-th token graph of $$G$$.  
This result holds for any $$k$$, so the matrix $$H^{QMC}(G)$$ in the computational basis consists of $$(n + 1)$$ blocks indexed by Hamming weight $$k$$, where each block corresponds to the Laplacian $$L(F_k(G))$$.


**Fact:** For any graph $$G$$, the QMC Hamiltonian $$H^{QMC}(G)$$ is equivalent to a direct sum over the Laplacian matrices $$L(F_k(G))$$ of the token graphs $$F_k(G)$$ for $$0 \leq k \leq n$$. Furthermore,

$$
\mathrm{eigs}(H^{QMC}(G)) = 
\bigcup_{0 \leq k \leq \lfloor n / 2 \rfloor} \mathrm{eigs}(L(F_k(G))).
$$

Similar results hold that connect EPR with the *signless Laplacians* of token grpahs and XY with the *adjacency matrices* of token graphs.

## Applications

The token graph formulation is interesting for a few reasons.

* Spectral properties of token graphs have been studied for multiple decades. Thus, results from spectral graph theory literature may be directly applied to quantum optimization. One example of this is that the token graphs of complete graphs are [Johnson Graphs](https://en.wikipedia.org/wiki/Johnson_graph), whose spectra are completely understood.

* Via the token graph formulation, quantum optimization problems can be presented as *purely classical* graph theory problems. This allows for both the study of problems such as QMC by the classical community and for natural classical formulations of $$QMA$$-complete problems.

* Studies into problems such as QMC can lead to insights into token graphs. For example, in [[APS25]]({{site.baseurl}}/bib#APS25) investigations into QMC led to conjectured bounds on the eigenspectra of token graphs, and some partial results towards these conjectures.

* Token graph formulations of problems may lead to new algorithm primitives, such as the Dicke state primitive in [[APS25]]({{site.baseurl}}/bib#APS25) (Section A.5).



