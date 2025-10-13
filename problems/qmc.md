---
type: problem
---

# QMC

## Definition

Given a graph $$G(V,E,w)$$ with vertex set $$V$$, edge set $$E$$, and nonnegative edge weights $$w$$, the Quantum MaxCut (QMC) Hamiltonian is defined as:

$$
H^{QMC}_G =  \sum_{(i,j) \in E} w_{ij}\, h^{QMC}_{ij}
$$

$$
h^{QMC}_{ij} :=\frac{1}{2}\left( I_i \otimes I_j - X_i \otimes X_j - Y_i \otimes Y_j - Z_i \otimes Z_j \right) 
=   2 \,w_{ij} \, \vert \psi^{-} \rangle_{ij}  \langle \psi^{-} \vert_{ij}\,,
$$

where $$\vert \psi^{-} \rangle = \frac{1}{\sqrt{2}} (\vert 01\rangle - \vert 10\rangle),$$ is the singlet state. 


## Hardness
* QMA-complete for positive-weighted graphs [[PM15]]({{site.baseurl}}/bib#PM15).
* Under a conjecture, getting a $$(0.956+\epsilon)$$-approximation is as hard as Unique Games [[HNPTW22]]({{site.baseurl}}/bib#HNPTW22).

## Algorithms 

We present all known approximation algorithms for QMC. We concisely summarize techniques with the following notation and link to the appropriate [Techniques]({{site.baseurl}}/techniques).

* ( $$u$$ \| $$\ell$$ ) denotes an upper bound $$u$$ and a corresponding lower bound $$\ell$$.
*  $$\cup$$ denotes that the algorithm attempts multiple combinations of upper and lower bounds on each graph and outputs the one corresponding to the best approximation ratio.
*  $$a+b$$ denotes the combination of multiple lower or upper bounds.
*  $$a,b$$ denotes the use of two lower bounds or upper bounds in parallel.
*  Threshold: $$a$$, $$b$$ denotes a rounding scheme where values above some threshold are rounded via strategy $$a$$ and the rest are rounded via $$b$$.

### General graphs

| Reference   | Value      | Techniques                                     |
|-------------|------------|-------------------------------------------|
| [[GP19]]({{site.baseurl}}/bib#GP19)    | $$0.498$$     | (Level-1 SoS \| GP Rounding)     |
| [[PT22]]({{site.baseurl}}/bib#PT22)     | $$1/2$$       | (Level-2 SoS \| Threshold: Cut State, GP Rounding) |
| [[HTPG24]]({{site.baseurl}}/bib#HTPG24)     | $$0.526$$      |(SOCP + Level-1 SoS \| Threshold: Match State, GP Rounding) $$\cup$$ (Augmented SOCP \| Modified GP Rounding ) |
| [[AGM20]]({{site.baseurl}}/bib#AGM20)    | $$0.531$$      | (Fractional Match Bound, Cut Bound \| Match State, Cut State, Forest State)    | 
| [[PT21]]({{site.baseurl}}/bib#PT21)      | $$0.533$$      | (Level-2 SoS \| Threshold: Match State, GP Rounding) $$\cup$$ (Level-1 SoS \| GP Rounding) | 
| [[Lee22]]({{site.baseurl}}/bib#Lee22)     | $$0.562$$      | (Relaxed Level-2 SoS \| GW Rounding + AGM Circuit)     | 
| [[LP24]]({{site.baseurl}}/bib#LP24)      | $$0.595$$      | (Level-2 SoS \| GP rounding) $$\cup$$ (Level-2 SoS Match Bound \| Match State)    | 
| [[JKKSW24]]({{site.baseurl}}/bib#JKKSW24)    | $$0.599$$      | Improved analysis of [[LP24]]({{site.baseurl}}/bib#LP24) using level-3 SoS Match Bound                 |
| [[GSS25]]({{site.baseurl}}/bib#GSS25)   | $$0.603$$      |  Improved analysis of [[LP24]]({{site.baseurl}}/bib#LP24) using level-13 SoS Match bound     |
| [[ALMPS25]]({{site.baseurl}}/bib#ALMPS25)  | $$0.611$$     | (Level-13 SoS \| Threshold: Reweighted Partial Match State, GP Rounding) $$\cup$$ (Level-13 SoS \| Match State) | 



### Triangle-free graphs

| Reference   | Value      |  Techniques                                     |
|-------------|------------|---------------------------|
| [[Kin23]]({{site.baseurl}}/bib#Kin23)     | $$0.582$$    |  (Level-2 SoS \| GP Rounding + AGM Circuit) | 
| [[GSS25]]({{site.baseurl}}/bib#GSS25)    | $$0.61383$$  |(Level-2 SoS \| GP Rounding + AGM Circuit) $$\cup$$ (Level-13 SoS $$13$$-Match Bound \| Match State)|



### Bipartite graphs

| Reference   | Value      | Techniques                                     |
|-------------|------------|---------------------------|
| [[LP24]]({{site.baseurl}}/bib#LP24)     | $$0.606$$    | (Level-2 SoS \| GP Rounding) $$\cup$$ (Match Bound \| Match State)    | 
| [[Kin23]]({{site.baseurl}}/bib#Kin23)     | $$\sqrt{1/2} \approx 0.707$$ |  (Level-2 SoS \| Zero State + AGM Circuit) |
| [[ALMPS25]]({{site.baseurl}}/bib#ALMPS25)  | $$\frac{1+\sqrt{5}}{2}\approx 0.809$$       | (Fractional Match Bound \| AGM Circuit) |
| [[JN25]]({{site.baseurl}}/bib#JN25)  | $$\frac{1+\sqrt{5}}{2}\approx 0.809$$        | (Match Bound \| Match State) |
| [[GSS25]]({{site.baseurl}}/bib#GSS25)    | $$0.8162$$       |    (Level-2 SoS \| Cut State + AGM circuit) |



## Normalizations and conventions

### Normalization 
We defined the QMC Hamiltonian with an overall normalization factor of $$1/2$$. This is an arbitrary choice, and simply correspond to multiplicative scalings, which do not affect approximation ratios. Other choices have appeared in the literature; we list some pros and cons of each

| Coefficient   | Pros      | Cons                                     |
|-------------|------------|---------------------------|
| $$1/4$$ | Ensures unit norm of each term | Excess fractions, computational basis states have different energy for QMC and MaxCut |
| $$1/2$$ | Less fractions, computational basis state have same energy for QMC and MaxCut | Each term no longer has unit norm |
| $$1$$ | Minimum fractions | No aligment with MaxCut, each term no longer has unit norm |
| $$1/\|\|H\|\|$$ | Hamiltonian has unit norm | Excess fractions|

In certain cases, the identity term in Eq. (1) is dropped, making the Hamiltonian *traceless*. This changes the approximability of the Hamiltonian (see Sec 1.1 [[GP19]]({{site.baseurl}}/bib#GP19)). 

### Physics conventions
In statistical mechanics, QMC is known as the *antiferromagnetic Heisenberg model*, and the problem is presented as:

Minimize

$$
H = \sum_{(i,j) \in E} w_{ij} \left( X_i \otimes X_j + Y_i \otimes Y_j + Z_i \otimes Z_j \right)
= \sum_{(i,j) \in E} w_{ij} \left( 2\left( S_i^+ S_j^- + S_i^- S_j^+ \right) + Z_i Z_j \right).
$$

Where 

$$S_k^+ = \frac{X_k + i Y_k}{2}, \quad S_k^- = \frac{X_k - i Y_k}{2},$$

are the canonical raising and lowering operators. Note that inverting the sign is accompanied by changing from maximization to minimization, yielding an equivalent problem. However, dropping the identity term does yield to a change in approximability. Thu, sometimes the identity term is reintroduced for consistency (with an appropriate negative sign).

## Known cases

### Complete graphs

It is shown in (among other places [[APS25]]({{site.baseurl}}/bib#APS25)) the maximum energy of unweighted complete graphs $$K_n$$ is given by

$$ \|H^{QMC}_{K_n}\| =    \begin{cases}
                              \frac{n^2+2n}{4}, & n \,\text{even}, \\
                              \frac{n^2+2n-3}{4}, & n \,\text{odd}.
                              \end{cases}
$$

### Complete bipartite graphs

It is shown in (among other places [[APS25]]({{site.baseurl}}/bib#APS25)) the maximum energy of unweighted complete bipartite graphs $$K_{A,B}$$ with $$A \le B$$ being the number of vertices on each side of the partition is given by

$$ \|H^{QMC}_{K_{A,B}}\| =  AB+A. $$

### Small paths and cycles

The maximum energies of small paths and cycles was computed in [[APS25]]({{site.baseurl}}/bib#APS25) is reported below.

|  Graph  | $$n=3$$ | $$n=4$$ | $$n=5$$ | $$n=6$$ | $$n=7$$ | $$n=8$$ | $$n=9$$ | $$n=10$$ | $$n=11$$ | $$n=12$$ | $$n=13$$ | $$n=14$$ | $$n=15$$ |
|---------|---------|---------|---------|---------|---------|---------|---------|----------|----------|----------|----------|----------|----------|     
| $$C_n$$ | $$3.0000$$ | $$6.0000$$ | $$6.2361$$ | $$8.6056$$ | $$9.2104$$ | $$11.3022$$ | $$12.0946$$ | $$14.0309$$ | $$14.9379$$ | $$16.7748$$ | $$17.7592$$ | $$19.5271$$ | $$20.5673$$ |
| $$P_n$$ | $$3.0000$$ | $$4.7321$$ | $$5.8558$$ | $$7.4872$$ | $$8.6725$$ | $$10.2499$$ | $$11.4726$$ | $$13.0161$$ | $$14.2642$$ | $$15.7842$$ | $$17.0506$$ | $$18.5534$$ | $$19.8338$$ |


## Classical formulation

It is shown in [[APS25]]({{site.baseurl}}/bib#APS25)[Fact 1] that the QMC Hamiltonian on a graph $$G$$ can be written as a direct sum of Laplacians of the *token graphs* $$F_k(G)$$ of $$G$$ with $$0 \le k \le n$$. Furthermore, the eigenvalues follow

$$
  \text{eigs}(H^{QMC}_G) = \bigcup_{0 \leq k \leq \lfloor\frac{n}{2}\rfloor} \text{eigs}(L(F_k(G)).
$$

For more information and detail, see [Token Graphs]({{site.baseurl}}/techniques/token_graphs.html) and [[APS25]]({{site.baseurl}}/bib#APS25).


## Average case

There have been two papers analyzing the average-case energy of algorithms for QMC on $$D$$-regular graphs. [[MSS24]]({{site.baseurl}}/bib#MSS24), [[KKZ24]]({{site.baseurl}}/bib#KKZ24). Both of these papers give variational algorithms for unweighted $$D$$-regular graphs with provable average-case energy guarantees in the infinite size limit. Among other results, both works show that quantum circuits can prepare states with energy within $$2\%$$ error from the optimal for QMC on an infinite ring (also known as the 1D Heisenberg spin chain with periodic boundary conditions).


## Remarks

* $$QMC$$ is $$SU(2)$$-symmetric (invariant under the same single qubit rotation applied to all qubits) ([[Tak+23]]({{site.baseurl}}/bib#Tak+23))
* $$QMC$$ is equivalent to [$$EPR$$]({{site.baseurl}}/problems/EPR.html) on bipartite graphs under a conjugation of vertices on one side of the partition by $$Y$$ (Eq. 1 [[Kin23]]({{site.baseurl}}/bib#Kin23))
* $$H^{QMC}_G$$ can be represented by the union of the Laplacians of the *weighted token graphs* of $$G$$ $$\lfloor V/2 \rfloor$$ [[APS25]]({{site.baseurl}}/bib#APS25). Under this perspective QMC can be seen as a spectral graph theory problem on exponentially sized matices. These matrices are a specialization of [Kikuchi matrices](https://lucatrevisan.wordpress.com/2024/04/27/feiges-conjecture-and-the-magic-of-kikuchi-graphs/), typically defined on $$r$$-uniform hypergraphs, to graphs (r=2)
* There is always an optimal state $$\Psi\rangle$$ for $$H^{QMC}_G$$ supported only on computational-basis bistrings with Hamming weight $$\lfloor V /2 \rfloor$$ ([[APS25]]({{site.baseurl}}/bib#APS25))
* Terms $$h^{QMC}$$ are positive semi-definite


<div style="padding-bottom: 300px"></div>
