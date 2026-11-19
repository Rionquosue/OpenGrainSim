# Fluid Simulation for Computer Graphics (2nd edition)

- **Author:** Robert Bridson
- **Published:** A K Peters / CRC Press, 2015 (copyright 2016) · ISBN 978-1-4822-3283-7
- **Link:** <https://www.routledge.com/Fluid-Simulation-for-Computer-Graphics/Bridson/p/book/9781482232837>
- **Source used:** read from a copy supplied by the repository owner (not stored in this repository). Sections read: 1 (equations), 2.3 (time steps), 3.3 (time-step size), 5.1–5.3 (pressure), 7.3 and 7.6 (particle transfers, PIC/FLIP), 8.2 (ghost-fluid free surface), 8.5 (surface tension), 10.3 and 10.5 (viscosity). Book page numbers are given. This replaces an earlier version written from memory; the recalled equations were correct, and several practical details below are new.

## Summary

The standard text on grid and hybrid particle–grid liquids for graphics. The method: split each step into advection, body forces and a pressure projection that makes the flow incompressible, on a staggered (MAC) grid. Velocities can be carried on particles (PIC/FLIP) to avoid the smoothing of grid advection. The book insists on real SI units throughout, so parameters keep their physical meaning when a scene is resized. It does not cover APIC (published the same year); see the APIC extract.

## Key equations

**Incompressible Navier–Stokes** (eqs. 1.1–1.2, p. 3; $\nu$ kinematic viscosity):

$$\frac{\partial u}{\partial t} + u\cdot\nabla u + \frac{1}{\rho}\nabla p = g + \nu\nabla\cdot\nabla u, \qquad \nabla\cdot u = 0$$

**Variable-viscosity form** (eq. 10.4, p. 144; $\mu$ dynamic viscosity). Equal to the above only when $\mu$ is constant; the book recommends always using it, since the simplified form does not conserve angular momentum with varying viscosity and complicates the free-surface condition:

$$\frac{Du}{Dt} + \frac{1}{\rho}\nabla p = g + \frac{1}{\rho}\nabla\cdot\big(\mu(\nabla u + \nabla u^T)\big)$$

with the Newtonian viscous stress $\tau = 2\mu D + \lambda\operatorname{tr}(D)\,\delta$, $D = \tfrac12(\nabla u + \nabla u^T)$, and $\lambda = 0$ for incompressible flow (eq. 10.3).

**Splitting** (eqs. 2.8–2.10, p. 19): advect ($Du/Dt = 0$), add body forces ($u \leftarrow u + \Delta t\,g$, forward Euler), then project so that $\nabla\cdot u = 0$.

**Boundary conditions** (p. 13–15):

- Solid wall, inviscid ("no-stick"): $u\cdot\hat n = u_{solid}\cdot\hat n$; tangential motion is free.
- Solid wall, viscous ("no-slip"): $u = u_{solid}$; worth using only for high-viscosity goop.
- Free surface: $p = 0$.
- Separation (Batty et al. 2007): $u\cdot\hat n \ge u_{solid}\cdot\hat n$, letting liquid leave a wall but not enter it.

**Time step** (eq. 3.2, p. 34): limit how far a trajectory travels per step, with gravity included in the speed estimate so the bound is never zero:

$$\Delta t \le \frac{5\Delta x}{u_{max}}, \qquad u_{max} = \max\lvert u^n\rvert + \sqrt{5\Delta x\,\lvert g\rvert}$$

Semi-Lagrangian advection is unconditionally stable, so this is an accuracy limit. Within a step, particle trajectories can be traced in local substeps of $\Delta\tau = C\Delta x/(\lVert u\rVert + \epsilon)$ (p. 109). Also clamp the step so it never passes the end of the animation frame (p. 20).

**Pressure update on the MAC grid** (eq. 5.2, p. 69), for example the $x$ component:

$$u^{n+1}_{i+1/2,j,k} = u_{i+1/2,j,k} - \Delta t\,\frac{1}{\rho}\,\frac{p_{i+1,j,k} - p_{i,j,k}}{\Delta x}$$

**Pressure equation** (eq. 5.8, p. 75), for each fluid cell, a discrete Poisson problem $-\tfrac{\Delta t}{\rho}\nabla\cdot\nabla p = -\nabla\cdot u$:

$$\frac{\Delta t}{\rho}\,\frac{6p_{i,j,k} - p_{i\pm1,j,k} - p_{i,j\pm1,k} - p_{i,j,k\pm1}}{\Delta x^2} = -\frac{u_{i+1/2} - u_{i-1/2} + v_{j+1/2} - v_{j-1/2} + w_{k+1/2} - w_{k-1/2}}{\Delta x}$$

(the neighbour terms summed over all six neighbours), assembled as $Ap = b$ (eq. 5.9) with air neighbours set to the free-surface value and solid faces handled by the wall condition.

**Solving it:** preconditioned conjugate gradient with Modified Incomplete Cholesky (MICCG(0), tuning constant $\tau = 0.97$, safety constant $\sigma = 0.25$; pp. 79–89). Stop when

$$\lVert b - Ap\rVert \le \text{tol}\,\lVert b\rVert, \qquad \text{tol} \approx 10^{-6}$$

The pressure solve is usually where most simulation time goes (p. 81).

**Ghost-fluid free surface** (§8.2, pp. 127–129). With level set $\phi$ ($\phi \le 0$ in water), place $p = 0$ at the actual surface between a water cell $i$ and an air cell $i+1$ instead of at the air cell's centre:

$$\theta = \frac{\phi_i}{\phi_i - \phi_{i+1}}, \qquad p^G_{i+1} = \frac{\phi_{i+1}}{\phi_i}\,p_i, \qquad u^{n+1}_{i+1/2} = u_{i+1/2} - \frac{\Delta t}{\rho}\,\frac{\phi_{i+1} - \phi_i}{\phi_i}\,\frac{p_i}{\Delta x}$$

This removes the voxel staircase from the free surface (sub-cell ripples otherwise do not move). It increases the matrix diagonal; guard against $\phi_i$ near 0.

**Surface tension** (§8.5, pp. 131–132): a pressure jump at the surface proportional to mean curvature, used as the ghost-fluid surface value instead of 0:

$$p = \gamma\kappa, \qquad \kappa = \nabla\cdot\nabla\phi, \qquad \gamma_{water/air} \approx 0.073\ \text{J/m}^2, \qquad \Delta t \le O\!\left(\Delta x^{3/2}\sqrt{\rho/\gamma}\right)$$

The time-step limit comes from treating surface tension explicitly.

**PIC/FLIP** (§7.6, pp. 116–119):

- Particle-to-grid as a normalised weighted average (eq. 7.2): $u_{i+1/2,j,k} = \dfrac{\sum_p u_p\,k(x_p - x_{i+1/2,j,k})}{\sum_p k(x_p - x_{i+1/2,j,k})}$, with a trilinear hat kernel $k$.
- FLIP adds the grid's change in velocity to each particle instead of replacing it; PIC replaces it. Blend with a small $\alpha$:

$$u_p^{new} = \alpha\,\text{interp}(u_{grid}^{new}, x_p) + (1-\alpha)\big(u_p^{old} + \text{interp}(\Delta u_{grid}, x_p)\big), \qquad \alpha = \frac{6\Delta t\,\nu}{\Delta x^2}$$

  The second relation picks $\alpha$ so PIC's numerical smoothing matches a physical viscosity; if it exceeds 1, use PIC plus an explicit viscosity step.
- Start with 8 particles per grid cell (4 in 2D), from a jittered grid; reseed cells with too few, delete above about 12.

**Viscosity time step** (eq. 10.5, p. 151): explicit viscosity is stable only for

$$\Delta t < \frac{\Delta x^2\,\rho}{12\,\mu_{max}}$$

which is an order of magnitude stricter than advection needs; the book therefore integrates viscosity implicitly (backward Euler), which has no such limit.

## Takeaways for the grain solver

- **Grid liquid recipe, if phase 2 goes grid-based:** split step, MAC grid, MICCG pressure solve with relative tolerance $10^{-6}$, ghost-fluid free surface, and a choice of PIC, FLIP (or a blend of the two) or APIC transfers (APIC from its own extract, since this edition predates it).
- **Time step:** the gravity-aware speed estimate ($\max\lvert u\rvert + \sqrt{5\Delta x g}$) avoids a zero or undefined step at rest and suits the grain solver's substep chooser too.
- **Viscous semisolids need implicit viscosity.** Mud and slime have large $\mu$, where the explicit limit would force tiny steps; use the variable-viscosity form, integrated implicitly.
- **SI units everywhere.** The book's strongest practical advice; it matches the concept sheet's plan to keep physical parameters (friction angle, restitution, density) meaningful at any scale.
- **Particle budget:** 8 particles per cell is the standard starting point, which sets memory estimates for a FLIP or APIC liquid.

## Cautions

- Only the sections listed were read; smoke, fire, turbulence, shallow water, ocean and vortex chapters are not covered here.
- Equations were transcribed from a text extraction; the multi-line pressure equation (5.8) is condensed above. Check against the pages cited.
