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


### General Graphs

| Reference   | Value      | Techniques                                     |
|-------------|------------|-------------------------------------------|
| [[GP19]]({{site.baseurl}}/bib#GP19)    | $$0.649$$     | (Level-1 SoS \| GP rounding)      |
| [[APS25]]({{site.baseurl}}/bib#ALMPS25)    | $$0.674$$       | (Cut Bounds, Match Bound \| Cut State, Match State)   |


## Normalizations and Conventions

### Normalization 
We defined the XY Hamiltonian based on the original definition by [[GP19]]({{site.baseurl}}/bib#GP19) with an overall normalization factor of $$1/2$$. This was originally chosen to match the energy of computational basis states between QMC, XY, and MaxCut. We are unaware of any other conventions currently used in the literature.

### Physics conventions
In statistical mechanics, XY is known as the *antiferromagnetic XY model*, and the problem is presented as:

Minimize
\begin{align}
H &= \sum_{(i,j) \in E} w_{ij} \left(X_i \otimes X_j + Y_i \otimes Y_j \right)  \, 
\end{align}

Note that inverting the sign is accompanied by changing from maximization to minimization, yielding an equivalent problem. However, dropping the identity term does yield to a change in approximability. Thus, sometimes the identity term is reintroduced for consistency (with an appropriate negative sign). 

## Upper Bounds

Similar upper bounds presented in [$$QMC$$](({{site.baseurl}}/problems/QMC)) hold for XY, as demonstrated in [ALMPS25]({{site.baseurl}}/bib#ALMPS25).


## Average Case

There have been two papers analyzing the average-case energy of algorithms for XY on $$D$$-regular graphs. [[MSS25]]({{site.baseurl}}/bib#MSS24), [[KKZ24]]({{site.baseurl}}/bib#KKZ24). Both of these papers give variational algorithms for unweighted $$D$$-regular graphs with provable average-case energy guarantees in the infinite size limit. 


## Remarks

* Unlike EPR and QMC, terms $$h^{XY}$$ are not positive semi-definite.
* $$H^{XY}_G$$ can be represented by the union of the adjacency matrices of the *weighted token graphs* of $$G$$ $$\lfloor|V|/2\rfloor$$ [[APS25]]({{site.baseurl}}/bib#APS25). Under this perspective XY can be seen as a spectral graph theory problem on exponentially sized matices. 


## Conjectures / Open Questions
* Can we prove or disprove
$$\|H^{EPR}_G\| \le W + M/2\,?$$ Supporting numerical evidence is given in  [[APS25]]({{site.baseurl}}/bib#APS25) and [[TZ25b]]({{site.baseurl}}/bib#TZ25b). 
* Are there any hardness of approximability results for XY analagous to the $$0.956$$ barrier for QMC?


<div style="padding-bottom: 300px"></div>
