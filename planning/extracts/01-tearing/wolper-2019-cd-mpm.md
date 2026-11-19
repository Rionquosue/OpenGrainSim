# CD-MPM: Continuum Damage Material Point Methods for Dynamic Fracture Animation

- **Authors:** Joshuah Wolper, Yu Fang, Minchen Li, Jiecong Lu, Ming Gao, Chenfanfu Jiang
- **Published:** ACM Transactions on Graphics 38(4), SIGGRAPH 2019 · DOI [10.1145/3306346.3322949](https://doi.org/10.1145/3306346.3322949)
- **Source used:** the NSF public-access copy, read in full: <https://par.nsf.gov/servlets/purl/10108350>
- **Code:** <https://github.com/squarefk/ziran2019>

## Summary

The paper simulates tearing and crumbling (bread, cookies, jello, watermelon) on MPM particles without ever cutting a mesh. Each particle carries a damage value; damaged material simply stops resisting tension. It offers two routes that can be combined:

- **PFF-MPM:** a phase-field damage value that spreads smoothly through the material.
- **NACC-MPM:** a cheaper plasticity model (Non-Associated Cam-Clay) whose hardening makes material crumble without any damage field.

## Key equations

**Crack surface as a smooth field.** The crack surface integral is replaced by a volume integral over a phase field $c \in [0,1]$ ($c = 1$ healthy, $c = 0$ fully broken). $\mathcal{G}$ is the energy release rate and $l_0$ the crack width:

$$\int_{\Gamma} \mathcal{G}\, dA \;\approx\; \int_{\Omega_0} \left( \frac{(c-1)^2}{4 l_0} + l_0 \,\lvert \nabla_X c \rvert^2 \right) \mathcal{G}\, dX$$

The paper uses $l_0 = 0.5\,\Delta x$ (half a grid cell).

**Only tension is weakened.** Elastic energy is split into a tensile part $\Psi^+$ and a compressive part $\Psi^-$, and only the tensile part is degraded:

$$\hat\Psi(F) = g(c)\,\Psi^+(F) + \Psi^-(F), \qquad g(c) = (1-r)\,c^2 + r, \quad r = 0.001$$

The small residual $r$ keeps fully broken material from letting $F$ grow without bound and overflow.

**Total free energy density:**

$$\Psi(F, c) = g(c)\,\Psi^+(F) + \Psi^-(F) + \left( \frac{(c-1)^2}{4 l_0} + l_0 \lvert \nabla^X c \rvert^2 \right)\mathcal{G}$$

**Damage update (implicit, per step).** With mobility $M_c = \mathcal{G} / (2 R_c l_0)$:

$$\left( \frac{4 l_0 M_c (1-r)\,\Psi^+}{\mathcal{G}} + M_c + \frac{1}{\Delta t} \right) c^{n+1} - 4 l_0^2 M_c \nabla^2 c^{n+1} = M_c + \frac{c^n}{\Delta t}$$

This is a linear, heat-equation-like solve on the grid each step.

**Simple local alternative** (the paper notes it gives mesh-dependent cracks):

$$c^{n+1} = \min\!\left(c^n,\; 1 - H_s \left(1 - \frac{\sigma_f}{\sigma_M}\right)\right)$$

**NACC yield surface** (an ellipse in pressure $p$ and shear stress $q$):

$$y(p, q) = q^2(1 + 2\beta) + M^2 (p + \beta p_0)(p - p_0) \le 0, \qquad p_0 = K \sinh\!\big(\xi \max(-\alpha, 0)\big)$$

- $\beta$ is cohesion: the material can take tension down to $-\beta p_0$.
- $M$ sets friction; $K$ is the bulk modulus; $\xi$ sets how fast it hardens.
- $\alpha$ tracks hardening. When the material is stretched past the yield surface, the surface shrinks and the particle crumbles.

**Per-particle state:** mass, position, velocity, APIC matrix $C_p$, deformation gradient $F_p$, phase field $c_p$, NACC hardening $\alpha_p$.

## Takeaways for the grain solver

- **Tearing needs per-particle state.** Bread and meat need a deformation gradient (or a substitute) and a damage value on each particle. The grain solver's PBD particles have neither, so tearing is a separate material model, not a friction setting.
- **NACC is the cheap path.** It needs no extra grid solve, only a per-particle projection after the stress update. It fits the "semisolid" material planned for phase 2 better than full phase-field damage.
- **Damage only weakens tension.** Torn pieces still push against each other, so they never pass through one another.
- **Crack width is tied to grid size** ($l_0 = 0.5\Delta x$), so results change with resolution unless parameters are rescaled.

## Cautions

- MPM uses a background grid and a particle–grid transfer every step: a different engine from position-based dynamics. Adopting it means a second solver path, not an extension of the PBD loop.
- The implicit damage solve is a sparse linear system per step, which is harder to make fast and deterministic on the GPU than whole-array PBD operations.
