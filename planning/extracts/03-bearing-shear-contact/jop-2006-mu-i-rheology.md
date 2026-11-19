# A Constitutive Law for Dense Granular Flows (the μ(I) rheology)

- **Authors:** Pierre Jop, Yoël Forterre, Olivier Pouliquen
- **Published:** Nature 441, 2006, pp. 727–730 · DOI [10.1038/nature04801](https://doi.org/10.1038/nature04801)
- **Source used:** the arXiv preprint, read in full: <https://arxiv.org/abs/cond-mat/0612110>

## Summary

Dense flowing sand behaves like a liquid whose friction depends on how fast it is sheared. The paper writes that as a 3D constitutive law, now called the μ(I) rheology, and shows it reproduces measured surface flows on a pile between rough sidewalls with no fitted parameters. It has become the standard continuum model for dense granular flow.

## Key equations

**Inertial number** (shear rate $\dot\gamma$, grain diameter $d$, pressure $P$, grain density $\rho_s$):

$$I = \frac{\dot\gamma\, d}{\sqrt{P/\rho_s}}$$

$I$ compares how fast the material is sheared with how fast a grain can rearrange under pressure.

**Shear stress proportional to pressure:**

$$\tau = \mu(I)\,P$$

**Friction rises with shear rate**, from $\mu_s$ at rest to a limit $\mu_2$:

$$\mu(I) = \mu_s + \frac{\mu_2 - \mu_s}{I_0/I + 1}$$

**3D form: an incompressible fluid with an effective viscosity.**

$$\sigma_{ij} = -P\,\delta_{ij} + \tau_{ij}, \qquad \tau_{ij} = \eta(\lvert\dot\gamma\rvert, P)\,\dot\gamma_{ij}, \qquad \eta = \frac{\mu(I)\,P}{\lvert\dot\gamma\rvert}$$

with $\dot\gamma_{ij} = \partial_j u_i + \partial_i u_j$ and $\lvert\dot\gamma\rvert = \sqrt{\tfrac12 \dot\gamma_{ij}\dot\gamma_{ij}}$.

**Yield criterion.** The viscosity diverges as the shear rate goes to zero, so the material only flows if

$$\tau > \mu_s P, \qquad \tau = \sqrt{\tfrac12 \tau_{ij}\tau_{ij}}$$

**Parameters used for glass beads:** $\mu_s = \tan 20.9^\circ$, $\mu_2 = \tan 32.76^\circ$, $I_0 = 0.279$.

## Takeaways for the grain solver

- **Friction should rise with speed.** A pile that is barely moving behaves with $\mu_s$; fast avalanches see up to $\mu_2$. The grain solver can approximate this with a kinetic friction that depends on relative sliding speed, using the formula above per contact with $I$ computed from local speed and overlap.
- **Good default values.** $\mu_s = \tan 20.9^\circ \approx 0.38$ and $\mu_2 = \tan 32.76^\circ \approx 0.64$ are measured values for glass beads, useful as defaults and as test targets.
- **Test target:** the settling test in the concept sheet can check the pile's slope against $\arctan\mu_s$.

## Cautions

- The law assumes constant volume fraction (dense flow). It does not describe dilute, bouncing flows or the start-and-stop hysteresis of real piles.
