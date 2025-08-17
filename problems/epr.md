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


### General Graphs

| Reference   | Value      | Techniques                                     |
|-------------|------------|-------------------------------------------|
| [[Kin23]]({{site.baseurl}}/bib#Kin23)    | $$\frac{1}{\sqrt{2}} \approx 0.707$$      | (Level-2 SoS \| AGM circuits)     |
| [[JN25]]({{site.baseurl}}/bib#JN25)    | $$\frac{1+\sqrt{5}}{2} \approx 0.809$$      | (Star Bound + Computer Proof \| Match States)     |
| [[ALMPS25]]({{site.baseurl}}/bib#ALMPS25)    | $$\frac{1+\sqrt{5}}{2} \approx 0.809$$       | (Star Bound \| AGM circuits)   |


## Normalizations and Conventions

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
\begin{align}
H &= \sum_{(i,j) \in E} w_{ij} \left( - X_i \otimes X_j + Y_i \otimes Y_j - Z_i \otimes Z_j \right), \, 
\end{align}

Note that inverting the sign is accompanied by changing from maximization to minimization, yielding an equivalent problem. However, dropping the identity term does yield to a change in approximability. Thus, sometimes the identity term is reintroduced for consistency (with an appropriate negative sign). 

## Upper Bounds

The same upper bounds presented in [QMC](({{site.baseurl}}/problems/QMC)) apply to EPR with almost identical proofs. The only exception is that the constant $$a$$ in the matching bound has only been shown to be $$11/10$$ in [ALMPS25]({{site.baseurl}}/bib#ALMPS25).

## Average Case

There have been two papers analyzing the average-case energy of algorithms for EPR on $$D$$-regular graphs. [[MSS25]]({{site.baseurl}}/bib#MSS24), [[KKZ24]]({{site.baseurl}}/bib#KKZ24). Both of these papers give variational algorithms for unweighted $$D$$-regular graphs with provable average-case energy guarantees in the infinite size limit. Among other results, both works show that quantum circuits can prepare states with energy within $$2\%$$ error from the optimal for EPR on an infinite ring (also known as the 1D Heisenberg spin chain with periodic boundary conditions).


## Remarks

* The negative sign for EPR can be moved between the $$XX$$, $$YY$$, and $$ZZ$$ terms by simple unitary transformations on each qubit [[APS25]]({{site.baseurl}}/bib#APS25)
* EPR is equivalent to [QMC](({{site.baseurl}}/problems/QMC)) on bipartite graphs under a conjugation of vertices on one side of the partition by $$Y$$ (Eq. 1 [[Kin23]]({{site.baseurl}}/bib#Kin23))
* $$H^{EPR}_G$$ can be represented by the union of the *signless Laplacians* of the *weighted token graphs* of $$G$$ $$\lfloor|V|/2\rfloor$$ [[APS25]]({{site.baseurl}}/bib#APS25). Under this perspective EPR can be seen as a spectral graph theory problem on exponentially sized matices 
* Terms $$h^{EPR}$$ are positive semi-definite


## Conjectures / Open Questions
* Can we prove or disprove
$$\|H^{EPR}_G\| \le W + M \,?$$ Supporting numerical evidence is given in  [[APS25]]({{site.baseurl}}/bib#APS25) and [[TZ25b]]({{site.baseurl}}/bib#TZ25b). 
* Are there polytime algorithms to solve EPR exactly? What is the complexity of EPR?


<div style="padding-bottom: 300px"></div>
