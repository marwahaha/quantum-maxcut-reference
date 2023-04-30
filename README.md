# What is "quantum" Max-Cut?

It is a long-standing goal to find problems with **rigorous** quantum advantage.
In classical optimization problems, evidence is dubious; however, when *the output is a quantum state*, there is (intuitively) more hope.

This is a collection of notes about Hamiltonians, both algorithms and obstructions for them.

## The Hamiltonians

Recall that we use Pauli matrices $$P \in \{I, X, Y, Z\}$$, and $$P_i$$ means the Pauli matrix $$P$$ at site $$i$$.

**Quantum Max-Cut**: This is defined by a graph $$G(V,E)$$, where 

$$
H = \frac{1}{2} \sum_{(i,j) \in E} I_i \otimes I_j - X_i \otimes X_j - Y_i \otimes Y_j - Z_i \otimes Z_j
$$

Alternatively, this can be written as $$H = \sum_{(i,j) \in E} 2 \vert \psi^{-} \rangle  \langle \psi^{-} \vert $$, where $$\vert \psi^{-} \rangle = \frac{1}{\sqrt{2}} (\vert 01\rangle - \vert 10\rangle)$$ is the singlet state. Note that this Hamiltonian is rotation-invariant, and the minimum energy is zero.

This Hamiltonian can optionally be weighted with coefficients $$w_{ij}$$.

**EPR Hamiltonian**: I think this term is due to [[King22]](https://arxiv.org/pdf/2209.02589.pdf). This is defined also by a graph $$G(V,E)$$, where

$$
H = \frac{1}{2} \sum_{(i,j) \in E} I_i \otimes I_j + X_i \otimes X_j - Y_i \otimes Y_j + Z_i \otimes Z_j
$$

Alternatively, this can be written as $$H = \sum_{(i,j) \in E} \vert \phi^+\rangle \langle \phi^+\vert $$, where $$\vert \phi^+\rangle = \frac{1}{\sqrt{2}}(\vert 00 \rangle + \vert 11 \rangle)$$ is the EPR pair. Note that this Hamiltonian has a particularly simple optimal product state of $$\vert 0 \rangle^{\otimes n}$$ with approximation ratio $$\frac{1}{2}$$.

This Hamiltonian can also optionally be weighted with coefficients $$w_{ij}$$.

**Wigner GUE**: This is a random family of Hamiltonians of the form

$$
H = \frac{1}{2^n} \sum_{\vec{P} \in \{I, X, Y, Z\}^n} g_{\vec{P}} \vec{P}
$$

where an instance is drawn by choosing each $$g_{\vec{P}} \sim \mathcal{N}(0,1)$$ i.i.d. 

There is a variant of this model (let's call it a **Sparse Wigner** Hamiltonian) with similar eigenvalue statistics, famously studied in [[CDBBT23]](https://arxiv.org/pdf/2302.03394.pdf).

**Quantum spin glasses**: There are a few ways to describe a random family of quantum spin glasses:

* One way is to restrict the **Wigner GUE** to random $$k$$-local Pauli Hamiltonians (i.e. terms with exactly $$k$$ non-identity Pauli matrices). This is studied for example in [[ES14]](https://arxiv.org/pdf/1407.1552.pdf). 
* Another way is to restrict the **Wigner GUE** to terms of the form $$P_{i_1} \otimes \dots \otimes P_{i_k}$$ where $$P$$ is the same Pauli matrix within each term. I'm not sure this has been exactly studied, but it was claimed in [[CDBBT23]](https://arxiv.org/pdf/2302.03394.pdf) in "Related Models"
* Yet another way is to use all terms of the form $$g_{ij} (X_i \otimes X_j + Y_i \otimes Y_j + Z_i \otimes Z_j)$$ where $$g_{ij}$$ is an i.i.d. standard Gaussian. This might most closely resemble the quantum Max-Cut Hamiltonian.

There are also fermionic Hamiltonians, but I will ignore those here.



To talk about: Algorithms, Hardness, worst vs avg case, OGPs, optimal value, tools (Lasserre/VQE/rounding), NLTS, PCPs, certificates for random graphs, ...

---

Inspired by conversations with many people, including Bobak Kiani, Joao Basso, Robbie King, Nathan Ju, James Sud, and Adrian She.

More to add? Email me at [kmarw@uchicago.edu](mailto:kmarw@uchicago.edu).
