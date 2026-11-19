# Remeshing-Free Graph-Based Finite Element Method for Fracture Simulation

- **Authors:** Avirup Mandal, Parag Chaudhuri, Subhasis Chaudhuri
- **Published:** Computer Graphics Forum 42(1), 2023, pp. 117–134 · DOI [10.1111/cgf.14725](https://doi.org/10.1111/cgf.14725) (arXiv preprint 2103.14870)
- **Source used:** the authors' copy, read in full: <https://avirupmandal.github.io/files/CGF_paper_main_2022.pdf>
- **Project page:** <https://avirupmandal.github.io/fracture-cgf2022>

## Summary

The method tears tetrahedral meshes (a loaf of bread, a meat-filled loaf in a follow-up) without remeshing. It treats the mesh's edges as a graph and rewrites the elastic energy purely in terms of edge lengths. Fracture then means marking edges as broken; the system matrix keeps its size, so no new elements or vertices are created.

## Key equations

**Standard FEM setup.** Displacement is interpolated from nodes, and dynamics are $M\ddot u + H\dot u + f_{int} = f_{ext}$.

**Stress along each edge.** Each tetrahedron's stress is projected onto its six edge directions. For an edge from node $i$ to node $j$ with unit direction $\hat d_{ij}$:

$$\sigma^e_{ij} = \hat d_{ij}^{\,T}\, \sigma^e\, \hat d_{ij}$$

Collecting the six edge stresses gives $\sigma^e_d = T\,\sigma^e_c$, where $T$ is a $6 \times 6$ transformation built from the edge direction cosines ($\cos\phi_x = \hat d_{ij}\cdot\hat x$, and so on).

**Non-local smoothing.** Edge stresses are averaged over nearby tetrahedra with a kernel $\omega$ of support radius $R_d$ (linear in the paper), so cracks do not depend on single elements:

$$\sigma^{e}_{cen} = \sum_{r \in R_d} \omega(r - r_{cen})\,\sigma^e_c(r)$$

**Breaking rule.** Each edge has a damage flag $\zeta_{ij} \in \{0, 1\}$ and never heals:

$$\zeta_{ij} = \begin{cases} 0 & \sigma^{*}_{e_{ij}} < \sigma_{thres} \\ 1 & \sigma^{*}_{e_{ij}} \ge \sigma_{thres} \end{cases}$$

Once an edge breaks, its normal stress becomes zero in every tetrahedron sharing it.

**Stress after fracture.** With $\zeta$ the diagonal matrix of edge flags (zero for broken edges in the paper's convention of transmitted stress):

$$\sigma^e_{c,frac} = T^{-1}\,\zeta^{T}\,T\,\sigma^e_c$$

**Ductile option.** Plasticity uses $F = F_e F_p$ with a flow amount $\beta$ (user parameters: yield stress, flow rate $\nu$, hardening $K$):

$$\beta = \operatorname{clamp}\!\left( \frac{\nu\,\big(\lVert\sigma_e\rVert_2 - \sigma_{thres} - K\delta\big)}{\lVert\sigma_e\rVert_2},\; 0,\; 1 \right)$$

$\delta$ starts at zero and accumulates plastic flow, so material hardens or softens over time.

## Takeaways for the grain solver

- **"Break a bond" is a clean model for tearing.** For a particle solver, the closest equivalent is a bond network between neighbouring particles: each bond carries a stress, breaks past a threshold, and never re-forms. This fits the existing neighbour list.
- **Average before you break.** Smoothing the stress over a neighbourhood stops cracks from following the particle layout. A particle version would average bond stress over neighbours within a fixed radius.
- **The system never changes size.** Broken bonds are flagged, not removed, which keeps GPU arrays fixed-size and the run deterministic.

## Cautions

- This is a mesh method (tetrahedra, implicit FEM). Only the bond-breaking idea transfers to particles; the edge-energy derivation does not.
- The exact orientation of the $\zeta$ convention (whether 1 marks "broken" or "transmits") differs between the breaking rule and the stress formula in the text extraction. Confirm against equations 15–18 in the paper.
