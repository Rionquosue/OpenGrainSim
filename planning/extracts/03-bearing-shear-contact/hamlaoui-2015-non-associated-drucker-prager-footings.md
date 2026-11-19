# Finite Element Analysis of the Plastic Limit Load and the Collapse Mechanism of Strip Foundations with Non-Associated Drucker–Prager Model

- **Authors:** M. Hamlaoui, A. Oueslati, B. Lamri, G. de Saxcé (Université de Lille 1; Université Hassiba Benbouali de Chlef)
- **Published:** European Journal of Environmental and Civil Engineering, 2015 (online 20 February 2015) · DOI [10.1080/19648189.2015.1005162](https://doi.org/10.1080/19648189.2015.1005162)
- **Source used:** read in full from a copy obtained by the repository owner. The PDF is not stored in this repository.
- **Replaces:** the Drucker & Prager (1952) original in the reading list, which could not be obtained. This paper states the Drucker–Prager model precisely, cites the 1952 paper as its origin, and adds the non-associated flow rule and validated bearing-capacity results.

## Summary

The paper studies how much load a strip footing can carry on soil that follows the Drucker–Prager model with a **non-associated** flow rule: the soil dilates (expands) less than its friction angle would imply, which matches experiments on real soils. Using incremental finite elements (Cast3M), it validates against closed-form solutions, computes limit loads for rough and smooth footings, and maps the collapse mechanism. Its main finding: under a rough footing, the angle of the rigid wedge that forms is independent of the dilatancy angle.

## Key equations

**Yield function** (cohesion $c$, deviatoric stress $s$, $r$ a constant set by the friction angle $\varphi$):

$$f(\sigma) = \frac{1}{r}\,\sigma_{eq} + \alpha\,\operatorname{tr}(\sigma) - c \le 0, \qquad \sigma_{eq} = \sqrt{\tfrac32\, s : s}$$

$$\tan\varphi = 3\alpha, \qquad r = \frac{3\sqrt3}{\sqrt{9 + 12\tan^2\varphi}}$$

This choice of $r$ makes the cone match Mohr–Coulomb **in plane strain**. With $\alpha = 0$ it reduces to von Mises. Equivalently, in invariant form ($I_1 = \operatorname{tr}\sigma$, $J_2 = \tfrac12 s : s$):

$$\sqrt{J_2} + \frac{\tan\varphi}{\sqrt{9 + 12\tan^2\varphi}}\, I_1 = \frac{3c}{\sqrt{9 + 12\tan^2\varphi}}$$

**Plastic potential and flow rule.** Plastic strain flows normal to a second cone set by the dilatancy angle $\psi$, not by $\varphi$:

$$g(\sigma) = \frac{1}{r}\,\sigma_{eq} + \beta\,\operatorname{tr}(\sigma), \qquad \tan\psi = 3\beta, \qquad 0 \le \psi \le \varphi$$

$$\dot\varepsilon^{p} = \lambda\,\frac{\partial g}{\partial \sigma}, \qquad \lambda \ge 0, \qquad \lambda\, f(\sigma) = 0$$

$\psi = \varphi$ is the associated (normality) case; $\psi = 0$ means no volume change during plastic flow. (The paper prints the complementarity condition as $\lambda\, g(\sigma) = 0$; the standard condition, used here, is $\lambda f = 0$.)

**Prandtl–Hill bearing capacity** of a strip footing on weightless soil (associated case; $b$ footing width), scaled by cohesion:

$$\frac{P_u}{c\, b} = \cot\varphi\left( e^{\pi\tan\varphi} \tan^2\!\left(\frac{\pi}{4} + \frac{\varphi}{2}\right) - 1 \right)$$

**Drescher–Detournay correction for non-associated flow.** Replace $\varphi$ and $c$ in the formula above with reduced values:

$$\tan\varphi^* = \frac{\cos\varphi\cos\psi}{1 - \sin\varphi\sin\psi}\,\tan\varphi, \qquad c^* = \frac{\cos\varphi\cos\psi}{1 - \sin\varphi\sin\psi}\,c$$

On the velocity characteristics the soil then obeys Mohr–Coulomb with these values: $\lvert\tau\rvert + \sigma_n \tan\varphi^* = c^*$.

**Collapse mechanism (Prandtl).** A rigid wedge under the footing moves down with base angle

$$\delta = \frac{\pi}{4} + \frac{\varphi}{2}$$

then a log-spiral shear zone, then a rigid passive wedge pushed up and out beside the footing. The paper measures $\delta$ from the simulations and finds it matches $\pi/4 + \varphi/2$ within about 2% **for every dilatancy angle tested** (rough footing, $\varphi = 35°$, $\psi$ from 10° to 35°). Lower dilatancy shrinks the failure zone and lowers the limit load, but leaves the wedge angle unchanged.

## Results worth keeping as test targets

**Dimensionless bearing capacity, associated case** ($P_u / cb$):

| $\varphi = \psi$ | Finite elements | Prandtl–Hill exact |
| --- | --- | --- |
| 35° | 46.52 | 46.12 |
| 30° | 30.71 | 30.14 |
| 25° | 21.05 | 20.72 |
| 20° | 15.01 | 14.83 |
| 15° | 11.15 | 10.98 |
| 10° | 8.47 | 8.34 |
| 5° | 6.61 | 6.49 |

**Effect of dilatancy, rough footing, $\varphi = 35°$:**

| $\psi$ | Finite elements | Drescher–Detournay |
| --- | --- | --- |
| 35° | 46.41 | 46.12 |
| 30° | 46.15 | 45.28 |
| 25° | 43.74 | 43.03 |
| 15° | 40.15 | 36.05 |
| 0° | 25.38 | 24.37 |

- Rough and smooth footings give almost the same bearing capacity.
- With low dilatancy ($\psi$ near 0) the load–settlement curve oscillates; the limit load is then taken as the average in the plastic regime. This is a known hard case for numerical methods.

**Simulation setup used:** strip half-width $B$ = 1 m, depth $H$ = 6.5 B, half-length $L$ = 16 B; $E$ = 100 MPa, $\nu$ = 0.2, $c$ = 10 kPa; 10,732 six-node triangles, refined near the footing edge; displacement-controlled loading.

## Takeaways for the grain solver

- **Two calibrations of the same cone.** This paper's $r$ matches Mohr–Coulomb in plane strain; the ANSYS form (outer cone, $\beta = 2\sin\varphi / (\sqrt3(3 - \sin\varphi))$) matches it at the outer corners in 3D. They give different cones for the same friction angle. Pick one, document it, and test against the matching reference.
- **Dilatancy is a second, independent setting.** Real sand dilates less than its friction angle implies. Exposing friction angle and dilatancy angle separately (with dilatancy defaulting low) gives more realistic piles and failures than a single friction value. The 2016 MPM sand paper in this folder uses zero dilatancy (volume-preserving flow).
- **A ready-made validation test.** Press a rigid plate into a bed of grains with known friction and measure the peak force per unit width. It should approach the tables above, scaled by cohesion and width. With cohesion near zero, use Terzaghi's self-weight term instead (see the Terzaghi extract).
- **A visual check too.** At failure, the grains under a rough plate should form a wedge at about $45° + \varphi/2$ to the horizontal, whatever the dilatancy. That is easy to measure from particle displacements.

## Cautions

- Plane-strain, weightless soil, small deformations: these are continuum engineering results. A particle simulation will only approach them with many grains across the footing.
- Several equations in the text extraction were scrambled; the forms above were rebuilt and checked against the paper's own reduction to von Mises and against its tabulated Prandtl–Hill values (e.g. 46.12 at 35°). The closed-form limit load for the paper's 2-D block validation test (its Eq. 4) could not be reliably recovered and is left out.
