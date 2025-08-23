# What is "quantum" Max-Cut?

It is a long-standing goal to find problems with **rigorous** quantum advantage.
In classical optimization, evidence is dubious. However, when the output is a quantum state, there is (intuitively) more hope.
These *quantum optimization problems* can be formulated as follows: given a local Hamiltonian of $$n$$ qubits

$$ H = \sum_{i=1}^m H_i \, , $$

approximate the maximum energy (eigenvalue) of $$H$$. Here, we require the $$H_i$$ act nontrivially on a constant number of qubits and have norm $$\mathcal{O}(1)$$.

A seminal result from [[KSV02]]({{site.baseurl}}/bib#KSV02) showed that approximating the maximum energy to inverse polynomial accuracy in $$n$$ is $$QMA$$-complete.
A sequence of works then showed that restricted versions of the Hamiltonian, i.e. to specific choices of $$H_i$$, remain $$QMA$$-complete. Simpler Hamiltonians with hardness guarantees often allow us to tailor algorithms to specific instances of hard problems, thus probing more concretely the approximatability of these problems. This is in direct analogy to the field of classical optimization, where problems such as max-$$k$$-SAT are often used as canonical examples for studying algorithms and complexity. 

One such canonical example for quantum optimzation was shown in [[PM15]]({{site.baseurl}}/bib#PM15) to be $$QMA$$-complete

$$ H = \sum_{(i,j)\in E}  \frac{w_{ij}}{2}\left( I_i \otimes I_j - X_i \otimes X_j - Y_i \otimes Y_j - Z_i \otimes Z_j \right) \, ,$$

where $$E, w_{ij}\ge 0$$ denote the set of edges in some graph $$G$$ and their respective weights. 
This problem was formally introduced in [[GP19]]({{site.baseurl}}/bib#GP19) as the *quantum MaxCut* (QMC) problem.
The name is due to an analogy to the classical MaxCut problem, where a single term on an edge of the graph is a rank-2 projector of the vertices in the edge onto the classical antisymmetric bitstrings

$$H_i = \vert 01\rangle \langle 01\vert + \vert 10 \rangle \langle 10 \vert \, .$$

In contrast, quantum MaxCut projects each edge onto the antisymmetric quantum *singlet state* $$\vert \psi^{-} \rangle = \frac{1}{\sqrt{2}} (\vert 01\rangle - \vert 10\rangle)$$

$$ H_i = \psi^{-} \rangle  \langle \psi^{-} \vert \, .$$

Much like how $$\vert 01\rangle$$ and  $$\vert 10\rangle$$ are the unique antisymmetric computational basis states on two qubits, $$\vert \psi^{-} \rangle$$ is the unique antisymmetric **Bell basis** state on two qubits. Indeed, one may study Hamiltonians consisting of projectors onto symmetric Bell basis states. This is known as the EPR Hamiltonian.

In this page, you will find descriptions of QMC, EPR, as well as other well-studied quantum optimization problems. TODO: roadmap



this page is inspired by conversations with many people, including Bobak Kiani, Joao Basso, Robbie King, Nathan Ju, Ojas Parekh, Anuj Apte, Eunou Lee, and Adrian She.

More to add? Email us at [kmarw@uchicago.edu](mailto:kmarw@uchicago.edu) and [jsud@uchicago.edu](mailto:jsud@uchicago.edu) or open an issue on [GitHub](https://github.com/marwahaha/quantum-maxcut-reference).



