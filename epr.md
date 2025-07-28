# EPR Hamiltonian

## Definition


$$
H^{EPR} = \frac{1}{2} \sum_{(i,j) \in E} w_{ij} \left( I_i \otimes I_j + X_i \otimes X_j - Y_i \otimes Y_j + Z_i \otimes Z_j \right) =  \sum_{(i,j) \in E} 2 w_{ij} \vert \phi^{-} \rangle_{ij}  \langle \phi^{-} \vert_{ij}\,,
$$

where $$w_{ij} \ge 0$$, and $$\vert \phi^{+} \rangle = \frac{1}{\sqrt{2}} (\vert 00\rangle + \vert 11\rangle)$$. 

This Hamiltonian is rotation-invariant. The maximum energy is non-negative, and at most $2\sum_{(i,j) \in E} w_{ij}$.

## Hardness
In StoqMA for positive-weighted graphs [[PM15]](bib#PM15).

Could be in P.

## Algorithms

TBD
