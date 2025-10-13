---
type: technique
---

# Upper Bounds

Crucial to achieving good approximation ratios for Local Hamiltonian Optimization Problems is deriving good upper bounds on the maximum energy. 
These upper bounds are used both to analyze the approximation ratios and to give results that can be rounded to valid solutions. 
We here summarize common technqiues, focusing on QMC, as near identical results for EPR and XY have been documented in [[King23]]({{site.baseurl}}/bib#King23) and [[APS25]]({{site.baseurl}}/bib#APS25).

## Triangle Inequalities

### Edge-wise 

The maximum value $$h^{QMC}$$ can take on an edge is $$2$$. Thus, an edge-wise application triangle inequality yields 

$$
\|H^{QMC}_G\| \le \sum_{(i,j) \in E} w_{ij} \,\|h^{QMC}_{ij}\| = 2 W,
$$ 

where $$W$$ is the total edge weight. 

### Monogamy of Entanglement on a Star

From Lemma 1 of [[AGM20]]({{site.baseurl}}/bib#AGM20) we have if $$G(V,E,w)$$ is a star graph then

$$
\|H^{QMC}_G\| \le W+ \max_{j \,\sim\, i} w_{ij},
$$

where $$j \sim i$$ denotes vertices adjacent to $$i$$. In particular, if $$G$$ is unweighted this simplifies to 

$$
\|H^{QMC}_G\| \le |E|+1.
$$ 

In fact, for the unweighted case, the bound is tight.

### Vertex-wise 
A bound on the maximum energy of the graph can be achieved by applying the triangle inequality vertex-wise

$$
\begin{aligned} 
\|H^{QMC}_G\| &\le \frac{1}{2}\sum_i \left|\left| \sum_{j \,\sim\, i} w_{ij} \, h^{QMC}_{ij} \right|\right| \\
&\le \frac{1}{2}\sum_i \left(\sum_{j \,\sim\, i} w_{ij} + \max_{j \,\sim\, i} w_{ij}\right) \\
&=W+\frac{1}{2}\sum_i \max_{j \,\sim\, i} w_{ij}, 
\end{aligned}
$$

where in the first line we compensate for double-counting of edges with a factor of $$1/2$$ and in the second line we apply monogamy of entanglement on a star. 
For unweighted graphs this simplifies to 

$$\|H^{QMC}_G\| \le |E|+\frac{|V|}{2}.$$

### Cut bound

For QMC we can also apply the triangle inequality term-wise by separating out the Pauli $$X$$, $$Y$$, and $$Z$$ terms via the decomposition

$$
\begin{aligned}
H^{QMC}_G &= \sum_{ij \in E}\frac{w_{ij}}{2}(I_iI_j - X_iX_j) \\
&+ \sum_{ij \in E}\frac{w_{ij}}{2}(I_iI_j - Y_iY_j) \\
&+ \sum_{ij \in E}\frac{w_{ij}}{2}(I_iI_j - Z_iZ_j) - W \,I.
\end{aligned}
$$

Applying the triangle inequality on each sum we get $$3C-W$$, where $$C$$ is the weight of the maximum cut in $$G$$. 
This is because each Pauli term encodes the maximum cut (MaxCut) Hamiltonians in its own basis. 
For example, in the computational basis the MaxCut Hamiltonian is given by

$$ 
H^{MC}_G := \sum_{ij \in E} \frac{w_{ij}}{2} (I_iI_j - Z_iZ_j).
$$

## Relaxations

Upper bounds for QMC may alternatively be obtained by *relaxing* the problem using program optimization. 
We provide an summarize some examples, building in complexity from simple to complex. 

### Linear programs

#### Fractional match bound

Based on monogamy of entanglement of a star, the QMC energy can be upper bounded via the following linear program (LP).

$$
\begin{aligned}
    \text{max}\quad &\sum_{ij \in E} w_{ij}\, x_{ij}, \\
    \text{subject to} \quad&\sum_{j \sim i}x_{ij}\le d_i+1 \quad \forall i \in V, \\
    \quad &x_{ij} \ge 0 \quad \forall (i,j)\in E,
\end{aligned}
$$

where $$x_{ij}$$ denotes the energy obtained by the program on edge $$(i,j)$$, $$j\sim i$$ denotes the vertices incident to $$i$$, and $$d_i$$ is the degree of vertex $$i$$. 
Because the first constraint encodes monogamy of entanglement and the QMC Hamiltonian on each edge is positive semi-definite, all constraints in the LP are also present in QMC, making the program a valid relaxation. 

In [[LP24]]({{site.baseurl}}/bib#LP24) (Lemma 1) it was observed that by introducing the variables $$y_{ij}^+ = max(x_{ij}-1, 0)$$, the monogamy of entanglement result of a vertex $$i$$ holds

$$
\sum_{j \sim i} y_{ij}^+ \le 1.
$$

Thus, the following linear program is a valid relaxation of the maximization of the Hamiltonian $$H^{QMC}_G - W I$$

$$
\begin{aligned}
    \text{max}\quad &\sum_{ij \in E} w_{ij}\, y^+_{ij}, \\
    \text{subject to} \quad&\sum_{j \sim i}y^+_{ij}\le 1 \quad \forall i \in V, \\
    \quad &y^+_{ij} \ge 0 \quad \forall (i,j)\in E.
\end{aligned}
$$

Interestingly, this is exactly the *maximum weighted fractional matching* LP. Thus, we have that 

$$
\|H^{QMC}_G\| \le W+FM
$$

where FM is the value of the maximum weighted fractional matching LP. This LP is efficiently solvable.

#### Match bound

Indeed, we can actually push the linear program formulation further by incorporating more linear constraints. For instance, consider the linear program for maximum weighted *integer* matching (or simply, a *matching*).

$$
\begin{aligned}
    \text{max}\quad &\sum_{ij \in E} w_{ij}\, z_{ij}, \\
    \text{subject to} \quad&\sum_{j \sim i}z_{ij}\le 1 \quad \forall i \in V, \\
    \quad&\sum_{ij \in E(F)} z_{ij} \le \tfrac{V(S)-1}{2} \quad \forall F \subseteq G \in \mathcal{F}, \\
    \quad &z_{ij} \ge 0 \quad \forall (i,j)\in E,
\end{aligned}
$$

where $$E(F)$$ denotes the edges in the subgraph $$F$$ of $$G$$ and $$\mathcal{F}$$ denotes the set of *biconnected factor-critical graphs*, which are odd-order unweighted graphs such that removing any vertex and its incident edges leaves the graph connected and with a perfect matching. 
This LP, due to [[Edm65]]({{site.baseurl}}/bib#Edm65) is efficiently solvable, and it is shown that vertecies of the LP are *integer-valued*, i.e. $$z_{ij} \in {0,1}$$ for all edges $$(i,j)$$.
 
It is shown in [[LP24]]({{site.baseurl}}/bib#LP24), Lemma 4 that if some solution $$(z_e)_{e \in E}$$ satisfies for all subgraphs $$F$$ up to $$r$$ vertices, then $$\frac{r+1}{r+2}(z_e)$$ satisfies the above LP. Thus it suffices to numerically check that the maximum value of $H^{QMC}_F$ for all biconnected factor-critical graphs up to size $$r$$ satisfy the bound in the second above constraint in order to prove
 
$$
\|H^{QMC}_G\| \le W + a\,M,
$$

where  $$a=\tfrac{r}{r-1}$$ and $$M$$ is the size of the maximum weighted integer matching. 
Modifications of this technique were used to show that the bound is true for $$r=5$$ in [[JKKSW24]]({{site.baseurl}}/bib#JKKSW24) for QMC and EPR and for $$r=13$$ in [[GSS25]]({{site.baseurl}}/bib#GSS25) for QMC. 
[It remains open if $$a$$ can be pushed all the way to $$1$$ for EPR and QMC]({{site.baseurl}}/open_questions.html). Indeed there are examples where $$a=1$$ is tight (for instance consider the graph consisting of a single edge).

### Cone programs (SOCP)

The first step to going beyond linear programming is by adding in nonlinear constraints. Indeed, consider the results in [[PT22]]({{site.baseurl}}/bib#PT22) Lemma 1, which shows that 

$$
h_{ij}^2 + h_{ik}^2 + h_{jk}^2 \le 2(h_{ij}\,h_{jk}+h_{ij}\,h_{ik}+h_{ik}\,h_{jk}),
$$

where $$h_{ij}$$ dentoes the QMC energy on some edge $$(i,j)$$. 
These kinds on nonlinear constraints are referred to as *convexgamy* in [[LP24]]({{site.baseurl}}/bib#LP24) and can be plugged into the linear programs introduced above. 
Indeed, the previous equation, along with the linear constraint $$h_{ij}+h_{ik}+h_{jk} \le 3$$ is enough ensure the consistency of all two-qubit marginals of all triples of vertices [[HTPG24]]({{site.baseurl}}/bib#HTPG24). 
This linear program, augmented with certain nonlinear constraints, can be formulated as a *second order cone program*, which is efficiently solvable.

### Quantum moment-SoS hierarchies (Level-$$k$$ SoS)

Semidefinite programming (SDPs) is a more powerful and general framework for optimization than the methods above. 
Quantum moment-Sum-of-Squares (moment-SoS) hierarchies are dual to SDPs and provide upper bounds on the maximum eigenvalues. 
To introduce the hierarchies, we first observe that we can write QMC exactly as the (exponentially-sized) program

$$
\begin{aligned}
\text{max}\quad & \operatorname{Tr}\big[H^{QMC}_G\,\rho\big],\\[2mm]
\text{s.t.}\quad & \rho \succeq 0,\\[1mm]
& \operatorname{Tr}[\rho]=1.
\end{aligned}
$$

The central bottleneck is the global PSD constraint $$\rho\succeq 0$$: while $$\operatorname{Tr}[\rho]=1$$ is a single linear equation, $$\rho\succeq 0$$ encodes an exponential number of linear constraints (one per principal minor / eigenvalue) because $$\rho$$ is of size $$(2^n\times 2^n)$$.
A practical SDP relaxation therefore **replaces** the global constraint $$\rho\succeq 0$$ by positivity constraints that are only required to hold on a *polynomial-sized family* $$\mathcal{M}$$ of operators. Writing $$L(M):=\operatorname{Tr}[M\rho]$$, this relaxed program is

$$
\begin{aligned}
\text{max}\quad & L(H^{QMC}_G),\\[2mm]
\text{s.t.}\quad & L(M)\ge 0\quad \forall\,M\in\mathcal{M},\\[1mm]
& L(\mathbb{I})=1.
\end{aligned}
$$

This program has an interpretation of maximizing over *pseudo-states* $$\rho$$ where requiring $$L(M)\ge 0$$ for every $$M$$ of the form $$O^\dagger O$$ ensures that the linear functional $$L$$ behaves like an expectation value against a pseudo-state. Different relaxations correspond to different choices of $$\mathcal{M}$$ and different ways of encoding the constraint $$L(M)\ge 0$$. The strength of the relaxation is controlled by a "level" parameter $$k$$, which roughly characterizes the size of $$\mathcal{M}$$, typically the leading order of the polynomial.

Here are three examples of SDP heirarchies for QMC.


#### 1. The quantum Lasserre hierarchy 

The **quantum Lasserre heirarchy** used first for QMC in [[GP19]]({{site.baseurl}}/bib#GP19) considers expectation values of products of Pauli matrices up to some bounded size. Define the operator span

$$
\mathcal{V}_k := \text{span}\big\{ \sigma_{i_1}^{\alpha_1}\cdots \sigma_{i_t}^{\alpha_t} \;\big|\; t\le k,\;\alpha_j\in\{I,X,Y,Z\},\; i_j\in[n]\big\},
$$

i.e. all Pauli strings of length at most $k$.  

The **moment matrix** is then

$$
\Gamma^{(k)}_{u,v} = L(u^\dagger v), \qquad u,v\in \mathcal{V}_k.
$$

The constraint $$L(O^\dagger O)\ge 0$$ for all $$O\in\mathcal{V}_k$$ is exactly equivalent to

$$
\Gamma^{(k)} \succeq 0.
$$

So the level-$$k$$ relaxation is:

$$
\begin{aligned}
\text{max}\quad & L(H^{QMC}_G),\\
\text{s.t.}\quad & \Gamma^{(k)} \succeq 0,\\
& L(\mathbb{I})=1,\\
& \text{linear identities from Pauli algebra hold.}
\end{aligned}
$$

At **level-2**, for example, we include all single- and two-qubit Pauli operators. 
This already captures a rich set of correlations and gives strong bounds, though the size of $$\Gamma^{(2)}$$ grows quadratically with $$n$$.  

#### 2. The NPA heirarchy 

The **Proj relaxation** of [[Tak+23]]({{site.baseurl}}/bib#Tak+23) adapts the **NPA hierarchy** to QMC. Its key idea is to exploit **SU(2) symmetry**: the fact that QMC Hamiltonians are invariant under identical single qubit unitary rotations on each qubit.  
Instead of indexing the moment matrix by *all* Pauli monomials, the Proj program only keeps track of **SU(2)-invariant combinations**. 
This massively reduces the number of variables while still capturing the relevant structure of the problem.  

Formally, we again build a moment matrix

$$
\Gamma^{\mathrm{Proj}}_{i,j} = L(b_i^\dagger b_j),
$$

but now the basis $\{b_i\}$ consists only of operators consistent with the SU(2) algebra. 
Alongside $$\Gamma^{\mathrm{Proj}}\succeq 0$$, we impose all the Casimir identities and commutation rules of the spin algebra.  

The program then looks like:

$$
\begin{aligned}
\text{max}\quad & L(H^{QMC}_G),\\
\text{s.t.}\quad & \Gamma^{\mathrm{Proj}} \succeq 0,\\
& L(\mathbb{I})=1,\\
& \text{Casimir identities and commutation rules hold.}
\end{aligned}
$$

The advantage: thanks to symmetry, the size of $$\Gamma^{\mathrm{Proj}}$$ grows much more slowly than in the Lasserre hierarchy, while still converging to the true optimum at a finite level.  

#### 3. The SWAP hierarchy 

The **SWAP heirarchy** of [[WCEHK24]]({{site.baseurl}}/bib#WCEHK24) takes a different route: instead of Pauli operators, it builds everything from the **swap operators** $$S_{ij}$$ that exchange two qubits $$i$$ and $$j$$.  

This is natural because the singlet projector on edge $$(i,j)$$, which defines the QMC Hamiltonian, can be written as

$$
P^{\mathrm{singlet}}_{ij} = \tfrac{1}{2}(\mathbb{I} - S_{ij}).
$$

So the Hamiltonian is already linear in swaps.

The operator space is then

$$
\mathcal{V}^{\mathrm{SWAP}}_d := \text{span}\{ S_{i_1 j_1} S_{i_2 j_2}\cdots S_{i_t j_t} : t\le d \},
$$

and the moment matrix is

$$
\Gamma^{\mathrm{SWAP}}_{u,v} = L(u^\dagger v), \qquad u,v\in \mathcal{V}^{\mathrm{SWAP}}_d.
$$

The constraints are

$$
\Gamma^{\mathrm{SWAP}} \succeq 0,\qquad L(\mathbb{I})=1,
$$

plus all **swap algebra relations**, like $$S_{ij}^2=\mathbb{I}$$ and braid relations $$S_{ij}S_{jk}S_{ij}=S_{jk}S_{ij}S_{jk}$$.  

A crucial benefit is that the swap algebra is exactly the group algebra of the symmetric group $$S_n$$, which can be block-diagonalized using representation theory. 
This allows the relaxation to scale to larger $n$ than a direct Pauli-based moment matrix.  

Even at **low degree (e.g. $d=2$)**, [[WCEHK24]]({{site.baseurl}}/bib#WCEHK24) shows that the SWAP relaxation gives strong bounds.  


