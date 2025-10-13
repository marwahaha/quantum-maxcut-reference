---
type: problem
---

# XY

## Definition

Given a graph $$G(V,E,w)$$ with vertex set $$V$$, edge set $$E$$, and nonnegative edge weights $$w$$, the XY Hamiltonian is defined as:

$$
H^{XY}_G =  \sum_{(i,j) \in E} w_{ij}\, h^{XY}_{ij}
$$

$$
h^{XY}_{ij}  :=\frac{1}{2}\left( I_i \otimes I_j - X_i \otimes X_j - Y_i \otimes Y_j \right)
$$


## Hardness
* QMA-complete for positive-weighted graphs [[PM15]]({{site.baseurl}}/bib#PM15).

## Algorithms 

We present all known approximation algorithms for EPR. We concisely summarize techniques with the following notation and link to the appropriate [Techniques]({{site.baseurl}}/techniques).

* ( $$u$$ \| $$\ell$$ ) denotes an upper bound $$u$$ and a corresponding lower bound $$\ell$$. 

| Reference   | Value      | Techniques                                     |
|-------------|------------|-------------------------------------------|
| [[GP19]]({{site.baseurl}}/bib#GP19)    | $$0.649$$     | (Level-1 SoS \| GP rounding)      |
| [[APS25]]({{site.baseurl}}/bib#ALMPS25)    | $$0.674$$       | (Cut Bound, Match Bound \| Cut State, Match State)   |


## Normalizations and conventions

### Normalization 
We defined the XY Hamiltonian based on the original definition by [[GP19]]({{site.baseurl}}/bib#GP19) with an overall normalization factor of $$1/2$$. This was originally chosen to match the energy of computational basis states between QMC, XY, and MaxCut. We are unaware of any other conventions currently used in the literature.

### Physics conventions
In statistical mechanics, XY is known as the *antiferromagnetic XY model*, and the problem is presented as:

Minimize

$$
H &= \sum_{(i,j) \in E} w_{ij} \left(X_i \otimes X_j + Y_i \otimes Y_j \right).  
$$

Note that inverting the sign is accompanied by changing from maximization to minimization, yielding an equivalent problem. However, dropping the identity term does yield to a change in approximability. Thus, sometimes the identity term is reintroduced for consistency (with an appropriate negative sign). 


## Known cases

### Complete graphs

It is shown in (among other places [[APS25]]({{site.baseurl}}/bib#APS25)) the maximum energy of unweighted complete graphs $$K_n$$ is given by

$$ \|H^{XY}_{K_n}\| =    \begin{cases}
                              \frac{n^2+n}{4}, & n \,\text{even}, \\
                              \frac{n^2+n-2}{4}, & n \,\text{odd}. 
                              \end{cases}
$$


### Small paths and cycles

The maximum energies of small paths and cycles was computed in [[APS25]]({{site.baseurl}}/bib#APS25) is reported below.

|  Graph  | $$n=3$$ | $$n=4$$ | $$n=5$$ | $$n=6$$ | $$n=7$$ | $$n=8$$ | $$n=9$$ | $$n=10$$ | $$n=11$$ | $$n=12$$ | $$n=13$$ | $$n=14$$ | $$n=15$$ |
|---------|---------|---------|---------|---------|---------|---------|---------|----------|----------|----------|----------|----------|----------|     
| $$C_n$$ | $$2.5000$$ | $$4.8284$$ | $$5.1180$$ | $$7.0000$$ | $$7.5489$$ | $$9.2263$$ | $$9.9115$$ | $$11.4721$$ | $$12.2420$$ | $$13.7274$$ | $$14.5552$$ | $$15.9879$$ | $$16.8577$$ |
| $$P_n$$ | $$2.4142$$ | $$3.7361$$ | $$4.7321$$ | $$5.9940$$ | $$7.0273$$ | $$8.2588$$ | $$9.3138$$ | $$10.5267$$ | $$11.5958$$ | $$12.7962$$ | $$13.8752$$ | $$15.0668$$ | $$16.1532$$ |


## Classical formulation

It is shown in [[APS25]]({{site.baseurl}}/bib#APS25) [Fact 2] that the XY Hamiltonian on a graph $$G$$ can be written as an affine transformation of a direct sum of the adjacency matrices of the *token graphs* $$F_k(G)$$ of $$G$$ with $$0 \le k \le n$$. Furthermore, the eigenvalues follow

$$
  \text{eigs}(H^{XY}_G) = \bigcup_{0 \leq k \leq \lfloor\frac{n}{2}\rfloor} \text{eigs}(\frac{W}{2} I - A(F_k(G)).
$$

For more information and detail, see [Token Graphs]({{site.baseurl}}/techniques/token_graphs.html) and [[APS25]]({{site.baseurl}}/bib#APS25).


## Average case

There have been two papers analyzing the average-case energy of algorithms for XY on $$D$$-regular graphs. [[MSS25]]({{site.baseurl}}/bib#MSS24), [[KKZ24]]({{site.baseurl}}/bib#KKZ24). Both of these papers give variational algorithms for unweighted $$D$$-regular graphs with provable average-case energy guarantees in the infinite size limit. 


## Remarks

* Unlike EPR and QMC, terms $$h^{XY}$$ are not positive semi-definite.
* $$H^{XY}_G$$ can be represented by the union of the adjacency matrices of the *weighted token graphs* of $$G$$ $$\lfloor V /2\rfloor$$ [[APS25]]({{site.baseurl}}/bib#APS25). Under this perspective XY can be seen as a spectral graph theory problem on exponentially sized matices. 




<div style="padding-bottom: 300px"></div>
