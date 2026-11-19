# AnisoMPM: Animating Anisotropic Damage Mechanics

- **Authors:** Joshuah Wolper, Yunuo Chen, Minchen Li, Yu Fang, Ziyin Qu, Jiecong Lu, Meggie Cheng, Chenfanfu Jiang
- **Published:** ACM Transactions on Graphics 39(4), SIGGRAPH 2020 · DOI [10.1145/3386569.3392428](https://doi.org/10.1145/3386569.3392428)
- **Source used:** the NSF public-access copy, read in full: <https://par.nsf.gov/servlets/purl/10180979>
- **Code:** <https://github.com/penn-graphics-research/ziran2020> (`Projects/anisofracture`)

## Summary

AnisoMPM makes cracks follow a material's grain: raw meat tears along muscle fibres, string cheese peels into strands, wood splits along the grain. Each particle stores one or two fibre directions. Those directions steer both where damage grows and how stiff the material is along the fibres.

## Key equations

**Damage evolution.** Damage $d \in [0,1]$ (0 healthy, 1 broken) grows when a driving force beats a geometric resistance. $\eta$ is a mobility (viscosity of crack growth):

$$\eta\,\dot d = (1-d)\,\tilde{\mathcal H} - \mathcal D_c, \qquad \mathcal D_c = d - l_0^2\,\Delta d$$

**Cracks never heal.** The driving force is the running maximum of a crack driving state:

$$\tilde{\mathcal H}(X, t) = \max_{s \in [0,t]} \tilde D\big(\text{state}(X, s)\big) \ge 0$$

**Driving state from tensile stress only.** $\langle x \rangle = (x + \lvert x\rvert)/2$ is the Macaulay bracket, $\sigma_i$ and $n_i$ are principal stresses and directions, $\zeta$ sets the slope:

$$\tilde D = \zeta \left\langle \Phi(\sigma^+) - 1 \right\rangle, \qquad \sigma^+ = \sum_i \langle \sigma_i \rangle\, n_i \otimes n_i$$

**Fibre-aware failure measure.** $\sigma_c$ is the critical stress, and $A$ a structural tensor built from fibre directions $a_1$, $a_2$:

$$\Phi(\sigma^+) = \frac{1}{\sigma_c^2}\,\big(A\sigma^+ : \sigma^+ A\big), \qquad A = I + \alpha_1 (a_1 \otimes a_1) + \alpha_2 (a_2 \otimes a_2)$$

- $\alpha_1 = \alpha_2 = 0$: isotropic material.
- $\alpha_1 \ne 0$, $\alpha_2 = 0$: one fibre direction (meat, cheese).
- Both non-zero: two fibre directions (woven or orthotropic materials).

**Explicit damage update per particle.** Cheaper than the implicit solve, and stable for small steps:

$$d_p^{n+1} = \min\!\left(1,\; d_p^n + \frac{\Delta t}{\eta} \Big\langle (1-d_p^n)\,\tilde D_p^n - \big(d_p^n - l_0^2\,\Delta d_p^n\big) \Big\rangle \right)$$

**Implicit alternative** (update $\tilde D$ first, then solve for $d^{n+1}$; discretised on the grid as a positive semi-definite system $(A + B)\,d = c$):

$$\left(1 + \frac{\Delta t}{\eta}\big(\tilde D^n + 1\big)\right) d^{n+1} - \frac{\Delta t}{\eta}\, l_0^2\, \Delta d^{n+1} = \frac{\Delta t}{\eta}\,\tilde D^n + d^n$$

The paper transfers damage with FLIP, which avoids PIC's smoothing and APIC's extra memory.

**Inextensible fibres (optional).** The fibres are held at fixed length by a constraint on the velocity gradient along each fibre: $(a \otimes a) : \nabla v = 0$.

## Takeaways for the grain solver

- **Fibre direction is one vector per particle.** For a "meat" or "bread crumb" material, store `fibre.npy` (N × 3) alongside position and radius. The tearing rule then needs only the particle's own stress and its fibre.
- **The explicit damage update is GPU-friendly.** It is a per-particle update plus one Laplacian over neighbours, which maps onto the existing neighbour list.
- **Tension-only damage** again means broken pieces still collide properly.
- **Damage as a running maximum** is a simple, deterministic rule: a particle's damage only ever goes up.

## Cautions

- The stress $\sigma$ comes from MPM's deformation gradient. A PBD particle has no stress tensor, so the grain solver would need a substitute (for example, the stretch of each particle's neighbour bonds).
- The equations were transcribed from a text extraction of the PDF, which scrambles fraction layouts. The $\Delta t/\eta$ placement in both updates was rebuilt from that layout; check it against Section 4.3 of the paper before implementing.
