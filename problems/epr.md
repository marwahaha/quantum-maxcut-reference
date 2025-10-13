---
type: problem
---

# EPR

## Definition

Given a graph $$G(V,E,w)$$ with vertex set $$V$$, edge set $$E$$, and nonnegative edge weights $$w$$, the EPR Hamiltonian is defined as:

$$
H^{EPR}_G =  \sum_{(i,j) \in E} w_{ij}\, h^{EPR}_{ij}
$$

$$
h^{EPR}_{ij} :=\frac{1}{2}\left( I_i \otimes I_j + X_i \otimes X_j - Y_i \otimes Y_j + Z_i \otimes Z_j \right) 
=   2 \,w_{ij} \, \vert \phi^{+} \rangle_{ij}  \langle \phi^{+} \vert_{ij}
$$

where $$\vert \phi^{+} \rangle = \frac{1}{\sqrt{2}} (\vert 00\rangle + \vert 11\rangle),$$ is the EPR state. 


## Hardness
* In StoqMA for positive-weighted graphs [[PM15]]({{site.baseurl}}/bib#PM15).
* No known barriers to approximability.

## Algorithms 

We present all known approximation algorithms for EPR. We concisely summarize techniques with the following notation and link to the appropriate [Techniques]({{site.baseurl}}/techniques).

* ( $$u$$ \| $$\ell$$ ) denotes an upper bound $$u$$ and a corresponding lower bound $$\ell$$. 

| Reference   | Value      | Techniques                                     |
|-------------|------------|-------------------------------------------|
| [[Kin23]]({{site.baseurl}}/bib#Kin23)    | $$\frac{1}{\sqrt{2}} \approx 0.707$$      | (Level-2 SoS \| AGM circuits)     |
| [[JKKSW24]]({{site.baseurl}}/bib#JKKSW24)    | $$\frac{18}{25} = 0.72$$      | (Match bound \| Match States + Zero States)     |
| [[JN25]]({{site.baseurl}}/bib#JN25)    | $$\frac{1+\sqrt{5}}{2} \approx 0.809$$      | (Star Bound + Computer Proof \| Match States)     |
| [[ALMPS25]]({{site.baseurl}}/bib#ALMPS25)    | $$\frac{1+\sqrt{5}}{2} \approx 0.809$$       | (Star Bound \| AGM circuits)   |


## Normalizations and conventions

### Normalization 
We defined the EPR Hamiltonian with an overall normalization factor of $$1/2$$. This is an arbitrary choice, and simply correspond to multiplicative scalings, which do not affect approximation ratios. Other choices have appeared in the literature; we list some pros and cons of each

| Coefficient   | Pros      | Cons                                     |
|-------------|------------|---------------------------|
| $$1/4$$ | Ensures unit norm of each term | Excess fractions, different norm than QMC |
| $$1/2$$ | Less fractions, same norm as QMC | Each term no longer has unit norm |
| $$1$$ | Minimum fractions | Different norm than QMC |
| $$1/\|\|H\|\|$$ | Hamiltonian has unit norm | Excess fractions|

In certain cases, the identity term in Eq. (1) is dropped, making the Hamiltonian *traceless*. This changes the approximability of the Hamiltonian (see Sec 1.1 [[GP19]]({{site.baseurl}}/bib#GP19)). 

### Physics conventions
In statistical mechanics, EPR is known as the *ferromagnetic XXZ model*, and the problem is presented as:

Minimize
$$
H = \sum_{(i,j) \in E} w_{ij} \left( - X_i \otimes X_j + Y_i \otimes Y_j - Z_i \otimes Z_j \right). \, 
$$

Note that inverting the sign is accompanied by changing from maximization to minimization, yielding an equivalent problem. However, dropping the identity term does yield to a change in approximability. Thus, sometimes the identity term is reintroduced for consistency (with an appropriate negative sign). 


## Known cases

### Complete graphs

It is shown in (among other places [[APS25]]({{site.baseurl}}/bib#APS25)) the maximum energy of unweighted complete graphs $$K_n$$ is given by

$$ \|H^{QMC}_{K_n}\| =    \begin{cases}
                              \frac{n^2}{2}, & n \,\text{even}, \\
                              \frac{n^2-1}{2}, & n \,\text{odd}.
                              \end{cases}
$$

### Complete bipartite graphs

It is shown in (among other places [[APS25]]({{site.baseurl}}/bib#APS25)) the maximum energy of unweighted complete bipartite graphs $$K_{A,B}$$ with $$A \le B$$ being the number of vertices on each side of the partition is given by

$$ \|H^{EPR}_{K_{A,B}}\| =  AB+A. $$

### Small paths and cycles

The maximum energies of small paths and cycles was computed in [[APS25]]({{site.baseurl}}/bib#APS25) is reported below.

|  Graph  | $$n=3$$ | $$n=4$$ | $$n=5$$ | $$n=6$$ | $$n=7$$ | $$n=8$$ | $$n=9$$ | $$n=10$$ | $$n=11$$ | $$n=12$$ | $$n=13$$ | $$n=14$$ | $$n=15$$ |
|---------|---------|---------|---------|---------|---------|---------|---------|----------|----------|----------|----------|----------|----------|     
| $$C_n$$ | $$4.0000$$ | $$6.0000$$ | $$6.8284$$ | $$8.6056$$ | $$9.6272$$ | $$11.3022$$ | $$12.4152$$ | $$14.0309$$ | $$15.1979$$ | $$16.7748$$ | $$17.9777$$ | $$19.5271$$ | $$20.7557$$ |
| $$P_n$$ | $$3.0000$$ | $$4.7321$$ | $$5.8558$$ | $$7.4872$$ | $$8.6725$$ | $$10.2499$$ | $$11.4726$$ | $$13.0161$$ | $$14.2642$$ | $$15.7842$$ | $$17.0506$$ | $$18.5534$$ | $$19.8338$$ |


## Classical formulation

It is shown in [[APS25]]({{site.baseurl}}/bib#APS25) [Fact 3] that after a simple unitary transformation, the EPR Hamiltonian on a graph $$G$$ can be written as a direct sum of the *signless Laplacians* $$Q(\cdot)$$ of the *token graphs* $$F_k(G)$$ of $$G$$ with $$0 \le k \le n$$. Furthermore, the eigenvalues follow

$$
  \text{eigs}(H^{EPR}_G) = \bigcup_{0 \leq k \leq \lfloor\frac{n}{2}\rfloor} \text{eigs}(Q(F_k(G)).
$$

For more information and detail, see [Token Graphs]({{site.baseurl}}/techniques/token_graphs.html) and [[APS25]]({{site.baseurl}}/bib#APS25).

## Average case

There have been two papers analyzing the average-case energy of algorithms for EPR on $$D$$-regular graphs. [[MSS25]]({{site.baseurl}}/bib#MSS24), [[KKZ24]]({{site.baseurl}}/bib#KKZ24). Both of these papers give variational algorithms for unweighted $$D$$-regular graphs with provable average-case energy guarantees in the infinite size limit. Among other results, both works show that quantum circuits can prepare states with energy within $$2\%$$ error from the optimal for EPR on an infinite ring (also known as the 1D Heisenberg spin chain with periodic boundary conditions).


## Remarks

* The negative sign for EPR can be moved between the $$XX$$, $$YY$$, and $$ZZ$$ terms by simple unitary transformations on each qubit [[APS25]]({{site.baseurl}}/bib#APS25)
* EPR is equivalent to [QMC](({{site.baseurl}}/problems/QMC)) on bipartite graphs under a conjugation of vertices on one side of the partition by $$Y$$ (Eq. 1 [[Kin23]]({{site.baseurl}}/bib#Kin23))
* $$H^{EPR}_G$$ can be represented by the union of the *signless Laplacians* of the *weighted token graphs* of $$G$$ $$\lfloor V/2\rfloor$$ [[APS25]]({{site.baseurl}}/bib#APS25). Under this perspective EPR can be seen as a spectral graph theory problem on exponentially sized matices 
* Terms $$h^{EPR}$$ are positive semi-definite



<div style="padding-bottom: 300px"></div>
