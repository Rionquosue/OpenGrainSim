# TinyDEM: Minimal Open Granular DEM Code with Sliding, Rolling and Twisting Friction

- **Author:** Roman Vetter (ETH Zürich)
- **Published:** Computer Physics Communications 320, 109942, 2025 · DOI [10.1016/j.cpc.2025.109942](https://doi.org/10.1016/j.cpc.2025.109942)
- **Source used:** the arXiv preprint, read in full: <https://arxiv.org/abs/2507.12610>
- **Code:** C++11 with OpenMP, 3-clause BSD licence (linked from the paper)

## Summary

A compact, readable 3D discrete element method (DEM) solver for dry, soft, spherical grains of mixed sizes. It solves Newton's equations for translation and rotation, using quaternions for orientation. Contacts are inelastic and frictional, with sliding, rolling and twisting friction and full torque exchange; grains also collide with arbitrary triangle meshes. It replaces Cundall & Strack (1979) in this reading list: it implements the same contact model, extends it, and is openly available.

## Key equations

Quantities refer to grain $i$ relative to grain $j$; $x$ position, $v$ velocity, $\omega$ angular velocity, $R$ radius.

**Overlap and contact normal:**

$$d = x_i - x_j, \qquad \hat n = \frac{d}{\lVert d\rVert}, \qquad \delta = R_i + R_j - \lVert d\rVert \ge 0, \qquad u_n = -\delta\,\hat n$$

**Normal and shear forces (spring–dashpot):**

$$f_n = -k_n u_n - c_n v_n, \qquad f_s = -k_s u_s - c_s v_s, \qquad \lVert f_s\rVert \le \mu_s \lVert f_n\rVert$$

$u_s$ is the accumulated shear displacement of the static-friction spring (below), and $\mu_s$ the sliding friction coefficient.

**Hertz–Mindlin stiffness,** which grows with overlap through the contact radius $a$:

$$k_n = \tfrac43 E^* a, \qquad k_s = 8 G^* a, \qquad a = \sqrt{R^*\delta}$$

$$\frac{1}{E^*} = \frac{1-\nu_i^2}{E_i} + \frac{1-\nu_j^2}{E_j}, \qquad \frac{1}{G^*} = \frac{2-\nu_i}{G_i} + \frac{2-\nu_j}{G_j}, \qquad G_i = \frac{E_i}{2(1+\nu_i)}, \qquad \frac{1}{R^*} = \frac{1}{R_i} + \frac{1}{R_j}$$

**Damping from the coefficient of restitution** $e_n$ (with effective mass $1/m^* = 1/m_i + 1/m_j$):

$$\beta = \frac{1}{\sqrt{1 + (\pi/\ln e_n)^2}}, \qquad c_n = \sqrt{5\,m^* k_n}\;\beta, \qquad c_s = \sqrt{\tfrac16\, m^* k_s}\;\beta$$

The factor $\tfrac16$ in $c_s$ is the paper's own heuristic, tuned so that tangential and normal restitution match.

**Relative velocity at the contact point** ($r_i = R_i - \delta/2$ is the lever arm):

$$v = v_i - v_j + \hat n \times (r_i\omega_i - r_j\omega_j), \qquad v_n = (v\cdot\hat n)\,\hat n, \qquad v_s = v - v_n$$

**Rolling and twisting friction** (Kelvin–Voigt springs, each capped by its own Coulomb limit; TinyDEM sets $k_r = k_n$, $k_t = k_s$, $c_r = c_n$, $c_t = c_s$):

$$f_r = -k_r u_r - c_r v_r, \quad \lVert f_r\rVert \le \mu_r\lVert f_n\rVert, \qquad f_t = -k_t u_t - c_t v_t, \quad \lvert f_t\rvert \le \mu_t\lVert f_n\rVert$$

$$v_r = \left(\frac{1}{r_i} + \frac{1}{r_j}\right)^{-1} \omega\times\hat n, \qquad v_t = R^*\,\omega\cdot\hat n, \qquad \omega = \omega_i - \omega_j$$

**Static-friction springs** ($m \in \{s, r, t\}$). Each contact remembers its spring stretch $u_m = \int v_m\,dt$ since first touch. Every step:

1. Rotate the stored stretch into the current tangent plane, keeping its length ($m = s, r$):
$$u_m \leftarrow \lVert u_m\rVert \frac{u_m - (u_m\cdot\hat n)\hat n}{\lVert u_m - (u_m\cdot\hat n)\hat n\rVert}$$
2. If the force is within the Coulomb limit (sticking), extend the spring: $u_m \leftarrow u_m + \Delta t\, v_m$.
3. Otherwise (sliding), cap the force and reset the spring so it sits exactly on the limit:
$$f_m \leftarrow \mu_m \lVert f_n\rVert \frac{f_m}{\lVert f_m\rVert}, \qquad u_m \leftarrow -\frac{1}{k_m}\big(f_m + c_m v_m\big)$$

**Time integration** (semi-implicit Euler, symplectic when damping is off), with global damping $c_i$, $\gamma_i$:

$$a_i = \frac{1}{m_i}\sum_j \big(f_n^{ij} + f_s^{ij}\big) + g - c_i v_i, \qquad v_i \leftarrow v_i + \Delta t\, a_i, \qquad x_i \leftarrow x_i + \Delta t\, v_i$$

Angular velocity is updated the same way from the torques, and orientation by quaternion rotation.

**Default time step** (resolves a head-on collision with about a dozen steps):

$$\Delta t = R_{min}\sqrt{\frac{\rho\,(1-\nu^2)}{E}}$$

## Takeaways for the grain solver

- **The friction rule, spelled out completely.** The three-step spring update (project, stick, or cap and reset) is the full Coulomb model with memory. The grain solver's PBD friction is the memoryless version; if tall piles creep, adding a per-contact stored offset `u_s` to the contact list, updated exactly like this, is the fix.
- **Rolling friction stops piles flowing like ball bearings.** Spheres with only sliding friction roll too freely, so piles come out too flat. A rolling-friction coefficient $\mu_r$ is the standard way to get realistic angles of repose with spherical grains, and is worth adding to the grain solver's settings.
- **Restitution as the artist control.** $e_n$ (how bouncy grains are) converts directly to damping through $\beta$, which is more intuitive than raw damping values.
- **Contact forces are equal and opposite** ($f^{ij} = -f^{ji}$), so each contact can be computed once and scattered to both grains, matching the concept sheet's half-shell neighbour search.

## Cautions

- Explicit DEM needs time steps short enough to resolve each collision (the formula above gives microseconds for stiff sand), far smaller than PBD substeps. Use TinyDEM's friction logic, not its time stepping.
- Equations were transcribed from a text extraction of the preprint; check them against the published version.
