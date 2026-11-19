# Animating Sand as a Fluid

- **Authors:** Yongning Zhu, Robert Bridson
- **Published:** ACM Transactions on Graphics 24(3), SIGGRAPH 2005, pp. 965–972 · DOI [10.1145/1073204.1073298](https://doi.org/10.1145/1073204.1073298)
- **Source used:** the authors' copy, read in full: <https://www.cs.ubc.ca/~rbridson/docs/zhu-siggraph05-sandfluid.pdf>

## Summary

The first paper to simulate sand with PIC/FLIP. Grains are not tracked individually; sand is a continuum that flows like a liquid where it yields and moves rigidly where it does not. Particles carry the material; a grid solves pressure and friction. It also introduced FLIP to incompressible liquids in graphics.

## Key equations

**Mohr–Coulomb yield (von Mises form).** With mean stress $\sigma_m = \operatorname{tr}(\sigma)/3$ and equivalent shear stress $\bar\sigma = \lvert \sigma - \sigma_m\delta \rvert_F / \sqrt2$, sand does not yield while:

$$\sqrt3\,\bar\sigma < \sin\phi\;\sigma_m$$

$\phi$ is the friction angle; $\sin\phi$ plays the role of the Coulomb friction coefficient.

**Frictional stress where sand flows.** $D = (\nabla u + \nabla u^{T})/2$ is the strain rate and $p$ the pressure from the incompressibility solve:

$$\sigma_f = -\sin\phi \; p \;\frac{D}{\sqrt{1/3}\,\lvert D\rvert_F}$$

This is the point on the yield surface that most directly resists the sliding.

**Rigidity test per cell.** The stress needed to stop all sliding in one cell of size $\Delta x$ within one step:

$$\sigma_{rigid} = -\frac{\rho\, D\, \Delta x^2}{\Delta t}$$

If $\sigma_{rigid}$ satisfies the yield condition, the cell is marked rigid. Connected rigid cells are then projected to rigid-body motion, preserving their linear and angular momentum. Other cells get the frictional update $u \mathrel{+}= \frac{\Delta t}{\rho}\nabla\cdot\sigma_f$.

**Wall friction** (Coulomb, applied when removing the normal velocity at a boundary; $\mu$ is the sand–wall friction coefficient):

$$u_T \leftarrow \max\!\left(0,\; 1 - \frac{\mu\,\lvert u\cdot n\rvert}{\lvert u_T\rvert}\right) u_T$$

**Cohesion** adds a constant $c > 0$ to the yield condition: $\sqrt3\,\bar\sigma < \sin\phi\;\sigma_m + c$.

**PIC/FLIP velocity update.**

- PIC: particles take the interpolated new grid velocity. Smooth, but adds numerical viscosity.
- FLIP: particles add the interpolated *change* in grid velocity. Keeps detail, but noisier.
- A weighted blend tunes the amount of numerical viscosity. The paper prefers PIC's damping for sand and FLIP for water.

## Takeaways for the grain solver

- **The wall-friction formula is directly reusable.** It is the same velocity-level Coulomb rule the grain solver needs at colliders: remove the normal component, then shrink the tangential component by $\mu\,\lvert v_n\rvert$, clamping to zero for static friction.
- **Rigid-or-flowing is the continuum view of piles.** The grain solver gets the same behaviour from per-contact static friction; this paper shows how to get it for millions of grains when individual grains are too costly.
- **Sand prefers damping, water prefers detail.** For the phase 2 liquid, a FLIP-style detail-keeping update is right for water, and a damped one for mud and semisolids.

## Cautions

- No elasticity: piles cannot store stress, so very tall columns behave less realistically (the paper notes the "hourglass" pressure limit is not reproduced).
- Grid-based: resolution is set by the grid cell, not by grain size.
