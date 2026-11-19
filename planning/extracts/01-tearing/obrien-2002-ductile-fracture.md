# Graphical Modeling and Animation of Ductile Fracture

- **Authors:** James F. O'Brien, Adam W. Bargteil, Jessica K. Hodgins
- **Published:** ACM Transactions on Graphics 21(3), SIGGRAPH 2002, pp. 291–294
- **Links:** <https://escholarship.org/uc/item/1pb4c21m> · <https://cal.cs.umbc.edu/Papers/Obrien-2002-GMA>
- **Source used:** the eScholarship copy, read in full (supplied by the repository owner; not stored in this repository). This replaces an earlier version of this extract that was written from memory; the recalled equations turned out correct apart from notation.

## Summary

A short paper that adds plasticity to O'Brien and Hodgins' 1999 brittle-fracture method (tetrahedral finite elements, cracks started and grown at nodes, remeshing as cracks advance). Material can now stretch and bend permanently before it breaks, so it tears instead of shattering. The paper's point is that even plastic deformation too small to see changes how things break: a little plasticity turns a wall that shatters into one that tears into a few large pieces.

## Brittle versus ductile

- **Elastic limit** (yield point): beyond it, deformation becomes permanent.
- **Failure threshold:** beyond it, the material fractures.
- **Brittle:** the two nearly coincide. Elastic energy is stored and released at fracture, driving the crack on, so brittle materials shatter.
- **Ductile:** failure comes well after yield. Plastic flow absorbs energy, so more work is needed to push a crack, and ductile materials tear.

## Key equations

**Strain split.** Total strain $\varepsilon$ (Green's strain, computed from the current shape) is plastic plus elastic; plastic strain is part of the material's state and starts at zero:

$$\varepsilon = \varepsilon^{p} + \varepsilon^{e}$$

**Elastic strain deviation** (shape change only; removing the volume part makes plasticity insensitive to pressure and keeps volume constant):

$$\varepsilon' = \varepsilon^{e} - \frac{\operatorname{Tr}(\varepsilon^{e})}{3}\, I$$

**Yield condition** (von Mises), with the elastic limit $\gamma_1$ and the Frobenius norm:

$$\gamma_1 < \lVert \varepsilon' \rVert$$

**Plastic flow** when yielding: the plastic strain moves toward the elastic strain deviation by the amount it exceeds the limit:

$$\Delta\varepsilon^{p} = \frac{\lVert\varepsilon'\rVert - \gamma_1}{\lVert\varepsilon'\rVert}\,\varepsilon'$$

**Plastic limit** $\gamma_2$, applied every step:

$$\varepsilon^{p} := \big(\varepsilon^{p} + \Delta\varepsilon^{p}\big)\,\min\!\left(1,\; \frac{\gamma_2}{\lVert \varepsilon^{p} + \Delta\varepsilon^{p}\rVert}\right)$$

The paper's picture: the plastic strain is dragged by the total strain on a rope of length $\gamma_1$, inside a fence of radius $\gamma_2$. The elastic force pulls the total strain toward the plastic strain, but not the other way round, so the final shape depends on the history of deformation.

**Fracture** uses the 1999 method unchanged, with element stresses computed from the elastic strain $\varepsilon^{e}$ instead of the total strain. The 1999 separation tensor is not restated in this paper.

## Parameter values shown (thin walls struck by a heavy weight)

| $\gamma_1$ | $\gamma_2$ | Result |
| --- | --- | --- |
| 0 | 0 | purely brittle: shatters |
| 0.001 | 0.006 | breaks into several large pieces |
| 0.001 | 0.018 – 0.162 | increasingly torn, with a small part torn away largely intact at 0.162 |
| 0.001 | 0.486 | obvious bending and tearing |
| 0.00044 – 0.0044 | 0.486 | varying how early plastic flow starts |

The examples also include a cylinder pulled and twisted apart, a torn thin sheet, and a clay slab punctured by a sphere compared with a real one.

## Takeaways for the grain solver

- **Two artist parameters for tearing food:** an elastic limit ($\gamma_1$, how far it stretches before deforming permanently) and a plastic limit ($\gamma_2$, how much permanent stretch it can take). Bread and meat sit at small $\gamma_1$ and large $\gamma_2$.
- **Small numbers matter.** The paper's values are tiny (0.001) yet change the result completely, so these controls need a logarithmic slider or a preset per material.
- **Particle version.** With bonds between neighbouring particles instead of tetrahedra, the same update applies per bond: the bond's rest length moves toward its current length by the excess over $\gamma_1$, capped at $\gamma_2$; the bond breaks past a separate toughness. This fits the bond-network tearing idea in the Mandal extract.
- **Plastic flow ignores pressure** here (von Mises). That suits dough and meat; sand needs the pressure-dependent Drucker–Prager model instead (see the 03 folder).

## Cautions

- The fracture criterion and remeshing are in the 1999 paper, which is not in this reading list.
- No timing or stability figures are given.
