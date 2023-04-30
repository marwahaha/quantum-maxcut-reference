# What is "quantum" Max-Cut?

It is a long-standing goal to find problems with **rigorous** quantum advantage.
In classical optimization problems, evidence is dubious; however, when *the output is a quantum state*, there is (intuitively) more hope.

This is a collection of notes about Hamiltonians, both algorithms and obstructions for them.

$$\newcommand{\ket}[1]{\mathinner{|{#1}\rangle}}$$


$$\newcommand{\bra}[1]{\mathinner{\langle{#1}|}}$$

## The Hamiltonians

Recall that we use Pauli matrices $$P \in \{I, X, Y, Z\}$$, and $$P_i$$ means the Pauli matrix $$P$$ at site $$i$$.

**Quantum Max-Cut**: This is defined by a graph $$G(V,E)$$, where 

$$
H = \frac{1}{2} \sum_{(i,j) \in E} I_i \otimes I_j - X_i \otimes X_j - Y_i \otimes Y_j - Z_i \otimes Z_j
$$

Alternatively, this can be written as $$H = \sum_{(i,j) \in E} 2 |\psi^{-} \rangle  \langle \psi^{-} |$$, where $$|\psi^{-} \rangle = \frac{1}{\sqrt{2}} (|01\rangle - |10\rangle)$$ is the singlet state. Note that this Hamiltonian is rotation-invariant, and the minimum energy is zero.

This Hamiltonian can optionally be weighted with coefficients $$w_{ij}$$.

**EPR Hamiltonian**: I think this term is due to [[King22]](https://arxiv.org/pdf/2209.02589.pdf). This is defined also by a graph $$G(V,E)$$, where

$$
H = \frac{1}{2} \sum_{(i,j) \in E} I_i \otimes I_j + X_i \otimes X_j - Y_i \otimes Y_j + Z_i \otimes Z_j
$$

Alternatively, this can be written as $$H = \sum_{(i,j) \in E} |\phi^+\rangle \langle \phi^+|$$, where $$|\phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$ is the EPR pair. Note that this Hamiltonian has a particularly simple optimal product state of $|0\rangle^{\otimes n}$ with approximation ratio $\frac{1}{2}$.

This Hamiltonian can also optionally be weighted with coefficients $$w_{ij}$$.

**Wigner GUE**: This is a random family of Hamiltonians of the form

$$
H = \frac{1}{2^n} \sum_{\vec{P} \in \{I, X, Y, Z\}^n} g_{\vec{P}} \vec{P}
$$

where an instance is drawn by choosing each $g_{\vec{P}} \sim \mathcal{N}(0,1)$ i.i.d. 

UNDER CONSTRUCTION: TO WRITE

"Spin glass" refers to a model exhibiting a weirdo "glassy" state of matter. There is no universal mathematical definition of a spin glass. Typically, the phrase refers to the physical phenomena of ground states of magnetic alloys that are "frozen" in their configuration at low temperatures.

This is typically modeled by a probability distribution on the overall state of the particles $$\sigma \in\{-1,+1\}^N$$. Each of the particles $$\sigma_i$$ are also called **sites**.

The Hamiltonian is

$$
H(\sigma) = \sum_{k=1}^\infty c_k \langle J^{(k)}, \sigma^{\otimes k} \rangle
$$

where we have fixed parameters $$c_k \in \mathbb{R}$$, and a random **disorder matrix** or **interaction matrix** $$J^{(k)}$$. This is a random symmetric Gaussian tensor of order $$k$$. (If $$J^{(k)}$$ is a random matrix of Booleans instead of Gaussians, we say this is "boolean disorder". The behavior is qualitatively the same as for "Gaussian disorder")

Physical states can be thought of as samples from the Gibbs distribution, which weighs the likelihood you are likely to see different spin configurations based on relative energies and a temperature parameter. Given an inverse temperature $$\beta$$, the probability  of a given configuration $$\sigma$$ is

$$
\Pr[\sigma] \propto e^{\beta H(\sigma)}.
$$

## Why spin glasses?

Historically: People studied the free energy of magnetic alloys (an Ising model, where each particle has "positive" or "negative" spin i.e. a Boolean) on a lattice (because physics, $$\mathbb{Z}^2$$ or $$\mathbb{Z}^3$$).

The lattice is too hard, so we relax to a "mean-field model", where all particles are connected to all other particles. Here, we can finally get a mathematical handle on the behavior of the model.

## What kinds of spin glasses are there?

We usually consider a spin glass with the following characteristics:
* ``mixed-spin`` or ``pure``
* ``spherical`` or ``Ising`` domain

### Mixed vs Pure
A **pure** spin glass (of level $$p$$) only has $$p$$-body interactions. By contrast, a **mixed-spin** spin glass could have interactions of any number of terms.

We can see this with the mixture polynomial $$\xi(s)$$:

$$
\xi(s) = \sum_{k=1}^\infty c^2_k s^k
$$

With a pure spin spin glass, $$c_k = 0$$ except when $$k = p$$.

### Spherical vs Ising

An **Ising** spin glass has the spin configurations $$\sigma \in \{+1,-1\}^N$$. Sometimes, this is too challenging, so we consider **spherical** spin glasses, where the domain of spin configurations $$\{-1,+1\}^N$$ is relaxed to the sphere of radius $$\sqrt{N}$$ in $$\mathbb{R}^N$$.

Think of the sphere as a relaxation of the hypercube. The ground state energy for the Ising model is an upper bound to the ground state energy for the spherical model. However, because the sphere is a connected set (whereas the hypercube is discrete), properties of spherical spin glasses may be easier to determine using (e.g.) calculus.



## Spin glass energy, PVP, RSB

Sherrington and Kirkpatrick [[SK75]](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.35.1792)  heuristically computed the free energy of a simple nontrivial spin glass using the "replica method". However, their "RS ansatz" is not physical at low temperature.

To fix this, Parisi [[Par80]](https://iopscience.iop.org/article/10.1088/0305-4470/13/4/009) introduced the "RSB ansatz" (aka Parisi ansatz). We can use this to describe the free energy at any temperature using a variational principle.

How does this work? We call $$P(\gamma)$$ the *Parisi functional*, where $$\gamma: [0,1] \to \mathbb{R}$$ is any nondecreasing function ($$\gamma$$ is called the "functional order parameter").

$$
GSED = min_{\gamma} P(\gamma)
$$

If $$\gamma$$ is constant, this is **replica symmetric* (RS).
If there is a point where $$\gamma$$ is increasing, we call it **replica symmetry breaking** (RSB).

If $$\gamma$$ takes *exactly* $$k$$ nonzero values, then we call it $$k$$-RSB. (For example, 1-RSB, 2-RSB, ...)

If it is not RS, and not $$k$$-RSB for some constant $$k$$, then $$\gamma$$ takes on an infinite number of nonzero values. We call this $$\infty$$-RSB. But this could mean lots of things:
* There could be countably many values (in $$\mathbb{N}$$) or uncountably many (in $$\mathbb{R}$$).
* There could be some inputs in $$[0,a]$$ and $$[b,1]$$ where $$\gamma$$ is strictly increasing, yet constant from $$[a,b]$$.
* If $$\gamma$$ is strictly increasing in $$[0,1]$$, we call it Full-RSB or fRSB.
* Some works use the language $$\ell$$-fRSB to count the number of disjoint regions in $$[0,1]$$ that are strictly increasing; see for example [[AZ22]](https://arxiv.org/pdf/2209.03866.pdf).

We can view the $$\gamma$$ like an unnormalized cumulative distribution function; i.e. $$\gamma(0_-) = 0$$. Every PDF will have support at least at one point (perhaps at zero). Every additional point is considered "replica symmetry breaking".


# CSPs and diluted spin glasses

Mean-field models are **dense**, where all things are connected to all things. **Sparse** models have $$o(N)$$ average degree.

The physicists' dream is to study Ising models on sparse, "physical" graphs (like the lattice $$\mathbb{Z}^3$$). Unfortunately, we only have good results for random graphs, not lattices. A sparse, random interaction matrix is called a **diluted mean-field model**.

For example:
 * The interactions may be along a random $$d$$-regular graph
 * or random Erdos-Renyi graph with average degree $$d$$.

When the models are random, we can prove properties about sparse models using a corresponding dense versions. This uses important properties of sparse random graphs, which are **expanders**, and almost all small neighborhoods are trees. On the other hand, these are "not physical" ($$\mathbb{Z}^3$$ does not have these properties).


Random CSPs are sparse models. However, some of the properties of dense spin glasses do actually appear if you look closely and in the right places.

When studying CSPs, it's mathematically useful to introduce $$\beta$$ (a smoothening parameter). CSPs don't intrinsically have a notion of temperature.


## Value of CSPs in UNSAT regime


When CSPs are in the "unsatisfiable regime", (e.g. the clause density is high enough), **we can relate the optimal value to the ground state energy density of a dense spin glass.**

This is known in several settings:
* MaxCut $$\longleftrightarrow$$ SK model: [[DMS'17]](https://arxiv.org/abs/1503.03923)
* Max $$k$$-XOR $$\longleftrightarrow$$ pure $$k$$-spin Ising model [[Sen'17]](https://arxiv.org/abs/1606.02365)
* Max $$q$$-cut $$\longleftrightarrow$$ Potts model [[Sen'17]](https://arxiv.org/abs/1606.02365)
* Max $$k$$-SAT $$\longleftrightarrow$$ a particular Ising mixed spin glass model [[Panchenko'17]](https://arxiv.org/abs/1608.06256)

**Is this relationship true in general?** This is a natural question to ask, and for now, is open. However, major progress was made on this by a paper of Panchenko and Talagrand:
* [[PT14]](https://arxiv.org/abs/math/0405357) - This paper _upper bounds_ the limiting value of the fraction of satisfied clauses of _any_ CSP with $$\alpha n$$ clauses ($$\alpha$$ is any constant) by the free energy of a peculiar dense mean-field model (which can be seen as a slight generalization of the SK model). The latter model's free energy is explicitly calculated in _any_ regime (RS/$$k$$-RSB/fRSB). However, the paper cannot provide lower bounds, and so it does not capture the exact limiting value.

## OGPs for CSPs
What is an OGP? See [[Gamarnik21]](https://arxiv.org/abs/2109.14409) for a reference.

If there is an OGP, then algorithms that do not see the whole graph (i.e., **local algorithms** with at most $$\log N$$ depth) will not find optimal solutions. In fact, all algorithms that are insensitive to small changes in the input will be obstructed by an OGP. See [[CLSS21]](https://arxiv.org/abs/2108.06049).

For the spin glass problems, an OGP means there is some interval $$[a,b]$$ where $$0 < a < b < 1$$ where $$\gamma$$ is constant, yet $$\gamma$$ changes value **both** in $$[0,a]$$ and also in $$[b,1]$$. We typically think about OGP at low temperature.

**A key takeaway:** If the Parisi minimizer is RS or fRSB, then there is no overlap gap property. Otherwise, there is an OGP, even if it is $$\infty$$-RSB!

We also know that some CSPs exhibit an OGP, because the associated dense spin glass model does. This connection is usually done via the Guerra-Tonnineli interpolation.

* [[CGPR19]](https://arxiv.org/abs/1707.05386) - Max Cut of $$k$$-uniform hypergraphs for $$k \ge 4$$ even exhibits an OGP a.a.s across all instances.
* [[CLSS21]](https://arxiv.org/abs/2108.06049) -  Max $$k$$-XOR exhibits an OGP a.a.s across all instances.

From [[CLSS21]](https://arxiv.org/abs/2108.06049), it turns out that OGPs obstruct local classical _and_ local quantum algorithms from well-approximating random MAX $$k$$-XOR instances.



# What we know about spin glass models

## Example: The SK Model
This is the canonical spin glass people study; i.e. the pure model at $$p=2$$.

At high temperature (when $$\beta < \beta_c$$), we know the SK Model is $$RS$$ (or 0-RSB) [[ALR87]](https://cmsr.rutgers.edu/images/people/lebowitz_joel/publications/jll.pub_286.pdf), [[Bol18]](https://arxiv.org/abs/1809.07972). There exists an efficient algorithm to output solutions to the free energy. this is done by solving the TAP equations iteratively by Bolthausen [[Bol12]](https://arxiv.org/pdf/1201.2891.pdf).

At low temperature (when $$\beta > \beta_c$$), the SK Model is $$\infty$$-RSB [[ACZ20]](https://arxiv.org/abs/1703.06872). There exists an efficient algorithm to output solutions to the free energy. Assuming fRSB, this is done by an AMP algorithm that discretizes the dynamics of the Auffinger-Chen Principle rewrite of the Parisi-Variational Principle. This algorithm is due to Montanari [[Mon19]](https://arxiv.org/abs/1812.10897).

Some other cool papers:
* [[Crisanti 2002]](https://arxiv.org/abs/cond-mat/0111037) shows the actual original plots getting 0.7632 for SK.
* [[Parisi 1980]](https://iopscience.iop.org/article/10.1088/0305-4470/13/4/009): The original Parisi paper approximating the SK model, with a good visual of replica symmetry breaking.
* [[FA85]](https://iopscience.iop.org/article/10.1088/0305-4470/19/9/033/meta): Early work showing SK has a correspondence with MaxCut

##  Mixed even-spin Ising models
Some results for SK generalize to mixed-spin models, especially if they are *even* (i.e. $$c_k = 0$$ when $$k$$ is odd).

* For pure models when $$k \ge 4$$ even, there is a coupled-OGP: [[CGPR19]](https://arxiv.org/abs/1707.05386)
* For mixed-spin models of even spin, the number of configurations at each energy level is known: [[CHL17]](https://arxiv.org/abs/1609.04368)
* [[Panchenko 2011]](https://arxiv.org/pdf/1112.4409.pdf) extends Parisi's proof from $$p=2$$ to general mixed even-spin Ising models.
* See also [[AT16]](https://link.springer.com/article/10.1007/s00440-015-0691-z), [[AC15]](https://onlinelibrary.wiley.com/doi/abs/10.1002/cpa.21617) for more about the phase diagram.


## General Ising models

We also know several things about the Parisi functional for any Ising spin glass:

* It is strictly convex, so it has a unique minimum: [[Auffinger 2014]](https://arxiv.org/abs/1402.5132)
* It is differentiable: [[Panchenko 2008]](https://projecteuclid.org/journals/electronic-communications-in-probability/volume-13/issue-none/On-differentiability-of-the-Parisi-formula/10.1214/ECP.v13-1365.full)
* [[Auffinger 2013]](https://arxiv.org/abs/1303.3573) is very clear explaining order parameters and replica symmetry breaking
* [[Chen 2015]](https://arxiv.org/abs/1501.06635) describes several properties of the Parisi functional
* [[Jagannath 2015]](https://arxiv.org/abs/1502.04398) has an alternate approach to calculating the Parisi formula at zero temperature with dynamic programming. See a [talk here](https://cims.nyu.edu/~aukosh/slides/cornelltalk.pdf).



A rewrite of the Parisi-Variational Principle in terms of a _functional_ min-max principle is provided by the Auffinger-Chen Principle [[AC17]](https://arxiv.org/pdf/1402.5132.pdf). The AC Principle gives a rewrite of the PVP in terms of an auxiliary stochastic process (a drifted Brownian motion) that does a random walk in the cavity field component of the initial free-entropy term ($$\Phi(1, x)$$).



## Spherical models

* Pure Spherical $$p$$-spin glass is 2-RSB at $$\beta = \infty$$ [[AZ18]](https://arxiv.org/pdf/1802.02991.pdf). (for all $$p \geq 2$$???)
* The analytic behavior of the mixture polynomial determines when the mixed spherical $$p$$-spin glass is fRSB [[Proposition-1, Sub21]](https://arxiv.org/abs/1812.04588).
* The TAP equations for mixed spherical $$p$$-spin glasses are to be solved on the _ball_ $$\mathcal{B}^n$$ (and not the sphere) [[Sub21a]](https://arxiv.org/abs/2111.07132), [[Sub21b]](https://arxiv.org/abs/2111.07134).
* The free-energy "landscape" of spherical $$p$$-spin glasses (mixed and pure) is well understood by studying the first two moments of the number of critical points of the Hessian using the Kac-Rice formula as a dictionary between the geometry of the problem at hand and the random matrix (the Hessian) [[Sub16]](https://arxiv.org/abs/1804.10576).
* An explicit and analytic expression for the free energy is given using a nice simplification of the _Crisanti-Sommers_ representation known as the Chen-Sen representation [[CS17]](https://arxiv.org/abs/1512.08492); see also [[JT17]](https://link.springer.com/article/10.1007/s00220-017-2864-3). In the event of fRSB, this formula simply becomes an integral over (roughly) the mixture polynomial [[Proposition-2, Sub21]](https://arxiv.org/pdf/1812.04588.pdf).
* [[JB21]](https://arxiv.org/abs/2104.08299) study a shattering transition.
* See also [[AB13]](https://projecteuclid.org/journals/annals-of-probability/volume-41/issue-6/Complexity-of-random-smooth-functions-on-the-high-dimensional-sphere/10.1214/13-AOP862.pdf) and [[ABC13]](https://onlinelibrary.wiley.com/doi/pdf/10.1002/cpa.21422) for other papers using the Kac-Rice approach.
* fRSB is known for some models due to Talagrand (for example see [[Talagrand 2005]](https://eds.s.ebscohost.com/eds/pdfviewer/pdfviewer?vid=1&sid=e553dbeb-b9e9-4bd1-a4c3-8c3d11385e20%40redis)).


# AMP and OGP
* Define $$L$$ as the space of functions $$\gamma: [0,1) \times [0,\infty)$$ where $$\int_0^1 \gamma(t) dt$$ is finite.
* Let $$U$$ be the restriction of $$L$$ to nondecreasing functions.

Define $$P$$ as the Parisi functional. Then
* $$OPT = inf_{\gamma \in U} P(\gamma)$$ is the solution to the spin glass. This limit exists, and there is a unique $$\gamma_{OPT} \in U$$ that is the minimizer.
* $$ALG = inf_{\gamma \in L} P(\gamma)$$. (I think this limit exists because $$P$$ is lower-bounded but I couldn't find a reference.) There may not be a minimizer $$\gamma_{ALG} \in L$$, but if it does, it is unique.

By [[AMS20]](https://arxiv.org/abs/2001.00904) and [[Sel21]](https://arxiv.org/pdf/2105.03506.pdf), we  know the following things:
* If $$\gamma_{OPT}$$ is fRSB (i.e. strictly increasing), then $$ALG = OPT$$ (and thus $$\gamma_{ALG}$$ exists.)
* If $$\gamma_{ALG}$$ exists, then approximate message passing (AMP) algorithms achieve $$ALG$$.
* If $$\gamma_{ALG}$$ exists, then $$\xi(s) = \sum_p c_p s^p$$ is such that $$c_2 \ne 0$$.

On even mixed-spin glasses, we know a few more things:
* If $$ALG \ne OPT$$, then AMP algorithms do NOT achieve OPT. This is regardless of whether $$\gamma_{ALG}$$ exists. Together with above, this implies $$ALG = OPT$$ if and only if AMP is optimal.
* If there's branching OGP, any overlap-concentrated algorithm will achieve at most $$ALG$$. (This comes from a study of branching OGP; see [[HS21]](https://arxiv.org/pdf/2110.07847.pdf)). For example, any approximately Lipschitz algorithms are overlap concentrated on even mixed-spin glasses. (This probably extends to any spin glass.)


Consider the following statements:
1. ALG = OPT
2. fRSB (equivalent to no-OGP)
3. AMP achieves ALG
4. AMP is obstructed to ALG
5. Branching OGP (as in HS21)
6. Coupled OGP (as in CGPR19)
7. AMP achieves OPT.

* 2 implies 1.
* 1 implies 3 and 7.
* 2 is disjoint from 5, and disjoint from 6.
* 5 implies 4 (and also 6).
* 6 implies not 7, and thereby not 1.
* It's possible that all problems satisfy either 2 or 6.


# Open questions

What is the $$k$$ in $$k$$RSB for a given CSP?

### Guessing game

It's conjectured that $$p$$-XOR, $$p > 2$$, is $$k$$RSB for some value $$k>0$$, but not fRSB. What is the number $$k$$ as a function of $$p$$?

[Juspreet] $$k \sim p$$? [Chris] $$k = C^p$$? [Kunal] $$k = \infty$$?


# Algorithms

* Lower bounds from Overlap Gap Property (OGP)
    * [[Gamarnik 2021]](https://www.pnas.org/doi/pdf/10.1073/pnas.2108492118) survey on OGPs
* Algorithms exploiting fRSB
    * [[Subag 2021]](https://arxiv.org/abs/1812.04588) - An algorithm that outputs near ground states of mixed spherical $$p$$-spin glasses under fRSB.
    * [[AMS20]](https://arxiv.org/abs/2001.00904) same, but for Ising spins: message passing algorithm to find near ground states of fRSB Ising spin glasses
    * [[AM20]](https://arxiv.org/abs/2009.11481) Implementation of the ideas above. Numerics for 2-XORSAT (SK) and 3-XORSAT (3-spin glass), the latter using the method in [[AC16]](https://arxiv.org/abs/1606.05335).
    * [[CD21]](https://arxiv.org/abs/2102.12043) studies the QAOA running to minimize mixed-spin Ising Hamiltonians.

* More about AMP and Lipschitz schemes: [[GJ21]](https://projecteuclid.org/journals/annals-of-probability/volume-49/issue-1/The-overlap-gap-property-and-approximate-message-passing-algorithms-for/10.1214/20-AOP1448.pdf), [[Sellke 2021]](https://arxiv.org/pdf/2105.03506.pdf), and [[BS21]](https://arxiv.org/pdf/2110.07847)
* Low-degree methods and Boolean circuits, see [[GJW21]](https://arxiv.org/abs/2109.01342v1))
* Sum-of-squares: see [[GJJPR21]](https://ieeexplore.ieee.org/abstract/document/9317995/) and [[KB19]](https://link.springer.com/article/10.1007/s10107-020-01558-2)
* Mixing time bounds:
    * for Ising models: [[BJ18]](https://link.springer.com/article/10.1007/s00220-018-3152-6), [[BB19]](https://www.sciencedirect.com/science/article/pii/S0022123619300278), [[EFZ20]](https://arxiv.org/abs/2007.08200)
    * for spherical models: [[JG19]](https://projecteuclid.org/journalArticle/Download?urlId=10.1214%2F18-AIHP897)


Other interesting papers:
* OGP for max k xor https://arxiv.org/pdf/1707.05386.pdf
* OGP on dense models https://arxiv.org/pdf/2004.12063.pdf
* AMP algorithms are optimal https://arxiv.org/pdf/2110.07847.pdf
* random k-SAT phase transition https://arxiv.org/pdf/2106.02129.pdf
* 1RSB breaking in Max-NAE-SAT https://arxiv.org/pdf/1904.08891.pdf and NAE-SAT https://arxiv.org/pdf/2011.14270.pdf
* Unbalanced cut problem https://arxiv.org/pdf/1707.09042.pdf



### Phase diagrams

For all mixed-spin models on the hypercube, we know that at high enough temperature, it is RS (need a citation), and at zero temperature, it is $$\infty-RSB$$ [[ACZ17]](https://par.nsf.gov/servlets/purl/10156904). But we suspect more:

On the SK model, we know of the dAT line (?) that suggests that at there is a single transition from RS to something else at inverse temperature $$\beta = 1$$. We know the something else is not RS, and we suspect it is fRSB.

For other pure $$p$$-spin models where $$p > 2$$, we know of a second transition [[Gardner85]](https://www.sciencedirect.com/science/article/abs/pii/0550321385903748?via%3Dihub). At the lowest temperature, we expect there to be gaps in the support of increasing points in $$\gamma$$ (i.e. overlap gaps); this is proven for even $$p$$. I'm not sure what happens at the intermediate temperature.

---

Inspired by conversations with so many people, including Bobak Kiani, Joao Basso, Robbie King, Nathan Ju, James Sud, and Adrian She.

More to add? Email me at [kmarw@uchicago.edu](mailto:kmarw@uchicago.edu).
