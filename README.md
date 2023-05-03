# What is "quantum" Max-Cut?

It is a long-standing goal to find problems with **rigorous** quantum advantage.
In classical optimization problems, evidence is dubious; however, when *the output is a quantum state*, there is (intuitively) more hope.

Here we describe a few quantum Hamiltonians, along with associated algorithms and known hardness results.

## The Hamiltonians

Recall that we use Pauli matrices $$P \in \{I, X, Y, Z\}$$, and $$P_i$$ means the Pauli matrix $$P$$ at site $$i$$.
There are also fermionic Hamiltonians, but I will ignore those here.

#### Quantum Max-Cut
This is defined by a graph $$G(V,E)$$, where 

$$
H = \frac{1}{2} \sum_{(i,j) \in E} I_i \otimes I_j - X_i \otimes X_j - Y_i \otimes Y_j - Z_i \otimes Z_j
$$

Alternatively, this can be written as $$H = \sum_{(i,j) \in E} 2 \vert \psi^{-} \rangle  \langle \psi^{-} \vert $$, where $$\vert \psi^{-} \rangle = \frac{1}{\sqrt{2}} (\vert 01\rangle - \vert 10\rangle)$$ is the singlet state. Note that this Hamiltonian is rotation-invariant, and the minimum energy is zero.

This Hamiltonian can optionally be weighted with coefficients $$w_{ij}$$.

#### EPR Hamiltonian
I think this term is due to [[King22]](https://arxiv.org/pdf/2209.02589.pdf). This is defined also by a graph $$G(V,E)$$, where

$$
H = \frac{1}{2} \sum_{(i,j) \in E} I_i \otimes I_j + X_i \otimes X_j - Y_i \otimes Y_j + Z_i \otimes Z_j
$$

Alternatively, this can be written as $$H = \sum_{(i,j) \in E} \vert \phi^+\rangle \langle \phi^+\vert $$, where $$\vert \phi^+\rangle = \frac{1}{\sqrt{2}}(\vert 00 \rangle + \vert 11 \rangle)$$ is the EPR pair. Note that this Hamiltonian has a particularly simple optimal product state of $$\vert 0 \rangle^{\otimes n}$$ with approximation ratio $$\frac{1}{2}$$.

This Hamiltonian can also optionally be weighted with coefficients $$w_{ij}$$.

#### Wigner GUE
This is a random family of Hamiltonians of the form

$$
H = \frac{1}{2^n} \sum_{\vec{P} \in \{I, X, Y, Z\}^n} g_{\vec{P}} \vec{P}
$$

where an instance is drawn by choosing each $$g_{\vec{P}} \sim \mathcal{N}(0,1)$$ i.i.d. 

There is a variant of this model (let's call it a **Sparse Wigner** Hamiltonian) with similar eigenvalue statistics, famously studied in [[CDBBT23]](https://arxiv.org/pdf/2302.03394.pdf).

#### Quantum spin glasses
There are a few ways to describe a random family of quantum spin glasses:

* One way is to restrict the **Wigner GUE** to random $$k$$-local Pauli Hamiltonians (i.e. terms with exactly $$k$$ non-identity Pauli matrices). This is studied for example in [[ES14]](https://arxiv.org/pdf/1407.1552.pdf). 
* Another way is to restrict the **Wigner GUE** to terms of the form $$P_{i_1} \otimes \dots \otimes P_{i_k}$$ where $$P$$ is the same Pauli matrix within each term. I'm not sure this has been exactly studied, but it was claimed in [[CDBBT23]](https://arxiv.org/pdf/2302.03394.pdf) in "Related Models"
* Yet another way is to use all terms of the form $$g_{ij} (X_i \otimes X_j + Y_i \otimes Y_j + Z_i \otimes Z_j)$$ where $$g_{ij}$$ is an i.i.d. standard Gaussian. This might most closely resemble the quantum Max-Cut Hamiltonian.


## Algorithms

### Worst-case algorithms
Most algorithms have been studied for **Quantum Max-Cut**. A recent summary is given in Section 3 of [[King22]](https://arxiv.org/pdf/2209.02589.pdf).

The classical algorithms:

* The first thing you might try is writing down a SDP (semidefinite program) and round the solution to a product state. How we do this is by optimizing over "pseudo-states" on each qubit, and rounding to a global solution. But this algorithm only gives an approximation ratio of $$0.498$$ [[GP19]](https://arxiv.org/abs/1909.08846); and assuming a conjecture about Gaussian correlations, this is optimal for the level-1 SDP [[HNPTW22]](https://arxiv.org/abs/2111.01254). (This SDP technique generalized to other local Hamiltonian problems in [[PT20]](https://arxiv.org/pdf/2012.12347.pdf).)
* SDPs are sometimes more powerful when you use more of the system. The more general level-$$d$$$ SDP (or level-$$d$$ Lasserre hierarchy) optimizes over "pseudo-states" over $$d$$-local operators (and smaller), and then round to a global solution. So far, only the level-$$2$$ SDP has been studied; and this in fact gives an optimal $$0.5$$ approximation ratio for a product state [[PT22]](https://arxiv.org/abs/2206.08342). The main intuition is that level-$$2$$ SDPs capture monogamy-of-entanglement. 
* There are also ways to use the level-$$2$$ SDP to round to a *slightly entangled* state; this can go beyond $$0.5$$ approximation ratio. See [[AGM20]](https://arxiv.org/abs/2003.14394) (extended to arbitrary 2-local Hamiltonians in [[AGMS21]](https://arxiv.org/abs/2105.01193)) which achieves $$0.531$$ via tensor network decompositions of shallow quantum circuits, and then [[PT21]](https://arxiv.org/abs/2105.05698) which achieves $$0.533$$ via the level-$$2$$ SDP.

All algorithms beyond this use some classical computation, then *a variational circuit*. As a result, the best quantum algorithms outperform known classical algorithms.
* [[AGM20]](https://arxiv.org/abs/2003.14394) (extended to arbitrary 2-local Hamiltonians in [[AGMS21]](https://arxiv.org/abs/2105.01193)) runs a shallow quantum circuits on top of a product state, which works when the graph is low-degree.
* Recent work [[Lee22]](https://arxiv.org/pdf/2209.00789.pdf), [[King22]](https://arxiv.org/pdf/2209.02589.pdf) use a product state approximation, and apply a more general variational quantum circuit to reach $$0.562$$ (and $$0.582$$ on triangle-free graphs). Notably, they use an SDP *again* to optimize the variational circuit parameters. [[King22]](https://arxiv.org/pdf/2209.02589.pdf) also analyzes their algorithm on **EPR Hamiltonian**; since the optimal product-state is simply $$\vert 0\rangle^{\otimes n}$$, one can more carefully analyze the relative strength of algorithms *beyond* rounding.

This is the state-of-the-art that I am aware of; but there is no claim anywhere that these are the best possible classical or quantum algorithms.

Some useful tools here are the Lasserre hierarchy (or Sum-of-Squares) and rounding techniques, rounding *beyond* product states, and analyzing simple variational circuits.

### Average-case algorithms

As far as I'm aware, the only model that has been studied in the average-case is for the **Sparse Wigner GUE** [[CDBBT23]](https://arxiv.org/abs/2302.03394); since the eigenvalue distribution does not have a long tail, the naive "use phase estimation on a mixed state" provably reaches the optimal solution (whp). The eigenvalue distribution also holds for **Wigner GUE**, but it's not possible to even describe this input model with a polynomial number of terms, so the latter does not have an efficient algorithm.

It's possible to study algorithms for **Quantum spin glasses** and even **Quantum Max-Cut** on random regular graphs, but I haven't seen this done before.

## Hardness results

### Worst-case hardness

Deciding the ground state value of **Quantum Max-Cut** is QMA-hard (I can't actually find a reference that exactly matches this, perhaps [[CM13]](https://arxiv.org/pdf/1311.3161.pdf)? But it's only with arbitrary weights.) It might actually be QMA$$_1$$-hard without weights, I'm not sure.

However, deciding the ground state value of **EPR Hamiltonian** (with arbitrary weights) is StoqMA-hard [[King22]](https://arxiv.org/pdf/2209.00789.pdf). In other words, this Hamiltonian is "sign-problem free".


Note that for both **Quantum Max-Cut** and **EPR Hamiltonian**, product state solutions cannot have approximation ratio higher than $$0.5$$; for example, consider a graph with one edge. But this does not imply a bound on all classical algorithms.

There is one attempt at proving hardness-of-approximation (in particular, by classical algorithms). Assuming a conjecture about Gaussian correlations, [[HNPTW22]](https://arxiv.org/abs/2111.01254) shows that approximating **Quantum Max-Cut** with approximation ratio higher than $$0.956$$ is as hard as deciding the Unique Games problem (this is possibly NP-hard).

Proving quantum-hardness-of-approximation would require some quantum variant of a PCP theorem.

### Average-case hardness

Note that the Hamiltonians above can be studied in an average-case setting if we randomize the choice of graph; say, from $$\mathbf{G}(n,p)$$ or random $$d$$-regular graphs. In this case, we don't know if the optimal value concentrates, or even the right order of magnitude of the optimal value.

Random families of Hamiltonians *only* have a sense of average-case hardness. For the **Wigner GUE** (and sparse variant by [[CDBBT23]](https://arxiv.org/pdf/2302.03394.pdf)), the eigenvalue statistics are known to follow a semicircle law. As a result, the sparse variant is "quantumly easy", although they show a quantum circuit lower bound(!) A classical hardness result is not known.

For **Quantum spin glasses**,  [[ES14]](https://arxiv.org/pdf/1407.1552.pdf) describes the eigenvalue statistics (which either have Gaussian tails or follow a semicircle law). In the Gaussian case, I don't think the location of the optimal value is known. A natural question is if **Quantum spin glasses** have an optimal value related to a random family of **Quantum Max-Cut**, as is true in the classical setting (see [[Panchenko14 notes]](https://arxiv.org/abs/1412.0170), [[DMS17]](https://arxiv.org/abs/1503.03923), and [spin-glass-reference](https://marwahaha.github.io/spin-glass-reference/)).

There may be ways to extend recent average-case obstructions from *classical* spin glass theory to these quantum spin glasses. Notably, a property of the covariance matrices of nearly-optimal solutions (the *Overlap Gap Property* or (OGP)) obstructs certain algorithms from achieving the optimal solution in the average-case. See [[Gamarnik21 survey]](https://arxiv.org/pdf/2109.14409.pdf), and the original paper [[GS14]](https://arxiv.org/pdf/1304.1831.pdf) introducing the concept. The [spin-glass-reference](https://marwahaha.github.io/spin-glass-reference/) may also have some explainers. There are several kinds of OGPs (multi-, ensemble, [branching](https://arxiv.org/pdf/2110.07847.pdf), ...) that have different obstruction strengths, and may obstruct different classes of algorithms. See also [[AGK23]](https://arxiv.org/pdf/2304.00643.pdf) as viewing the OGP from the perspective of NLTS.

Some useful tools here include textbook random matrix theory ideas, like the trace power method and concentration bounds, as well as recent work in proving OGPs for spin glasses.


## Some other references

* [almost sure convergence in quantum spin glasses](https://arxiv.org/pdf/1508.01785.pdf)
* [paper by Hastings-O'Donnell](https://arxiv.org/pdf/2110.10701.pdf) on fermionic models; some others [paper1](https://link.springer.com/article/10.1007/JHEP07(2018)124), [paper2](https://link.springer.com/article/10.1007/s42543-018-0007-1) on concentration of SYK

---

Inspired by conversations with many people, including Bobak Kiani, Joao Basso, Robbie King, Nathan Ju, James Sud, and Adrian She.

More to add? Email me at [kmarw@uchicago.edu](mailto:kmarw@uchicago.edu).
