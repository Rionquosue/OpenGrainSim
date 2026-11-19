# Drucker-Prager Elastoplasticity for Sand Animation

- **Authors:** Gergely Klár, Theodore Gast, Andre Pradhana, Chuyuan Fu, Craig Schroeder, Chenfanfu Jiang, Joseph Teran
- **Published:** ACM Transactions on Graphics 35(4), SIGGRAPH 2016 · DOI [10.1145/2897824.2925906](https://doi.org/10.1145/2897824.2925906)
- **Source used:** the authors' copy, read in full: <https://www.math.ucdavis.edu/~jteran/papers/KGPSJT16.pdf>. (The UCLA copy and the supplementary technical document linked in the concept sheet now return "not found".)

## Summary

Sand as an elastoplastic continuum on MPM. A Hencky-strain elastic model lets the material store stress; Drucker–Prager plasticity caps the shear stress at a friction cone and returns stress to the cone when it is exceeded. A non-associative return keeps volume constant, avoiding the volume gain of standard plasticity. Results include piles with correct angles of repose, sand castles and collapsing columns.

## Key equations

**MPM governing equations** (mass and momentum, deformation gradient $F$):

$$\frac{D\rho}{Dt} + \rho\nabla\cdot v = 0, \qquad \rho\frac{Dv}{Dt} = \nabla\cdot\sigma + \rho g, \qquad \frac{DF}{Dt} = (\nabla v)\,F$$

Transfers use APIC with cubic B-splines.

**Hencky (logarithmic-strain) elasticity.** With $F^E = U\Sigma V^{T}$ (singular value decomposition):

$$\psi(F) = \mu\,\operatorname{tr}\!\big((\ln\Sigma)^2\big) + \frac{\lambda}{2}\big(\operatorname{tr}\ln\Sigma\big)^2$$

$$P(F) = U\big(2\mu\,\Sigma^{-1}\ln\Sigma + \lambda\operatorname{tr}(\ln\Sigma)\,\Sigma^{-1}\big)V^{T}$$

**Drucker–Prager return mapping.** Let $\epsilon = \ln\Sigma_p$ (the log-strain), $d$ the dimension, $\hat\epsilon = \epsilon - \frac{\operatorname{tr}(\epsilon)}{d} I$ its deviatoric part, and

$$\delta\gamma_p = \lVert\hat\epsilon\rVert_F + \frac{d\lambda + 2\mu}{2\mu}\operatorname{tr}(\epsilon)\,\alpha_p$$

Three cases:

1. **Inside the cone** ($\delta\gamma_p \le 0$): static friction; keep $F^E$.
2. **Expanding** ($\lVert\hat\epsilon\rVert = 0$ or $\operatorname{tr}\epsilon > 0$): no resistance; project to the cone tip, $F^E \leftarrow U V^{T}$.
3. **Otherwise**: dynamic friction; project to the cone's side,

$$H_p = \epsilon - \delta\gamma_p \frac{\hat\epsilon}{\lVert\hat\epsilon\rVert_F}, \qquad F^E \leftarrow U e^{H_p} V^{T}$$

Volume is preserved because $\operatorname{tr}(H_p) = \operatorname{tr}(\epsilon)$, so $\det F^E$ is unchanged.

**Cone width from the friction angle** $\phi_F$:

$$\alpha_p = \sqrt{\tfrac{2}{3}}\;\frac{2\sin\phi_F}{3 - \sin\phi_F}$$

**Hardening** (after Mast et al. 2014). Plastic correction $\delta q_p$ is 0 in case 1, $\lVert\epsilon\rVert_F$ in case 2 and $\delta\gamma_p$ in case 3:

$$q_p^{n+1} = q_p^n + \delta q_p, \qquad \phi_F = h_0 + (h_1 q_p^{n+1} - h_3)\,e^{-h_2 q_p^{n+1}}$$

Sand that has flown becomes more frictional, which settles piles.

## Takeaways for the grain solver

- **Friction angle to friction coefficient:** the internal friction coefficient is $\tan\phi_F$. The grain solver's static friction setting should be exposed as an angle of repose in the UI and converted with $\mu_s = \tan\phi$.
- **Three regimes match the PBD contact rule exactly:** stick (inside the cone), separate (tension, no resistance), slide (project to the cone). The grain solver's per-contact friction is the discrete version of this.
- **Volume-preserving projection** is the reason MPM sand does not puff up. A PBD liquid or semisolid needs an equivalent, which Position Based Fluids provides through its density constraint.

## Cautions

- MPM with SVD per particle per step is heavier than PBD; it is the reference for quality, not the version 1 target.
- The supplementary technical document with pseudocode is no longer at its old UCLA address.
