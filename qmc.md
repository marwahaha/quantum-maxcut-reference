# Quantum MaxCut

## Definition


$$
H^{QMC} = \frac{1}{2} \sum_{(i,j) \in E} w_{ij} \left( I_i \otimes I_j - X_i \otimes X_j - Y_i \otimes Y_j - Z_i \otimes Z_j \right) =  \sum_{(i,j) \in E} 2 w_{ij} \vert \psi^{-} \rangle_{ij}  \langle \psi^{-} \vert_{ij}\,,
$$

where $$w_{ij} \ge 0$$, and $$\vert \psi^{-} \rangle = \frac{1}{\sqrt{2}} (\vert 01\rangle - \vert 10\rangle)$$ is the singlet state. 

This Hamiltonian is rotation-invariant. The maximum energy is non-negative, and at most $2\sum_{(i,j) \in E} w_{ij}$.

## Hardness
QMA-complete for positive-weighted graphs [[PM15]](bib#PM15).

Under a conjecture, getting a $$(0.956+\epsilon)$$-approximation is as hard as Unique Games [[HNPTW22]](bib#HNPTW22).

## Algorithms

| Reference   | Value      | Graph Type         | Techniques                                     |
|-------------|------------|--------------------|-------------------------------------------|
| [[GP19]](bib#GP19)    | 0.498      | General graphs     |                    |
| [[PT22]](bib#PT22)     | 1/2        | General graphs     |                    |
| [[HTPG24]](bib#HTPG24)     | 0.526      | General graphs     |                    |
| [[AGM20]](bib#AGM20)    | 0.531      | General graphs     |                                           |
| [[PT21]](bib#PT21)      | 0.533      | General graphs     |                                           |
| [[Lee22]](bib#Lee22)     | 0.562      | General graphs     |                                           |
| [[LP24]](bib#LP24)      | 0.595      | General graphs     |                                           |
| [[JKKSW24]](bib#JKKSW24)    | 0.599      | General graphs     |              |
| [[GSS25]](bib#GSS25)   | 0.603      | General graphs     |              |
| [[Kin23]](bib#Kin23)     | 0.582      | Triangle-free      |                                           |
| [[GSS25]](bib#GSS25)    | 0.61383    | Triangle-free      |                                           |
| [[Kin23]](bib#Kin23)     | √1/2 ≈ 0.707 | Bipartite graphs |                                           |
| [[GSS25]](bib#GSS25)    | 0.8162       | Bipartite graphs   |                                           |
| [[ALMPS25]](bib#ALMPS25)  | .611        | General Graphs


<div style="padding-bottom: 300px"></div>
