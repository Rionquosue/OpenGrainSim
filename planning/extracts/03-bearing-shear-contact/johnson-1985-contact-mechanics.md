# Contact Mechanics

- **Author:** K. L. Johnson
- **Published:** Cambridge University Press, 1985 (this copy: 1987 paperback reprint) · DOI [10.1017/CBO9781139171731](https://doi.org/10.1017/CBO9781139171731)
- **Source used:** read from a scanned copy supplied by the repository owner (not stored in this repository). Sections read: Appendix 3 (summary of Hertz formulae), §5.5 adhesion, §7.2–7.4 tangential loading, §11.4–11.5 elastic and inelastic impact. Equations are transcribed from the page images; book page numbers are given so they can be checked. This replaces an earlier version written partly from memory; the recalled Mindlin stiffness turned out correct.

## Summary

The standard engineering reference on contact between solids. For a grain solver the useful parts are: Hertz theory (how hard two spheres push back as they overlap), Cattaneo–Mindlin theory (how they resist sliding before and during slip), the JKR adhesion model (how sticky contacts behave), and impact theory (how long a collision lasts and how bouncy it is).

## Hertz contact of two spheres (Appendix 3, p. 427)

Effective modulus and radius:

$$E^* = \left(\frac{1-\nu_1^2}{E_1} + \frac{1-\nu_2^2}{E_2}\right)^{-1}, \qquad R = \left(\frac{1}{R_1} + \frac{1}{R_2}\right)^{-1}$$

For normal load $P$:

| Quantity | Formula |
| --- | --- |
| Contact radius | $a = \left(\dfrac{3PR}{4E^*}\right)^{1/3}$ |
| Peak pressure | $p_0 = \dfrac{3P}{2\pi a^2} = \left(\dfrac{6PE^{*2}}{\pi^3 R^2}\right)^{1/3}$ |
| Approach (overlap) | $\delta = \dfrac{a^2}{R} = \left(\dfrac{9P^2}{16 R E^{*2}}\right)^{1/3}$ |
| Max. shear stress | $\tau_1 = 0.31\,p_0$ at depth $0.48a$ on the axis |
| Max. tensile stress | $\sigma_r = \tfrac13(1-2\nu)\,p_0$ at the edge of contact |

Inverting the approach gives the force law used in DEM (also eq. 11.20):

$$P = \tfrac43 R^{1/2} E^* \delta^{3/2}$$

Line contact (load $P$ per unit length): $a = (4PR/\pi E^*)^{1/2}$, $p_0 = 2P/\pi a$. Elliptical contacts follow with correction functions that can be taken as 1 to a first approximation.

## Tangential loading (§7.2–7.4, pp. 210–225)

**Stick and slip conditions** (Amontons' law, constant $\mu$; tangential traction $q$, pressure $p$, slip $s$):

$$\text{stick: } \lvert q\rvert \le \mu\lvert p\rvert, \qquad \text{slip: } \lvert q\rvert = \mu\lvert p\rvert, \quad \frac{q}{\lvert q\rvert} = -\frac{s}{\lvert s\rvert}$$

**Spheres, no slip** (eq. 7.32). A tangential force $Q_x$ shifts the two bodies relative to each other by

$$\delta_x = \frac{Q_x}{8a}\left(\frac{2-\nu_1}{G_1} + \frac{2-\nu_2}{G_2}\right)$$

so the **tangential stiffness** is

$$k_t = \frac{dQ_x}{d\delta_x} = 8\,G^* a, \qquad \frac{1}{G^*} = \frac{2-\nu_1}{G_1} + \frac{2-\nu_2}{G_2}$$

Unlike normal contact, this is linear in force for a given contact size. The traction this needs is infinite at the edge, so some micro-slip always occurs there.

**Spheres, partial slip** (Cattaneo 1938, Mindlin 1949; eqs. 7.39, 7.42). Slip starts at the edge and an annulus spreads inward; the central stick circle has radius

$$\frac{c}{a} = \left(1 - \frac{Q}{\mu P}\right)^{1/3}$$

and the tangential displacement becomes non-linear:

$$\delta_x = \frac{3\mu P}{16a}\left(\frac{2-\nu_1}{G_1} + \frac{2-\nu_2}{G_2}\right)\left\{1 - \left(1 - \frac{Q_x}{\mu P}\right)^{2/3}\right\}$$

At $Q = \mu P$ the stick region shrinks to a point and the contact slides. Johnson's experiments (1955) confirm this curve.

**Tangential versus normal compliance** (eqs. 7.43–7.44), same material:

$$\frac{d\delta_z}{dP} = \frac{1-\nu}{2Ga}, \qquad \frac{d\delta_x}{dQ_x} = \frac{2-\nu}{4Ga}$$

Their ratio, $(2-\nu)/2(1-\nu)$, runs from 1.17 to 1.5 for $\nu$ from 0.25 to 0.5, independent of load. Equivalently, tangential stiffness is $2(1-\nu)/(2-\nu)$ = 0.67–0.86 of normal stiffness.

**History matters** (§7.3–7.4). Because slip is irreversible, the contact state depends on the whole loading sequence, not just the current forces (Mindlin and Deresiewicz 1953). Unloading the tangential force does not retrace loading: reversed slip starts at the edge, so an oscillating force traces a hysteresis loop and dissipates energy. For an oblique force at angle $\alpha$ to the normal on an initially unloaded contact, there is no slip anywhere if $\tan\alpha \le \mu$, and immediate full sliding if $\tan\alpha > \mu$.

## Adhesion: the JKR model (§5.5, pp. 125–129)

With surface energy $\gamma$ per unit area of each surface (work of adhesion $2\gamma$), contact radius and load are related by

$$\left(P - \frac{4E^* a^3}{3R}\right)^2 = 16\pi\gamma E^* a^3$$

**Pull-off force** (load-controlled separation):

$$P_c = 3\pi\gamma R, \qquad a_c = \left(\frac{9\gamma R^2}{4E^*}\right)^{1/3}$$

Under displacement control the contact stays stable down to $P = -5P_c/9$. At zero load, adhesive contacts keep a finite contact radius, larger than Hertz predicts under any load. Real surfaces rarely show this because roughness breaks adhesive junctions; the exceptions are very smooth or very soft solids (gelatine, rubber), which the book's experiments use.

## Impact (§11.4–11.5, pp. 351–364)

**Elastic (Hertz) impact.** With $1/m = 1/m_1 + 1/m_2$ and approach speed $V_z$, the maximum overlap and the contact duration are

$$\delta_z^* = \left(\frac{15 m V_z^2}{16 R^{1/2} E^*}\right)^{2/5}, \qquad T_c = 2.94\,\frac{\delta_z^*}{V_z} = 2.87\left(\frac{m^2}{R E^{*2} V_z}\right)^{1/5}$$

The force pulse is close to a half sine: $P(t) \approx P^* \sin(\pi t / 2t^*)$.

**Onset of yield** (eq. 11.39): a sphere hitting a large flat body yields when $\rho V_Y^2 / Y = 26\,(Y/E^*)^4$; for hard steel on medium-hard steel, $V_Y \approx 0.14$ m/s.

**Coefficient of restitution with plastic indentation** (eq. 11.45; dynamic yield strength $Y_d$):

$$e \approx 3.8\left(\frac{Y_d}{E^*}\right)^{1/2}\left(\frac{\tfrac12 m V^2}{Y_d R^3}\right)^{-1/8}$$

Restitution is **not a material constant**: it is close to 1 at low speeds and falls with impact speed (roughly as $V^{-1/4}$ once indentation is fully plastic), as measurements on steel, brass, bronze and lead confirm.

## Takeaways for the grain solver

- **Stiffness from material properties.** If grains ever become compliant (XPBD or DEM), $k_n = \tfrac43 E^* \sqrt{R\delta}$ and $k_t = 8G^*a$ give physically meaningful stiffness from Young's modulus and Poisson's ratio. TinyDEM uses exactly these.
- **Tangential stiffness ratio 2/3 to 0.86.** This supports reading Cundall & Strack's garbled range as $k_s/k_n$ = 2/3 to 1, and gives a default of about 0.8.
- **Friction has memory and hysteresis.** Contacts that are loaded and unloaded sideways dissipate energy even without full sliding. A per-contact stored tangential offset (as in TinyDEM and Cundall & Strack) is the minimum needed to show this; plain PBD friction cannot.
- **Collision time sets the time step for compliant contacts.** $T_c$ for sand-sized grains is microseconds; this is why the concept sheet uses rigid PBD contacts rather than resolving each collision.
- **Restitution should fall with impact speed.** A constant bounciness is a simplification; for visual realism, faster impacts can be made less bouncy.
- **Cohesion for wet sand and semisolids.** JKR gives a pull-off force $3\pi\gamma R$ per contact. A simple cohesive grain model is a capped attractive force of that size between touching grains, which lets damp sand hold vertical walls.

## Cautions

- Hertz and Mindlin assume smooth, elastic, small-strain contacts between non-conforming bodies; real grains are rough and angular.
- Only the sections listed above were read; the book's chapters on rolling, plasticity, lubrication and rough surfaces are not covered here.
