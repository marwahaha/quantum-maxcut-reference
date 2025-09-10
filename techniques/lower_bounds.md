---
type: technique
---

# Lower Bounds

In the context of approximation algorithms, lower bounds are *algorithmic*, i.e. they represent a guarantee on the energy a state prepared by an algorithm can reach on a given problem instance. We here describe common algorithmic lower bounds. We mostly focus on QMC, and provide some algorithms specific to EPR or XY as noted. 

### Maximally Mixed State

We begin with a poor choice of an algorithmic lower bound. Consider the maximally mixed state $$\tilde{\rho}$$. For QMC, the energy of this state on a single unweighted edge is 

$$
\mathrm{Tr}\,[\tilde{\rho} \,h^{QMC}_{ij}]  = \tfrac{1}{2}\mathrm{Tr}\,[\tilde{\rho} \,(I_iI_j-X_iX_j-Y_iY_j-Z_iZ_j)]  = 1/2.
$$

In fact, the same result holds for EPR and XY. For QMC on a single edge, the maximum energy is $$2$$ (provided by the singlet state), so the mixed state only achieves a quarter of the optimum energy. In classical optimization, we sometimes use a random assignement as our baseline for algorithms, the Maximally Mixed State performs a similar role for our quantum optimization problems.

## Product States

We begin our search for better algorithmic lower bounds with product states, which are perhaps the simplest to explain and write down. By product states we mean tensor products of single qubit states - there is no entanglement present at all.


### Zero State 

We begin with a toy example, which works reasonably well in some cases for EPR. The zero state on a graph with $$n$$ qubits is simply the computational basis all zero state

$$
|ZERO\rangle = \otimes_{i=1}^n |0\rangle_i,
$$

This state is reasonable for EPR as the energy on an unweighted edge $$(i,j)$$ is simply

$$
\langle 00 | h^{EPR}_{ij} |  00\rangle= 1.
$$

That is, the state obtains an energy of $$1$$ on each edge. It is not hard to show that this is the *maximum* energy that *any product state* can get on a single unweighted edge, so the Zero State is indeed the optimal product state for EPR. By linearity, we can derive that its energy is given by

$$
\langle ZERO | H^{EPR}_G |  ZERO\rangle = W(G),
$$

where $$W(G)$$ is the weight of the graph. This is an important observation: for EPR, *the optimal product state is already known*. It is an open question as to whether the complexity of finding the optimimum product state is related to that of finding the optimum general state (i.e. solving the problem itself) (see Open Problems).

Note that this state is not great for QMC or XY, as it obtains energy $$0$$ and $$1/2$$ respectively on each unweighted edge, theryby doing worse or equivalent to the Maximally Mixed State. The intution is that QMC and XY encode projectors onto *asymmetric* states, and fully symmetric states like the Zero State lie in an orthogonal subspace to this image.

### Cut state

Indeed, for QMC and XY, we can design more refined product states. We focus on QMC, as it is most illustrive, and similar results for XY can be found in [[GP19]]({{site.baseurl}}/bib#GP19) and [[APS25]]({{site.baseurl}}/bib#APS25). 

For QMC, we can try to create product states that are antisymmetric. Fortunately, this is already a classical optimization problem known as Maximum Cut (MaxCut). Thus, we can run classical algorithms for MaxCut, which return classical bitstrings in $$\{0,1\}^n$$. We can then prepare these bitstrings in the computational basis

$$
|CUT\rangle = \otimes_{i=1}^n |z\rangle_i
$$

where $$z_i$$ is the value of the bitstring at bit $$i$$. We then see that the energy on an unweighted edge $$(i,j)$$ depends on the parity of $$i$$ and $$j$$

$$
\begin{aligned}
\langle 00 | h^{QMC}_{ij} |  00\rangle &= 0, \\
\langle 01 | h^{QMC}_{ij} |  01\rangle &= 1, \\
\langle 10 | h^{QMC}_{ij} |  10\rangle &= 1, \\
\langle 11 | h^{QMC}_{ij} |  11\rangle &= 0.
\end{aligned}
$$

The energy of the Cut State then is 

$$
\langle CUT | H^{QMC}_G |  CUT\rangle = C_z,
$$

where $$C_z$$ is the weight of the cut $$z$$.

Cut States are useful because we have upper bounds based on cuts (Cut Bound). This allows us to directly relate the lower and upper bounds in terms of cuts, allowing us to derive approximation ratios.

One caveat, however, is that MaxCut is $$NP$$-hard. Here, we can rely on a result from [[BFV10]]({{site.baseurl}}/bib#BFV10) (see [[AGM20]]({{site.baseurl}}/bib#AGM20), Theorem 1) which says that we can prepare a product state efficiently with energy at least $$.956$$ times the optimal product state. As the Cut State is a product state, this means we can efficiently prepare states with energy at least  $$.956\,C(G)$$, where $$C(G)$$ is the weight of the maximum cut of $$G$$.

### GP rounding 

A more general approach to preparing product states falls under the *relax-and-round* framework of optimization algorithms. 
In this framework, the problem is relaxed to an efficiently solvable instance with some guaranteed gap on the "looseness" of the relaxation. 
The relaxed problem is then *rounded* to a valid solution for the original problem in a way that the amount of energy lost is suitably bounded. 

Gharibian–Parekh (GP) rounding was introduced in [[GP19]]({{site.baseurl}}/bib#GP19) and works as follows

1. **Solve a Level-$$k$$ SoS (Upper Bounds):** obtain an optimal linear functional $$L$$ satisfying the constraints of the relaxation.  
2. **Extract local marginals:** for each qubit $$i$$, compute the single-qubit density operator
   $$
   \rho_i := \tfrac{1}{2}\Big(I + L(X_i)X + L(Y_i)Y + L(Z_i)Z\Big).
   $$
   By positivity of $$\Gamma^{(2)}$$, each $$\rho_i$$ is a valid qubit state.  
3. **Randomized rounding:** for each vertex $$i$$, sample a pure state from the distribution defined by $$\rho_i$$ (or deterministically pick the closest pure state if derandomization is desired).  
4. **Form the product state:** output
   $$
   |\Psi\rangle = \bigotimes_{i=1}^n |\psi_i\rangle.
   $$


An appealing aspect of GP rounding is that it unifies the earlier product-state constructions: the cut-based states described above can be recovered as special cases, while the SDP relaxation provides a principled way to go beyond cuts by exploiting richer two-qubit correlations. Thus, GP rounding is the **most general known framework for efficiently preparing high-energy product states** in QMC.


## Matching-Based States

Matching-based states slightly extend the space of states from product states to tensor products of $$1$$ and $$2$$-qubit states. The intuition is as follows: the QMC Hamiltonian aims to project each pair of qubits into a maximally entangled state (namely the singlet state). One may then try to prepare every edge simulateneously in a singlet state. However, this approach is blocked by *monogamy of entanglement*. I.e., one qubit can only be maximially entangled with one of its neighbors, and then it must be completely disentangled with all others. This forces us to consider slightly more complicated approaches.


### Match State

For a Match State, we to try to *maximally entangle as many disjoint edges as possible*. By this, we mean to pick a set of edges with maximum weight, such that no vertex appears in more than one edge. Conveniently, this is exactly the *maximum weighted matching problem*, which is in $$P$$ due to the Blossom algorithm of  [[Edm65]]({{site.baseurl}}/bib#Edm65). Concretely, a Match State is formed as follows

1. Run Edmonds algorithm to find the set of edges $$M$$ constituting a maximum weighted matching.
2. Prepare each pair of qubits in $$M$$ in the singlet state.
3. Prepare the remaining qubits in any state (say, the maximally mixed state)

Indeed, in step 3, we note that it *doesn't matter* what state the rest of the qubits are in. Because we choose a maximum matching, every edge is either in the matching or incident to an edge in the matching. The first case is handled by step 2, and in the second case the matched qubit is maximally entangled with its partner, so from the perspective of the third qubit, the matched qubit is *maximally mixed*. Thus, it does not matter what state the third qubit takes on, the energy of the edge will always be $$1/2$$. Indeed the total energy of the Match State is simple to calculate: each matched edge gets energy $$2 \,w_{ij}$$ and each unmatched edge gets energy $$w_{ij}/2$$ (as it locally looks like the Maximally Mixed State), so the total energy is

$$
\langle MATCH | H^{QMC}_G |  MATCH\rangle = 2 M(G) + \frac{1}{2}(W(G)-M(G)) = \frac{3M(G)+W(G)}{2},
$$

where $$M(G)$$ is the weight of the maximum matching. Match States, like Cut States, are nice because we have upper bounds based on matchings, so we can directly relate our upper and lower bounds in terms of combinatorial graph invariants.

### Partial Match State

Instead of *maximially entangling* all edges in a matching, we may also attempt to *partially entangle* them. This allows to gain energy on matched edges without forcing all unmatched edges to have the Maximally Mixed State energy $$1/2$$. In order to define a Parital Match State, we first need to define an initial product state, and then a final Match State that we would like to rotate to. We can then parameterize this rotation and interpolate between the two.

We start with an illustrative example for EPR, where we know that the optimal product state is the Zero State. We would then like to design a circuit that interpolates between this state and the Match State, which for EPR prepares each matched edge in the EPR pair. A circuit to perform this interpolation is given in [[Kin23]]({{site.baseurl}}/bib#Kin23), Sec 5.1. 

$$
|PMATCH\rangle = \prod_{(i,j) \in M} \exp(i \theta P_i P_j) |Zero\rangle, \quad P_i = \tfrac{1}{\sqrt{2}}(X_i-Y_i).
$$

When $$\theta=0$$ this state is the Zero State, and when $$\theta=\pi/2$$, this is the Match State for EPR. It can be easily confirmed that the unitaries $$exp(i \theta P_i P_j)$$ all pairwise commute.

One nice property of this circuit is that it *preserves the orientation of single-qubit marginals*. 
What we mean by that is as follows: in the Zero State, the Bloch vector of each qubit is simply pointing straight up to the north pole (the state $$|0\rangle$$).
As we apply the rotations $$\exp(i \theta P_i P_j)$$, the Bloch vectors of each matched qubit, representing their *single-qubit marginal* states, simply contract toward the origin. 
That is, their direction remains, but their magnitude shrinks. 
At $$\theta=\pi/2$$, the matched qubits have become maximally entangled, so their Bloch vectors collapse to the origin (i.e. that maximally mixed state). 
In between, we have a tunable parameter that interpolates between these two states, while preserving the direction of the single-qubit marginals along the way. 
This fact aids in analysis: if we have guarantees on the energy a product state can achieve, then in order to compute the eenrgy of a Partial Match State we simply need to rescale the contributions from the product state by the decay in their Bloch vector magnitudes. 

For QMC, the initial product state must be more complicated that the Zero state, so the analysis becomes messier. However, [[ALMPS25]]({{site.baseurl}}/bib#ALMPS25) discusses the preparation of Partial Match States starting from *any* initial product state.


## AGM Circuits

The general form of circuits preparing Partial Match States above was given by [[AGM20]]({{site.baseurl}}/bib#AGM20), Section 3. The method is

1. Prepare a good product state (i.e. CUT State for QMC or XY, ZERO State for EPR)
2. Apply a circuit of the form

$$
V(\theta) = \prod_{(i,j)\in E} exp(i \theta P_i P_j),
$$

where $$P_i$$ is an operator that acts only on $$i$$ and is specificied exactly by the initial (product) state of qubit $$i$$. Note that the terms $$exp(i \theta P_i P_j)$$ all pairwise commute, as there is only one unique operator per qubit. These circuits generally allow us to go beyond the framework of tensor products of $$1$$ and $$2$$-qubit states. They also provide circuit descriptions of quantum states, rather than the explicit descriptions above, which are only efficient due to tensor product structure.

### Fractional Match State

One example of an AGM circuit for EPR is the Fractional Match State of [[ALPMS25]]({{site.baseurl}}/bib#ALMPS25)

$$
|FMATCH\rangle = \prod_{(i,j) \in E} exp(i \theta m_{ij} P_i P_j) |Zero\rangle, \quad P_i = \tfrac{1}{\sqrt{2}}(X_i-Y_i),
$$

where $$m_{ij}$$ encodes the edge values of a *maximum fractional matching*. This circuit allows for an algorithm lower bound based on fractional matchings, which complements well with our Fractional Match Bound from (Upper Bounds).

### AGM Circuits for QMC

For QMC, [[AGM20]]({{site.baseurl}}/bib#AGM20) starts with a Cut State based on a bitstring $$z$$ obtained by an efficient algorithm for MaxCut such as Goemans-Williamson). Then $$P_i$$ is set to $$X_i$$ if $$z_i=1$$ and $$Y_i$$ else. This ansatz incorporates the asymmetry of QMC in both the choice of initial product state and in the ansatz operators.






