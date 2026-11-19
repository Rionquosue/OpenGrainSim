# The Affine Particle-In-Cell Method (APIC)

- **Authors:** Chenfanfu Jiang, Craig Schroeder, Andrew Selle, Joseph Teran, Alexey Stomakhin
- **Published:** ACM Transactions on Graphics 34(4), Article 51, SIGGRAPH 2015 · DOI [10.1145/2766996](https://doi.org/10.1145/2766996)
- **Source used:** the authors' copy, read in full: <https://www.math.ucdavis.edu/~jteran/papers/JSSTS15.pdf>. The journal follow-up was also read: Jiang, Schroeder, Teran, "An angular momentum conserving affine-particle-in-cell method", JCP 2017, <https://arxiv.org/abs/1603.06188>.

## Summary

PIC is stable but smears motion; FLIP keeps detail but is noisy. APIC gives each particle a small affine velocity field (a 3×3 matrix) instead of a single velocity. That keeps rotation and shear across the particle–grid round trip, conserving angular momentum, with almost no dissipation and without FLIP's noise. It works for incompressible liquids and for MPM.

## Key equations

Notation: $w_{ip}$ is the interpolation weight between grid node $i$ and particle $p$; $m$ is mass; $v$ velocity; $x$ position.

**Particle to grid (P2G):**

$$m_i^n\, v_i^n = \sum_p w_{ip}\, m_p \Big(v_p^n + B_p^n \big(D_p^n\big)^{-1} (x_i - x_p^n)\Big), \qquad m_i^n = \sum_p w_{ip}\, m_p$$

with the inertia-like matrix

$$D_p^n = \sum_i w_{ip}\,(x_i - x_p^n)(x_i - x_p^n)^{T}$$

For the common B-spline kernels $D_p$ is constant: $\tfrac14 \Delta x^2 I$ (quadratic) and $\tfrac13 \Delta x^2 I$ (cubic). The product $C_p = B_p D_p^{-1}$ is the particle's velocity gradient.

**Grid to particle (G2P):**

$$v_p^{n+1} = \sum_i w_{ip}\, \tilde v_i^{n+1}, \qquad B_p^{n+1} = \sum_i w_{ip}\, \tilde v_i^{n+1} (x_i - x_p^n)^{T}$$

**Why it matters:** affine velocity fields (translation, rotation, shear) survive both transfers exactly, so a spinning blob keeps spinning instead of slowing down as it would under PIC.

**MAC grids (liquids):** store one vector $c_{pa}$ per face direction $a$ per particle instead of the full matrix, and transfer each velocity component to its own face grid.

## Takeaways for the grain solver

- **APIC is one of three transfer options** for a grid liquid, alongside PIC and FLIP. Choose it for stable, swirling motion without FLIP's noise; FLIP suits large, splashy shots. Blender's own liquid offers both FLIP and APIC, and its manual describes APIC as more stable and better at keeping swirls.
- **Cost:** 9 extra floats per particle ($C_p$) and slightly more work per transfer. On the GPU this is a small, fully parallel per-particle cost.
- **Precision note:** $C_p$ is a velocity gradient; storing it in 32-bit is fine, consistent with the solver's "no positions below 32-bit" rule.

## Cautions

- APIC still needs a pressure projection for incompressible liquids; it only replaces the particle–grid transfers.
- The JCP follow-up proves angular-momentum conservation more carefully; read it if exact conservation matters.
