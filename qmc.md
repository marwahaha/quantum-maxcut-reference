---
title: Quantum MaxCut
---

# Quantum MaxCut

## Definition


$$
H^{QMC} = \frac{1}{2} \sum_{(i,j) \in E} w_{ij} \left( I_i \otimes I_j - X_i \otimes X_j - Y_i \otimes Y_j - Z_i \otimes Z_j \right) =  \sum_{(i,j) \in E} 2 w_{ij} \vert \psi^{-} \rangle_{ij}  \langle \psi^{-} \vert_{ij}\,,
$$

where $$w_{ij} \ge 0$$, and $$\vert \psi^{-} \rangle = \frac{1}{\sqrt{2}} (\vert 01\rangle - \vert 10\rangle)$$ is the singlet state. 

This Hamiltonian is rotation-invariant. The maximum energy is non-negative, and at most $2\sum_{(i,j) \in E} w_{ij}$.

## Hardness
QMA-complete for positive-weighted graphs [[PM15]](https://arxiv.org/abs/1506.04014).

Under a conjecture, getting a $$(0.956+\epsilon)$$-approximation is as hard as Unique Games [[HNPTW21]](https://arxiv.org/abs/2111.01254).

## Algorithms

TBD
