# Formula table

Every distinct formula from the reference extracts in [extracts/](extracts/README.md), one block each, with no repeats: where several sources give the same formula, it appears once and cites all of them. Source tags such as [S14] refer to the list at the end.

Formulas from sources marked ⚠️ in the source list were written from knowledge rather than transcribed, and blocks marked ⚠️ have a known transcription ambiguity. Check those against the original before implementing; check every formula against its source before it goes into code.

Duplicate and derived formulas have been removed, but alternative methods the solver may offer as options are kept side by side (marked *Alternative option* where a block is an alternative to the one before it, plus the PIC, FLIP and APIC transfer schemes); see [formulatable-removed.md](formulatable-removed.md) for what was removed, what was restored, and why.

## 1. Index


**A. Grain contact (DEM)**

- [F01. Effective contact properties](#f01-effective-contact-properties)
- [F02. Contact overlap and normal](#f02-contact-overlap-and-normal)
- [F03. Relative velocity at the contact point](#f03-relative-velocity-at-the-contact-point)
- [F04. Hertz normal force](#f04-hertz-normal-force)
- [F05. Hertz contact radius, pressure and approach](#f05-hertz-contact-radius-pressure-and-approach)
- [F06. Mindlin tangential stiffness](#f06-mindlin-tangential-stiffness)
- [F07. Cattaneo-Mindlin partial slip](#f07-cattaneo-mindlin-partial-slip)
- [F08. Spring-dashpot contact force](#f08-spring-dashpot-contact-force)
- [F09. Static friction spring with Coulomb limit](#f09-static-friction-spring-with-coulomb-limit)
- [F10. Wall friction velocity clamp](#f10-wall-friction-velocity-clamp)
- [F11. Rolling and twisting friction](#f11-rolling-and-twisting-friction)
- [F12. Damping from coefficient of restitution](#f12-damping-from-coefficient-of-restitution)
- [F13. Stiffness-proportional contact damping](#f13-stiffness-proportional-contact-damping)
- [F14. Global mass-proportional damping](#f14-global-mass-proportional-damping)
- [F15. Explicit time integration](#f15-explicit-time-integration)
- [F16. Critical time step of a contact spring](#f16-critical-time-step-of-a-contact-spring)
- [F17. Time step from elastic wave speed](#f17-time-step-from-elastic-wave-speed)
- [F18. JKR adhesion](#f18-jkr-adhesion)
- [F19. Hertz impact overlap and duration](#f19-hertz-impact-overlap-and-duration)
- [F20. Impact speed for onset of yield](#f20-impact-speed-for-onset-of-yield)
- [F21. Restitution with plastic indentation](#f21-restitution-with-plastic-indentation)

**B. Granular flow and soil failure**

- [F22. Friction angle to friction coefficient](#f22-friction-angle-to-friction-coefficient)
- [F23. Mohr-Coulomb yield in von Mises form](#f23-mohr-coulomb-yield-in-von-mises-form)
- [F24. Frictional stress of flowing sand](#f24-frictional-stress-of-flowing-sand)
- [F25. Rigid-cell stress test](#f25-rigid-cell-stress-test)
- [F26. Inertial number and mu(I) friction](#f26-inertial-number-and-mui-friction)
- [F27. mu(I) effective viscosity and yield](#f27-mui-effective-viscosity-and-yield)
- [F28. Drucker-Prager yield, plane-strain calibration](#f28-drucker-prager-yield-plane-strain-calibration)
- [F29. Non-associated flow rule](#f29-non-associated-flow-rule)
- [F30. Drucker-Prager cone width from friction angle](#f30-drucker-prager-cone-width-from-friction-angle)
- [F31. Drucker-Prager return mapping](#f31-drucker-prager-return-mapping)
- [F32. Friction hardening](#f32-friction-hardening)
- [F33. Hencky elasticity](#f33-hencky-elasticity)
- [F34. Prandtl-Hill bearing capacity](#f34-prandtl-hill-bearing-capacity)
- [F35. Drescher-Detournay reduced strength](#f35-drescher-detournay-reduced-strength)
- [F36. Prandtl wedge angle](#f36-prandtl-wedge-angle)
- [F37. Terzaghi bearing capacity](#f37-terzaghi-bearing-capacity)
- [F38. Bearing capacity factors](#f38-bearing-capacity-factors)

**C. Tearing and fracture**

- [F39. Tension-only elastic degradation](#f39-tension-only-elastic-degradation)
- [F40. NACC yield surface](#f40-nacc-yield-surface)
- [F41. Damage evolution with crack irreversibility](#f41-damage-evolution-with-crack-irreversibility)
- [F42. Fibre-aware crack driving force](#f42-fibre-aware-crack-driving-force)
- [F43. Explicit damage update](#f43-explicit-damage-update)
- [F44. Implicit phase-field damage update](#f44-implicit-phase-field-damage-update)
- [F45. Local damage rule](#f45-local-damage-rule)
- [F46. Edge stress and bond breaking](#f46-edge-stress-and-bond-breaking)
- [F47. Graph FEM plastic flow amount](#f47-graph-fem-plastic-flow-amount)
- [F48. Strain split and deviatoric yield](#f48-strain-split-and-deviatoric-yield)
- [F49. Plastic flow with plastic limit](#f49-plastic-flow-with-plastic-limit)
- [F50. Green strain and membrane stress](#f50-green-strain-and-membrane-stress)
- [F51. Discrete bending strain and sheet energy](#f51-discrete-bending-strain-and-sheet-energy)
- [F52. Separation strength](#f52-separation-strength)
- [F53. Bending fracture factor](#f53-bending-fracture-factor)
- [F54. Stretching plasticity rate and thinning](#f54-stretching-plasticity-rate-and-thinning)
- [F55. Bending plasticity update](#f55-bending-plasticity-update)
- [F56. Fracture remeshing size](#f56-fracture-remeshing-size)

**D. Liquids and continuum transfer**

- [F57. Navier-Stokes momentum with variable viscosity](#f57-navier-stokes-momentum-with-variable-viscosity)
- [F58. Pressure projection](#f58-pressure-projection)
- [F59. CFL time-step limit](#f59-cfl-time-step-limit)
- [F60. Ghost-fluid free surface](#f60-ghost-fluid-free-surface)
- [F61. Surface tension pressure jump](#f61-surface-tension-pressure-jump)
- [F62. MPM governing equations](#f62-mpm-governing-equations)
- [F63. PIC and FLIP particle to grid](#f63-pic-and-flip-particle-to-grid)
- [F64. PIC and FLIP grid to particle](#f64-pic-and-flip-grid-to-particle)
- [F65. PIC fraction from viscosity](#f65-pic-fraction-from-viscosity)
- [F66. APIC particle to grid](#f66-apic-particle-to-grid)
- [F67. APIC grid to particle](#f67-apic-grid-to-particle)
- [F68. SPH field interpolation](#f68-sph-field-interpolation)
- [F69. SPH pressure force for mixed sizes](#f69-sph-pressure-force-for-mixed-sizes)
- [F70. SPH artificial viscosity](#f70-sph-artificial-viscosity)
- [F71. Mixed-size neighbour criterion](#f71-mixed-size-neighbour-criterion)
- [F72. Adaptive character field](#f72-adaptive-character-field)
- [F73. Split and merge rule](#f73-split-and-merge-rule)
- [F74. Collider pre-subdivision criterion](#f74-collider-pre-subdivision-criterion)

**E. Neighbour search**

- [F75. Uniform grid cell index](#f75-uniform-grid-cell-index)
- [F76. Spatial hash](#f76-spatial-hash)
- [F77. Morton Z-order key](#f77-morton-z-order-key)
- [F78. Counting sort offsets](#f78-counting-sort-offsets)
- [F79. Neighbour index delta encoding](#f79-neighbour-index-delta-encoding)

**F. Floating-point precision**

- [F80. Rounding error model and machine epsilon](#f80-rounding-error-model-and-machine-epsilon)
- [F81. Kahan compensated summation](#f81-kahan-compensated-summation)
- [F82. Fused multiply-add](#f82-fused-multiply-add)
- [F83. Floating-point step size](#f83-floating-point-step-size)
- [F84. Reproducible summation by pre-rounding](#f84-reproducible-summation-by-pre-rounding)

## 2. Body

### A. Grain contact (DEM)

#### F01. Effective contact properties

---

**Purpose:** Combine two touching bodies' sizes, masses and elastic constants into single effective values.

**Formula:**

$$\frac{1}{R^*} = \frac{1}{R_1} + \frac{1}{R_2}, \qquad \frac{1}{E^*} = \frac{1-\nu_1^2}{E_1} + \frac{1-\nu_2^2}{E_2}, \qquad \frac{1}{G^*} = \frac{2-\nu_1}{G_1} + \frac{2-\nu_2}{G_2}, \qquad G_i = \frac{E_i}{2(1+\nu_i)}, \qquad \frac{1}{m^*} = \frac{1}{m_1} + \frac{1}{m_2}$$

**Summary:** Inputs to every Hertz–Mindlin formula below. A wall or static collider is the limit $R_2, E_2, m_2 \to \infty$. [S14], [S15]

---

#### F02. Contact overlap and normal

---

**Purpose:** Detect a contact between two spheres and measure how far they overlap.

**Formula:**

$$d = x_i - x_j, \qquad \hat n = \frac{d}{\lVert d\rVert}, \qquad \delta = R_i + R_j - \lVert d\rVert, \qquad \text{contact if } \delta > 0$$

**Summary:** The basic grain–grain test. $\delta$ is the overlap that stands in for real grain deformation; $\hat n$ is the push direction. [S13], [S14]

---

#### F03. Relative velocity at the contact point

---

**Purpose:** Get the sliding velocity at the contact, including the effect of grain spin.

**Formula:**

$$v = v_i - v_j + \hat n \times (r_i\,\omega_i - r_j\,\omega_j), \qquad v_n = (v\cdot\hat n)\,\hat n, \qquad v_s = v - v_n, \qquad r_i = R_i - \delta/2$$

**Summary:** Splits relative motion into normal (closing) and shear (sliding) parts; the shear part drives friction and torque. Cundall & Strack give the 2D form with angular velocities $\dot\theta$. [S13], [S14]

---

#### F04. Hertz normal force

---

**Purpose:** Force between two elastic spheres as a function of overlap.

**Formula:**

$$P = \tfrac43\, E^* R^{*1/2}\, \delta^{3/2}, \qquad k_n = \frac{dP}{d\delta} = \tfrac43 E^* a$$

**Summary:** Non-linear contact law: contacts stiffen as they are pressed; $a$ is the contact radius from F05. Use for compliant grains (DEM or XPBD); rigid PBD grains replace it with a hard constraint. [S14], [S15]

---

#### F05. Hertz contact radius, pressure and approach

---

**Purpose:** Size of the contact patch and peak pressure for a given normal load.

**Formula:**

$$a = \left(\frac{3PR^*}{4E^*}\right)^{1/3}, \qquad p_0 = \frac{3P}{2\pi a^2}, \qquad \delta = \frac{a^2}{R^*} = \left(\frac{9P^2}{16 R^* E^{*2}}\right)^{1/3}, \qquad p(r) = p_0\left(1 - \frac{r^2}{a^2}\right)^{1/2}$$

**Summary:** Contact patch geometry under load; the largest shear stress is $0.31\,p_0$ at depth $0.48a$ and the largest tension $\tfrac13(1-2\nu)p_0$ at the patch edge. Useful for crushing or damage thresholds. [S15]

---

#### F06. Mindlin tangential stiffness

---

**Purpose:** Resistance of a contact to small sideways displacement before it slips.

**Formula:**

$$\delta_x = \frac{Q_x}{8\,a\,G^*} \quad\Rightarrow\quad k_t = 8\,G^* a$$

**Summary:** Linear in force for a given contact radius. Gives the shear spring stiffness for DEM friction from material constants. [S14], [S15]

---

#### F07. Cattaneo-Mindlin partial slip

---

**Purpose:** How the stuck region shrinks and displacement grows as sideways force approaches the friction limit.

**Formula:**

$$\frac{c}{a} = \left(1 - \frac{Q}{\mu P}\right)^{1/3}, \qquad \delta_x = \frac{3\mu P}{16\,a\,G^*}\left\{1 - \left(1 - \frac{Q}{\mu P}\right)^{2/3}\right\}$$

**Summary:** *Alternative option:* Exact for one monotonic loading path only; general load histories need the Mindlin–Deresiewicz rules, which are not in the sources. The static friction spring handles any history at constant cost. Micro-slip starts at the contact edge immediately; full sliding at $Q = \mu P$. Explains friction hysteresis and energy loss under back-and-forth loading. [S15]

---

#### F08. Spring-dashpot contact force

---

**Purpose:** Normal and shear contact forces with damping.

**Formula:**

$$f_n = -k_n u_n - c_n v_n, \qquad f_s = -k_s u_s - c_s v_s, \qquad u_n = -\delta\,\hat n$$

**Summary:** The standard DEM contact law; $u_s$ is the stored shear spring stretch (F09). Forces are equal and opposite on the two grains. [S14], [S19]

---

#### F09. Static friction spring with Coulomb limit
---

**Purpose:** Per-contact friction with memory: stick below the Coulomb limit, slide at it, and survive grain rotation.

**Formula:**

1. Project: $u_m \leftarrow \lVert u_m\rVert\,\dfrac{u_m - (u_m\cdot\hat n)\hat n}{\lVert u_m - (u_m\cdot\hat n)\hat n\rVert}$, for $m = s, r$
2. Stick, if $\lVert f_m\rVert \le \mu_m\lVert f_n\rVert + c_0$: $\;u_m \leftarrow u_m + \Delta t\, v_m$
3. Slip, otherwise: $\;f_m \leftarrow \big(\mu_m \lVert f_n\rVert + c_0\big)\dfrac{f_m}{\lVert f_m\rVert}, \quad u_m \leftarrow -\dfrac{1}{k_m}\big(f_m + c_m v_m\big)$

**Summary:** The friction rule with memory for grain–grain and grain–collider contacts, for sliding ($s$), rolling ($r$) and twisting ($t$); the fix if PBD piles creep. $c_0$ is optional cohesion. Between different materials use the smaller friction and cohesion; give colliders their own $\mu$ (F22). [S13], [S14], [S15]

---

#### F10. Wall friction velocity clamp

---

**Purpose:** Apply Coulomb friction at a collider at the velocity level.

**Formula:**

$$u_T \leftarrow \max\!\left(0,\; 1 - \frac{\mu\,\lvert u\cdot n\rvert}{\lvert u_T\rvert}\right) u_T$$

**Summary:** *Alternative option:* Memoryless and velocity-level, so it suits grid liquids and velocity-based solvers; the static friction spring keeps sticking history. After removing the velocity into the wall, shrink the sliding velocity by the friction it can supply; clamping to zero gives static friction. Directly usable for SDF colliders. [S6]

---

#### F11. Rolling and twisting friction

---

**Purpose:** Resist grains rolling and spinning against each other.

**Formula:**

$$f_r = -k_r u_r - c_r v_r, \quad \lVert f_r\rVert \le \mu_r\lVert f_n\rVert, \qquad f_t = -k_t u_t - c_t v_t, \quad \lvert f_t\rvert \le \mu_t\lVert f_n\rVert$$

$$v_r = \left(\frac{1}{r_i} + \frac{1}{r_j}\right)^{-1} \omega\times\hat n, \qquad v_t = R^*\,\omega\cdot\hat n, \qquad \omega = \omega_i - \omega_j$$

**Summary:** Stops spherical grains rolling like ball bearings, so piles keep realistic slopes. TinyDEM sets $k_r = k_n$, $k_t = k_s$, $c_r = c_n$, $c_t = c_s$. [S14]

---

#### F12. Damping from coefficient of restitution

---

**Purpose:** Turn a desired bounciness into contact damping coefficients.

**Formula:**

$$\beta = \frac{1}{\sqrt{1 + (\pi/\ln e_n)^2}}, \qquad c_n = \sqrt{5\, m^* k_n}\;\beta, \qquad c_s = \sqrt{\tfrac16\, m^* k_s}\;\beta$$

**Summary:** Lets artists set restitution $e_n$ (0 = dead, 1 = perfectly bouncy) instead of raw damping. The $\tfrac16$ factor is TinyDEM's own heuristic. [S14]

---

#### F13. Stiffness-proportional contact damping

---

**Purpose:** Dashpots at each contact proportional to its stiffness.

**Formula:**

$$D_n = c_n\,\dot n, \qquad D_s = c_s\,\dot s, \qquad c_n = \beta k_n, \qquad c_s = \beta k_s$$

**Summary:** *Alternative option:* The original DEM damping; its factor is set directly rather than from a restitution value. Original DEM contact damping; shear damping is switched off while the contact slides, leaving friction to dissipate energy. [S13]

---

#### F14. Global mass-proportional damping

---

**Purpose:** Damp each grain's absolute motion so assemblies settle, stably.

**Formula:**

$$\dot x_{N+\frac12} = \frac{\dot x_{N-\frac12}\left(1 - \alpha\frac{\Delta t}{2}\right) + \dfrac{\sum (F + D)}{m}\,\Delta t}{1 + \alpha\frac{\Delta t}{2}}$$

(and the same for rotation with moments and inertia)

**Summary:** *Alternative option:* A cheap way to settle quasi-static piles, but it also slows free fall and rigid motion; contact damping acts only at contacts. Dashpots to ground with $C = \alpha m$, integrated with central differences. Changes how fast piles settle, not where they end up. [S13]

---

#### F15. Explicit time integration

---

**Purpose:** Advance grain velocities and positions each step.

**Formula:**

$$a_i = \frac{1}{m_i}\sum_j \big(f_n^{ij} + f_s^{ij}\big) + g - c_i v_i, \qquad v_i \leftarrow v_i + \Delta t\, a_i, \qquad x_i \leftarrow x_i + \Delta t\, v_i$$

**Summary:** Semi-implicit (symplectic) Euler; Cundall & Strack's time-centred form with velocities at half steps is equivalent. Rotation is updated the same way from torques. [S13], [S14]

---

#### F16. Critical time step of a contact spring

---

**Purpose:** Largest stable explicit time step for a contact.

**Formula:**

$$\Delta t_{crit} = 2\sqrt{m/k}$$

**Summary:** *Alternative option:* For constant (linear) contact stiffness; for Hertz contacts use the time step from elastic wave speed. Explicit DEM must run at a fraction of this. With realistic stiffness it is tiny, which is why the grain solver uses PBD substeps instead. [S13]

---

#### F17. Time step from elastic wave speed

---

**Purpose:** Default DEM time step that resolves a collision in about a dozen steps.

**Formula:**

$$\Delta t = R_{min}\sqrt{\frac{\rho\,(1-\nu^2)}{E}}$$

**Summary:** Based on elastic wave travel across the smallest grain; gives microseconds for stiff sand. [S14]

---

#### F18. JKR adhesion

---

**Purpose:** Contact size and pull-off force for sticky elastic spheres.

**Formula:**

$$\left(P - \frac{4E^* a^3}{3R^*}\right)^2 = 16\pi\gamma E^* a^3, \qquad P_c = 3\pi\gamma R^*, \qquad a_c = \left(\frac{9\gamma R^{*2}}{4E^*}\right)^{1/3}$$

**Summary:** $\gamma$ is surface energy per unit area of each surface. Pull-off force $P_c$ is a physical basis for cohesion in damp sand and sticky semisolids. [S15]

---

#### F19. Hertz impact overlap and duration

---

**Purpose:** Peak overlap and collision time for two elastic spheres.

**Formula:**

$$\delta^* = \left(\frac{15\, m^* V^2}{16\, R^{*1/2} E^*}\right)^{2/5}, \qquad T_c = 2.94\,\frac{\delta^*}{V} = 2.87\left(\frac{m^{*2}}{R^* E^{*2} V}\right)^{1/5}$$

**Summary:** Sets the time resolution a compliant contact model needs; the force pulse is close to a half sine. [S15]

---

#### F20. Impact speed for onset of yield

---

**Purpose:** Speed at which a sphere hitting a large flat body starts to deform plastically.

**Formula:**

$$\frac{\rho\, V_Y^2}{Y} = 26\left(\frac{Y}{E^*}\right)^4$$

**Summary:** $Y$ is the softer body's yield stress. Shows most metal impacts are partly plastic (about 0.14 m/s for hard steel); useful for deciding when impacts should dent or crumble. [S15]

---

#### F21. Restitution with plastic indentation

---

**Purpose:** How bounciness falls with impact speed.

**Formula:**

$$e \approx 3.8\left(\frac{Y_d}{E^*}\right)^{1/2}\left(\frac{\tfrac12 m^* V^2}{Y_d R^{*3}}\right)^{-1/8}$$

**Summary:** Restitution is not a material constant: near 1 at low speed, falling roughly as $V^{-1/4}$ once indentation is fully plastic. Option for speed-dependent bounciness. [S15]

---

### B. Granular flow and soil failure

#### F22. Friction angle to friction coefficient

---

**Purpose:** Convert an angle of repose or friction angle into a friction coefficient.

**Formula:**

$$\mu = \tan\phi$$

**Summary:** Lets the UI expose an angle of repose and the solver use $\mu$; settling tests check the pile slope against $\arctan\mu_s$. [S9], [S16]

---

#### F23. Mohr-Coulomb yield in von Mises form

---

**Purpose:** Decide whether sand at a point holds or flows.

**Formula:**

$$\sqrt3\,\bar\sigma < \sin\phi\;\sigma_m + c, \qquad \sigma_m = \frac{\operatorname{tr}\sigma}{3}, \qquad \bar\sigma = \frac{\lvert\sigma - \sigma_m\delta\rvert_F}{\sqrt2}$$

**Summary:** *Alternative option:* Part of the grid-sand model (Zhu & Bridson 2005), cheaper than the MPM return mapping but with no elastic stress, so tall columns are less accurate. Continuum Coulomb friction: shear stress is resisted in proportion to pressure; $c$ adds cohesion. [S6]

---

#### F24. Frictional stress of flowing sand

---

**Purpose:** Stress in yielding sand that most directly resists the sliding.

**Formula:**

$$\sigma_f = -\sin\phi\; p\;\frac{D}{\sqrt{1/3}\,\lvert D\rvert_F}, \qquad D = \tfrac12(\nabla u + \nabla u^T)$$

**Summary:** *Alternative option:* Part of the grid-sand model; see the Mohr–Coulomb yield block. Used on a grid where the yield test fails; added to the velocity as $\Delta t\,\nabla\cdot\sigma_f/\rho$. [S6]

---

#### F25. Rigid-cell stress test

---

**Purpose:** Stress needed to stop all sliding in one grid cell in one step.

**Formula:**

$$\sigma_{rigid} = -\frac{\rho\, D\, \Delta x^2}{\Delta t}$$

**Summary:** *Alternative option:* Part of the grid-sand model; the MPM return mapping handles static friction as its case 1 instead. If this satisfies the yield condition (F23), the cell is rigid; connected rigid cells move as rigid bodies. [S6]

---

#### F26. Inertial number and mu(I) friction

---

**Purpose:** Friction coefficient of dense flowing grains as a function of shear rate.

**Formula:**

$$I = \frac{\dot\gamma\, d}{\sqrt{P/\rho_s}}, \qquad \tau = \mu(I)\,P, \qquad \mu(I) = \mu_s + \frac{\mu_2 - \mu_s}{I_0/I + 1}$$

**Summary:** Friction rises from $\mu_s$ at rest to $\mu_2$ in fast flow. Glass beads: $\mu_s = \tan 20.9°$, $\mu_2 = \tan 32.76°$, $I_0 = 0.279$. Basis for speed-dependent kinetic friction. [S16]

---

#### F27. mu(I) effective viscosity and yield

---

**Purpose:** 3D form of the mu(I) law as a viscous fluid with a yield stress.

**Formula:**

$$\sigma_{ij} = -P\,\delta_{ij} + \eta\,\dot\gamma_{ij}, \qquad \eta = \frac{\mu(I)\,P}{\lvert\dot\gamma\rvert}, \qquad \text{flows only if } \tau > \mu_s P$$

with $\lvert\dot\gamma\rvert = \sqrt{\tfrac12\dot\gamma_{ij}\dot\gamma_{ij}}$ and $\tau = \sqrt{\tfrac12\tau_{ij}\tau_{ij}}$.

**Summary:** Continuum model for dense granular flow (avalanches, pouring); viscosity diverges at rest, giving a yield criterion. [S16]

---

#### F28. Drucker-Prager yield, plane-strain calibration

---

**Purpose:** Pressure-dependent yield cone matched to Mohr–Coulomb in plane strain.

**Formula:**

$$f(\sigma) = \frac{1}{r}\,\sigma_{eq} + \alpha\operatorname{tr}\sigma - c \le 0, \qquad \sigma_{eq} = \sqrt{\tfrac32\, s:s}, \qquad \tan\phi = 3\alpha, \qquad r = \frac{3\sqrt3}{\sqrt{9 + 12\tan^2\phi}}$$

**Summary:** Smooth replacement for Mohr–Coulomb; reduces to von Mises when $\alpha = 0$. [S17]

---

#### F29. Non-associated flow rule

---

**Purpose:** Let soil dilate less than its friction angle implies.

**Formula:**

$$g(\sigma) = \frac{1}{r}\,\sigma_{eq} + \beta\operatorname{tr}\sigma, \qquad \tan\psi = 3\beta, \qquad \dot\varepsilon^p = \lambda\frac{\partial g}{\partial\sigma}, \qquad \lambda \ge 0, \quad \lambda f = 0$$

**Summary:** Dilatancy angle $\psi \le \phi$ is a second, independent setting; $\psi = 0$ means no volume change during flow. [S17]

---

#### F30. Drucker-Prager cone width from friction angle

---

**Purpose:** Cone parameter used by the MPM sand return mapping.

**Formula:**

$$\alpha = \sqrt{\tfrac23}\;\frac{2\sin\phi_F}{3 - \sin\phi_F}$$

**Summary:** Converts the (possibly hardened) friction angle into the cone width used in F31. [S9]

---

#### F31. Drucker-Prager return mapping

---

**Purpose:** Project an elastic deformation back onto the friction cone each step.

**Formula:**

$$\epsilon = \ln\Sigma, \qquad \hat\epsilon = \epsilon - \frac{\operatorname{tr}\epsilon}{d}I, \qquad \delta\gamma = \lVert\hat\epsilon\rVert_F + \frac{d\lambda + 2\mu}{2\mu}\operatorname{tr}(\epsilon)\,\alpha$$

1. $\delta\gamma \le 0$: inside the cone, keep $F^E = U\Sigma V^T$.
2. $\lVert\hat\epsilon\rVert = 0$ or $\operatorname{tr}\epsilon > 0$: project to the tip, $F^E \leftarrow UV^T$.
3. Otherwise: $H = \epsilon - \delta\gamma\,\dfrac{\hat\epsilon}{\lVert\hat\epsilon\rVert_F}$, $\;F^E \leftarrow U e^{H} V^T$.

**Summary:** The three regimes are stick, separate and slide. Volume is preserved because $\operatorname{tr}H = \operatorname{tr}\epsilon$. [S9]

---

#### F32. Friction hardening

---

**Purpose:** Increase friction where sand has flowed.

**Formula:**

$$q^{n+1} = q^n + \delta q, \qquad \phi_F = h_0 + \big(h_1 q^{n+1} - h_3\big)e^{-h_2 q^{n+1}}$$

with $\delta q = 0$, $\lVert\epsilon\rVert_F$ or $\delta\gamma$ in return-mapping cases 1, 2, 3.

**Summary:** Sand that has moved becomes more frictional, which helps piles settle. [S9]

---

#### F33. Hencky elasticity

---

**Purpose:** Elastic energy and stress from logarithmic strain.

**Formula:**

$$\psi = \mu\operatorname{tr}\big((\ln\Sigma)^2\big) + \frac{\lambda}{2}\big(\operatorname{tr}\ln\Sigma\big)^2, \qquad P = U\big(2\mu\Sigma^{-1}\ln\Sigma + \lambda\operatorname{tr}(\ln\Sigma)\Sigma^{-1}\big)V^T$$

**Summary:** Elastic model paired with Drucker–Prager in MPM sand; makes the plastic projection simple in singular values. [S9]

---

#### F34. Prandtl-Hill bearing capacity

---

**Purpose:** Load a strip footing carries on weightless frictional soil before failure.

**Formula:**

$$\frac{P_u}{c\,b} = \cot\phi\left(e^{\pi\tan\phi}\tan^2\!\left(\frac{\pi}{4} + \frac{\phi}{2}\right) - 1\right)$$

**Summary:** Validation target: 46.12 at 35°, 30.14 at 30°, 14.83 at 20°. [S17]

---

#### F35. Drescher-Detournay reduced strength

---

**Purpose:** Correct bearing capacity for non-associated (low-dilatancy) soil.

**Formula:**

$$k = \frac{\cos\phi\cos\psi}{1 - \sin\phi\sin\psi}, \qquad \tan\phi^* = k\tan\phi, \qquad c^* = k\,c$$

**Summary:** Substitute $\phi^*$, $c^*$ into F34. Gives 36.05 for $\phi = 35°$, $\psi = 15°$. [S17]

---

#### F36. Prandtl wedge angle

---

**Purpose:** Angle of the rigid soil wedge that forms under a rough footing at failure.

**Formula:**

$$\delta_w = \frac{\pi}{4} + \frac{\phi}{2}$$

**Summary:** Independent of dilatancy angle (measured within about 2%); a visual check for plate-pressing tests. [S17]

---

#### F37. Terzaghi bearing capacity

---

**Purpose:** Ultimate pressure on soil under shallow footings, including soil weight.

**Formula:**

$$\text{strip: } q_{ult} = c'N_c + \sigma'_{zD}N_q + 0.5\,\gamma' B N_\gamma$$

$$\text{square: } q_{ult} = 1.3\,c'N_c + \sigma'_{zD}N_q + 0.4\,\gamma' B N_\gamma, \qquad \text{circular: } q_{ult} = 1.3\,c'N_c + \sigma'_{zD}N_q + 0.3\,\gamma' B N_\gamma$$

**Summary:** Validation target for a weight pressed into cohesionless sand, where the self-weight term dominates. [S18]

---

#### F38. Bearing capacity factors
---

**Purpose:** Factors for the Terzaghi bearing capacity as functions of friction angle.

**Formula:**

$$N_q = e^{\pi\tan\phi}\tan^2\!\left(\frac{\pi}{4} + \frac{\phi}{2}\right), \qquad N_c = (N_q - 1)\cot\phi, \qquad N_\gamma \approx \frac{2(N_q + 1)\tan\phi}{1 + 0.4\sin 4\phi}$$

**Summary:** $N_q$ and $N_c$ are the exact Prandtl–Reissner values implied by the Prandtl–Hill bearing capacity ($N_c$ is its result). They replace Terzaghi's approximate factors (Terzaghi's $N_c = 5.7$ at $\phi = 0$ against the exact 5.14). $N_\gamma$ has no exact closed form; this approximation avoids Terzaghi's chart-based $K_{p\gamma}$. [S17], [S18]

---

### C. Tearing and fracture

#### F39. Tension-only elastic degradation

---

**Purpose:** Weaken damaged material in tension only.

**Formula:**

$$\hat\Psi(F) = g(c)\,\Psi^+(F) + \Psi^-(F), \qquad g(c) = (1-r)c^2 + r, \quad r = 0.001$$

**Summary:** Torn pieces still push against each other; the small residual $r$ prevents numerical blow-up. [S1]

---

#### F40. NACC yield surface

---

**Purpose:** Plasticity that crumbles material through hardening, without a damage field.

**Formula:**

$$y(p,q) = q^2(1 + 2\beta) + M^2(p + \beta p_0)(p - p_0) \le 0, \qquad p_0 = K\sinh\!\big(\xi\max(-\alpha, 0)\big)$$

**Summary:** $\beta$ cohesion, $M$ friction, $\xi$ hardening rate, $\alpha$ hardening state. Cheap route to crumbling semisolids. [S1]

---

#### F41. Damage evolution with crack irreversibility

---

**Purpose:** Grow damage when a driving force beats geometric resistance; never heal.

**Formula:**

$$\eta\,\dot d = (1-d)\,\tilde{\mathcal H} - \big(d - l_0^2\Delta d\big), \qquad \tilde{\mathcal H}(X,t) = \max_{s\in[0,t]}\tilde D(X,s)$$

**Summary:** $d$ from 0 (healthy) to 1 (broken); the running maximum makes cracks permanent. [S2]

---

#### F42. Fibre-aware crack driving force

---

**Purpose:** Make cracks follow a material's grain (meat fibres, cheese strands).

**Formula:**

$$\tilde D = \zeta\left\langle\Phi(\sigma^+) - 1\right\rangle, \quad \sigma^+ = \sum_i\langle\sigma_i\rangle n_i\otimes n_i, \quad \Phi = \frac{A\sigma^+ : \sigma^+ A}{\sigma_c^2}, \quad A = I + \alpha_1 a_1\otimes a_1 + \alpha_2 a_2\otimes a_2$$

**Summary:** $\langle x\rangle = (x + \lvert x\rvert)/2$; $a_1$, $a_2$ are fibre directions stored per particle; $\alpha_1 = \alpha_2 = 0$ is isotropic. [S2]

---

#### F43. Explicit damage update

---

**Purpose:** GPU-friendly per-particle damage step.

**Formula:**

$$d_p^{n+1} = \min\!\left(1,\; d_p^n + \frac{\Delta t}{\eta}\Big\langle(1-d_p^n)\tilde D_p^n - \big(d_p^n - l_0^2\Delta d_p^n\big)\Big\rangle\right)$$

**Summary:** Per-particle update plus one Laplacian over neighbours. ⚠️ $\Delta t/\eta$ placement rebuilt from a damaged text layout; an implicit variant also exists in the source. [S2]

---

#### F44. Implicit phase-field damage update

---

**Purpose:** Advance the damage field one step.

**Formula:**

$$\left(\frac{4l_0 M_c(1-r)\Psi^+}{\mathcal G} + M_c + \frac{1}{\Delta t}\right)c^{n+1} - 4l_0^2 M_c\nabla^2 c^{n+1} = M_c + \frac{c^n}{\Delta t}, \qquad M_c = \frac{\mathcal G}{2R_c l_0}$$

**Summary:** *Alternative option:* Stable at large time steps, but needs a sparse linear solve on the grid every step; the explicit update is per particle. A linear, heat-equation-like solve per step on the grid. [S1]

---

#### F45. Local damage rule

---

**Purpose:** Cheap per-particle damage without a field solve.

**Formula:**

$$c^{n+1} = \min\!\left(c^n,\; 1 - H_s\left(1 - \frac{\sigma_f}{\sigma_M}\right)\right)$$

**Summary:** *Alternative option:* The cheapest damage rule, but the source reports cracks that depend on mesh direction and resolution. $H_s$ controls brittleness. Simple but cracks depend on mesh direction and resolution. [S1]

---

#### F46. Edge stress and bond breaking

---

**Purpose:** Break individual bonds (mesh edges) when smoothed stress along them is too high.

**Formula:**

$$\sigma_{ij} = \hat d_{ij}^T\sigma\,\hat d_{ij}, \qquad \sigma_{cen} = \sum_{r\in R_d}\omega(r - r_{cen})\,\sigma(r), \qquad \zeta_{ij} = \begin{cases}0 & \sigma^*_{ij} < \sigma_{thres}\\ 1 & \sigma^*_{ij} \ge \sigma_{thres}\end{cases}$$

**Summary:** Stress projected onto each edge, averaged over a neighbourhood, then thresholded; broken edges never heal and carry no normal stress. Maps directly onto a particle bond network. [S3]

---

#### F47. Graph FEM plastic flow amount

---

**Purpose:** Fraction of yield excess converted to plastic flow per step.

**Formula:**

$$\beta = \operatorname{clamp}\!\left(\frac{\nu\,\big(\lVert\sigma\rVert_2 - \sigma_{thres} - K\delta\big)}{\lVert\sigma\rVert_2},\; 0,\; 1\right)$$

**Summary:** *Alternative option:* Includes hardening, but its flow fraction does not scale with the time step, so results change with step size; the stretching plasticity rate includes the time step. $\nu$ flow rate, $K$ hardening, $\delta$ accumulated flow; used with $F = F_e F_p$ for ductile tearing. [S3]

---

#### F48. Strain split and deviatoric yield

---

**Purpose:** Separate permanent from elastic deformation and test for yield.

**Formula:**

$$\varepsilon = \varepsilon^p + \varepsilon^e, \qquad \varepsilon' = \varepsilon^e - \frac{\operatorname{Tr}(\varepsilon^e)}{3}I, \qquad \text{yield if } \gamma_1 < \lVert\varepsilon'\rVert$$

**Summary:** von Mises on shape change only, so volume is preserved; $\gamma_1$ is the elastic limit. [S4]

---

#### F49. Plastic flow with plastic limit

---

**Purpose:** Update permanent strain and cap total plastic deformation.

**Formula:**

$$\Delta\varepsilon^p = \frac{\lVert\varepsilon'\rVert - \gamma_1}{\lVert\varepsilon'\rVert}\,\varepsilon', \qquad \varepsilon^p := \big(\varepsilon^p + \Delta\varepsilon^p\big)\min\!\left(1,\; \frac{\gamma_2}{\lVert\varepsilon^p + \Delta\varepsilon^p\rVert}\right)$$

**Summary:** $\gamma_1$ (stretch before permanent deformation) and $\gamma_2$ (maximum permanent stretch) are the two tearing controls; tiny values (0.001) already change results. Per-bond version: move rest length toward current length. [S4]

---

#### F50. Green strain and membrane stress

---

**Purpose:** In-plane strain and stress of a thin sheet triangle.

**Formula:**

$$G = \tfrac12(F^TF - I), \qquad \sigma = k_s\big((1-\nu)G + \nu\operatorname{tr}(G)I\big), \qquad k_s = \frac{Yh}{1-\nu^2}$$

**Summary:** Stretching response of skins, crusts and peels modelled as sheets. [S5]

---

#### F51. Discrete bending strain and sheet energy

---

**Purpose:** Bending measure and total elastic energy of a triangle mesh sheet.

**Formula:**

$$S = \frac{1}{2A}\sum_{i=1}^3(\theta_i - \theta_i^R)\,l_i\,t_it_i^T, \qquad E = \sum_{\text{faces}}\frac{A_i}{2}G_i:\sigma_i + \sum_{\text{edges}}k_b\frac{l_i^2}{8A_i}(\theta_i - \theta_i^R)^2, \qquad k_b = \frac{\alpha k_s h^2}{12}$$

**Summary:** Dihedral-angle bending plus membrane energy; $\alpha$ allows anisotropic out-of-plane stiffness. [S5]

---

#### F52. Separation strength

---

**Purpose:** Mesh-independent test of whether a vertex fractures, and along which plane.

**Formula:**

$$q_1 = \sigma_0^+(t_0 - s) + \sum_{i=0}^{N-1}\sigma_i^+(t_{i+1} - t_i) + \sigma_N^+(-s - t_N), \qquad s_{sep} = \min\big(q_1\cdot\hat q,\; -q_2\cdot\hat q\big), \qquad \hat q = \frac{q_1 - q_2}{\lVert q_1 - q_2\rVert}$$

Fracture if $s_{sep} > \tau = \tau_V h$; bound $s_{sep} \le 2\max\lambda_i(\sigma_i^+)$.

**Summary:** Tension-only stress summed on each side of a candidate plane (200 sampled directions); break only the strongest vertex, then re-evaluate. [S5]

---

#### F53. Bending fracture factor

---

**Purpose:** Include extra outer-surface stretch from bending in the fracture test.

**Formula:**

$$F_B = F\left(I + \tfrac12\,\alpha\,h\,II^*\right)$$

**Summary:** $II^*$ is the second fundamental form with negative eigenvalues flipped. ⚠️ Printed with $\alpha$; the text derivation gives $\tfrac12 h\kappa$. [S5]

---

#### F54. Stretching plasticity rate and thinning

---

**Purpose:** Permanent stretching of a sheet and the thinning it causes.

**Formula:**

$$\gamma = \operatorname{clamp}\big(\Delta t\,\mu(\sigma_M - \sigma_0),\,0,\,1\big), \qquad \sigma_M = \sqrt3\left\lVert\sigma - \tfrac13\operatorname{tr}(\sigma)I\right\rVert, \qquad h \leftarrow h\,\det(\Sigma)^{-\gamma/3}$$

**Summary:** Fraction $\gamma$ of the volume-preserving elastic stretch becomes permanent ($F_{el} = F F_p$); thinned regions bend and break more easily. ⚠️ Exponent placement in the $F_p$ update did not survive extraction (paper Eq. 9). [S5]

---

#### F55. Bending plasticity update

---

**Purpose:** Permanent bending once curvature exceeds a yield value.

**Formula:**

$$S_p \leftarrow S_p + \frac{S_{el}}{\lVert S_{el}\rVert}\big(\lVert S_{el}\rVert - \kappa_0\big)$$

**Summary:** Lets sheets such as foil or a bent crust hold their shape after bending. [S5]

---

#### F56. Fracture remeshing size

---

**Purpose:** Refine resolution where fracture may start.

**Formula:**

$$M_{frac} = \left(\frac{2\lambda}{\tau\,x_{min}}\right)^2 I$$

**Summary:** Sizing tensor for the remesher; the particle analogue is adding resolution near high bond stress. [S5]

---

### D. Liquids and continuum transfer

#### F57. Navier-Stokes momentum with variable viscosity

---

**Purpose:** Equations of motion for an incompressible liquid, including viscosity that may vary in space.

**Formula:**

$$\frac{Du}{Dt} + \frac{1}{\rho}\nabla p = g + \frac{1}{\rho}\nabla\cdot\big(\mu(\nabla u + \nabla u^T)\big), \qquad \nabla\cdot u = 0$$

**Summary:** Solved by splitting each step into advection, body forces ($u \leftarrow u + \Delta t\,g$) and projection. Reduces to the inviscid Euler equations at $\mu = 0$ and to the constant-viscosity form when $\mu$ is uniform, but only this form conserves angular momentum with varying viscosity (mud, slime). Integrate the viscous term implicitly: explicit integration needs $\Delta t < \Delta x^2\rho/(12\mu_{max})$. [S7]

---

#### F58. Pressure projection

---

**Purpose:** Make a grid velocity field divergence-free.

**Formula:**

$$\nabla\cdot\left(\frac{\Delta t}{\rho}\nabla p\right) = \nabla\cdot u^*, \qquad u^{n+1} = u^* - \frac{\Delta t}{\rho}\nabla p, \qquad \text{solve } Ap = b \text{ until } \lVert b - Ap\rVert \le 10^{-6}\lVert b\rVert$$

**Summary:** Discretised on a staggered (MAC) grid as one equation per fluid cell; solve with MIC(0)-preconditioned conjugate gradient ($\tau = 0.97$, $\sigma = 0.25$). Usually the most expensive step of a grid liquid. [S6], [S7]

---

#### F59. CFL time-step limit

---

**Purpose:** Limit how far liquid moves per step, in grid cells.

**Formula:**

$$\Delta t \le \frac{C\,\Delta x}{u_{max}}, \qquad u_{max} = \max\lvert u^n\rvert + \sqrt{C\,\Delta x\,\lvert g\rvert}, \qquad C \approx 5$$

**Summary:** Including gravity in the speed estimate keeps the limit finite when the liquid starts at rest. An accuracy limit, since semi-Lagrangian advection is unconditionally stable; also clamp the step to the end of the frame. Blender exposes $C$ as "CFL Number" and subdivides steps when it is exceeded. [S7], [S10]

---

#### F60. Ghost-fluid free surface

---

**Purpose:** Place the free-surface pressure condition at the true liquid surface inside a grid cell.

**Formula:**

$$\theta = \frac{\phi_i}{\phi_i - \phi_{i+1}}, \qquad p^G_{i+1} = \frac{\phi_{i+1}}{\phi_i}\,p_i, \qquad u^{n+1}_{i+1/2} = u_{i+1/2} - \frac{\Delta t}{\rho}\,\frac{\phi_{i+1} - \phi_i}{\phi_i}\,\frac{p_i}{\Delta x}$$

**Summary:** For a water cell $i$ ($\phi \le 0$) next to an air cell $i+1$. Replaces setting $p = 0$ at the air cell's centre, which leaves voxel staircases and frozen sub-cell ripples; it only increases the matrix diagonal. Guard against $\phi_i$ near 0. [S7]

---

#### F61. Surface tension pressure jump

---

**Purpose:** Pull a liquid surface toward smaller area.

**Formula:**

$$p_{surface} = \gamma\,\kappa, \qquad \kappa = \nabla\cdot\nabla\phi, \qquad \Delta t \le O\!\left(\Delta x^{3/2}\sqrt{\rho/\gamma}\right)$$

**Summary:** Used as the ghost-fluid surface pressure instead of 0. $\gamma \approx 0.073$ J/m² for water and air. Explicit treatment gives the time-step limit shown; it matters only for small-scale drops and splashes. [S7]

---

#### F62. MPM governing equations

---

**Purpose:** Mass, momentum and deformation evolution for continuum materials on particles.

**Formula:**

$$\frac{D\rho}{Dt} + \rho\nabla\cdot v = 0, \qquad \rho\frac{Dv}{Dt} = \nabla\cdot\sigma + \rho g, \qquad \frac{DF}{Dt} = (\nabla v)F$$

**Summary:** The framework under MPM sand, snow and tearing; particles carry $F$ and material state. [S9]

---

#### F63. PIC and FLIP particle to grid

---

**Purpose:** Transfer particle velocities to the grid as a normalised weighted average.

**Formula:**

$$u_{i+1/2,j,k} = \frac{\sum_p u_p\,k(x_p - x_{i+1/2,j,k})}{\sum_p k(x_p - x_{i+1/2,j,k})}, \qquad k(x,y,z) = h\!\left(\tfrac{x}{\Delta x}\right)h\!\left(\tfrac{y}{\Delta x}\right)h\!\left(\tfrac{z}{\Delta x}\right), \quad h(r) = \max(0,\,1 - \lvert r\rvert)$$

**Summary:** Shared first half of the PIC and FLIP schemes (APIC uses its own transfer). Accumulate numerator and weight per grid face in one pass over particles, divide in a second pass; extrapolate into empty cells. Start with 8 particles per cell, reseed cells that run low, delete above about 12. [S7]

---

#### F64. PIC and FLIP grid to particle

---

**Purpose:** Update particle velocities from the grid, from fully smoothed (PIC) to fully detail-preserving (FLIP).

**Formula:**

$$u_p^{new} = \alpha\,\text{interp}\big(u_{grid}^{new}, x_p\big) + (1-\alpha)\Big(u_p^{old} + \text{interp}\big(u_{grid}^{new} - u_{grid}^{old}, x_p\big)\Big), \qquad 0 \le \alpha \le 1$$

**Summary:** $\alpha = 1$ is PIC (replaces the particle velocity: stable, smooth, viscous-looking); $\alpha = 0$ is FLIP (adds only the grid's change: detailed and splashy, but can develop noise); a small $\alpha$ blends them. Blender's "FLIP Ratio" is $1 - \alpha$. [S6], [S7], [S10]

---

#### F65. PIC fraction from viscosity

---

**Purpose:** Choose the PIC fraction so its numerical smoothing matches a physical viscosity.

**Formula:**

$$\alpha = \min\!\left(1,\; \frac{6\,\Delta t\,\nu}{\Delta x^2}\right)$$

**Summary:** $\nu$ is kinematic viscosity. Makes the blend a physical setting instead of a guess; if the formula exceeds 1, use PIC and add an explicit viscosity step. [S7]

---

#### F66. APIC particle to grid

---

**Purpose:** Transfer particle momentum to the grid keeping rotation and shear.

**Formula:**

$$m_i v_i = \sum_p w_{ip}\,m_p\Big(v_p + B_p D_p^{-1}(x_i - x_p)\Big), \qquad D_p = \sum_i w_{ip}(x_i - x_p)(x_i - x_p)^T$$

**Summary:** $D_p = \tfrac14\Delta x^2 I$ (quadratic) or $\tfrac13\Delta x^2 I$ (cubic); $C_p = B_pD_p^{-1}$ is the particle's velocity gradient. [S8]

---

#### F67. APIC grid to particle

---

**Purpose:** Transfer grid velocities back to particles with an affine velocity field.

**Formula:**

$$v_p = \sum_i w_{ip}\,\tilde v_i, \qquad B_p = \sum_i w_{ip}\,\tilde v_i\,(x_i - x_p)^T$$

**Summary:** The APIC option among the three transfer schemes (PIC, FLIP, APIC): nearly dissipation-free without FLIP's noise, conserves angular momentum, and keeps vortices; costs 9 extra values per particle. [S8], [S10]

---

#### F68. SPH field interpolation

---

**Purpose:** Evaluate any field from neighbouring particles.

**Formula:**

$$A(x) = \sum_j m_j\frac{A_j}{\rho_j}W(x - x_j, h)$$

**Summary:** Basis of SPH liquids; $W$ is a smoothing kernel with support radius $h$. [S12]

---

#### F69. SPH pressure force for mixed sizes

---

**Purpose:** Symmetric pressure force between particles of different sizes.

**Formula:**

$$F_i^P = -k\,m_i\sum_{j\ne i}m_j\left(\frac{\rho_i - \rho_0}{\rho_i^2} + \frac{\rho_j - \rho_0}{\rho_j^2}\right)\frac{\nabla_i(W_{h_i} + W_{h_j})}{2}$$

**Summary:** Averaging kernel gradients over both support radii keeps forces equal and opposite. [S12]

---

#### F70. SPH artificial viscosity

---

**Purpose:** Damp relative motion between liquid particles.

**Formula:**

$$F_i^{Vis} = -\eta\,m_i\sum_{j\ne i}m_j\,\Pi_{ij}\,\frac{\nabla_i(W_{h_i} + W_{h_j})}{2}$$

**Summary:** $\eta$ sets viscosity; higher values suit mud and semisolids. $\Pi_{ij}$ is the standard artificial-viscosity term. [S12]

---

#### F71. Mixed-size neighbour criterion

---

**Purpose:** Decide neighbours when particles have different support radii.

**Formula:**

$$\lVert x_i - x_j\rVert \le 2\max(h_i, h_j)$$

**Summary:** Guarantees symmetric neighbour relations for adaptive or polydisperse particles; build cells from the largest size. [S11], [S12]

---

#### F72. Adaptive character field

---

**Purpose:** Score where a liquid needs fine particles.

**Formula:**

$$c_f = \frac{\alpha\,\dfrac{d_i}{d_{max}} + \beta\left(1 - \dfrac{f_i}{f_{max}}\right) + \gamma\,\dfrac{N_i}{N_{max}}}{\alpha + \beta + \gamma}, \qquad d_i = \min\{d_{s_i}, d_{b_{i1}}, d_{b_{i2}}, \ldots\}$$

**Summary:** Low near surfaces and boundaries, under high pressure, or for isolated droplets. Also a candidate score for choosing the 64-bit precise set. [S11], [S12]

---

#### F73. Split and merge rule

---

**Purpose:** Change particle resolution from the character field.

**Formula:**

$$\text{split if } c_f < T, \qquad \text{merge if } c_f \ge 2T, \qquad \text{keep otherwise}$$

**Summary:** One particle splits into up to seven, spread out to avoid pressure spikes; pays off above about 10,000 particles. Radius and mass sharing are not specified by the source. [S11], [S12]

---

#### F74. Collider pre-subdivision criterion

---

**Purpose:** Make collider triangles small enough for box tests to be accurate.

**Formula:**

$$\max\big(\lVert a_j - b_j\rVert,\, \lVert c_j - b_j\rVert,\, \lVert a_j - c_j\rVert\big) < r_i$$

**Summary:** Subdivide collider triangles once before simulating so no edge is longer than a particle radius. [S11], [S12]

---

### E. Neighbour search

#### F75. Uniform grid cell index
---

**Purpose:** Bin particles into grid cells for neighbour search.

**Formula:**

$$c = \left\lfloor\frac{x - x_{min}}{\Delta x}\right\rfloor, \qquad \Delta x \approx \text{largest grain diameter}$$

**Summary:** Key each cell with the Morton order (F77), sort with counting-sort offsets (F78), then read neighbours from the 27 surrounding cells. Sorting is faster and deterministic compared with atomics. [S19], [S22]

---

#### F76. Spatial hash

---

**Purpose:** Map unbounded cell coordinates to a fixed-size table.

**Formula:**

$$\text{hash}(i,j,k) = \big(i\,p_1 \oplus j\,p_2 \oplus k\,p_3\big) \bmod n, \qquad p_1 = 73856093,\; p_2 = 19349663,\; p_3 = 83492791$$

**Summary:** *Alternative option:* For unbounded or very sparse domains; in bounded domains the plain grid key avoids hash collisions and extra distance checks. For very large or sparse domains; use a prime table size and 64-bit integers. Collisions put unrelated cells together, so distances must still be checked. [S21]

---

#### F77. Morton Z-order key

---

**Purpose:** Number cells so that nearby cells have nearby keys.

**Formula:**

$$z(i,j,k) = \ldots k_1 j_1 i_1\, k_0 j_0 i_0 \quad (\text{bits of } i, j, k \text{ interleaved})$$

**Summary:** Improves memory locality of sorted particle arrays; with nearly sorted data, full reorders can be infrequent (every 100th step in the source). [S22]

---

#### F78. Counting sort offsets

---

**Purpose:** Place particles into bins without a full key sort.

**Formula:**

$$\text{offset}[b] = \sum_{b' < b}\text{count}[b'], \qquad \text{position}(p) = \text{offset}[\text{bin}(p)] + \text{index in bin}(p)$$

**Summary:** Count, prefix-sum, scatter: $O(N + B)$. In PyTorch: `bincount`, `cumsum`, then a stable sort for deterministic in-bin order. [S20]

---

#### F79. Neighbour index delta encoding

---

**Purpose:** Store sorted neighbour lists compactly.

**Formula:**

$$\Delta_k = j_k - j_{k-1}, \qquad j_1 < j_2 < \ldots$$

**Summary:** Differences are mostly +1 or +2 after spatial sorting; variable-byte encoding saves up to 87% memory. For very large particle counts. [S23]

---

### F. Floating-point precision

#### F80. Rounding error model and machine epsilon

---

**Purpose:** Bound the error of one floating-point operation.

**Formula:**

$$\operatorname{fl}(a\circ b) = (a\circ b)(1 + \delta), \qquad \lvert\delta\rvert \le \varepsilon = \frac{\beta}{2}\beta^{-p}$$

**Summary:** $\varepsilon = 2^{-24}$ for float32 and $2^{-53}$ for float64. Addition is not associative, so summation order changes results. [S24]

---

#### F81. Kahan compensated summation

---

**Purpose:** Sum many values with error nearly independent of their count.

**Formula:**

$$y = x - c, \qquad t = s + y, \qquad c = (t - s) - y, \qquad s = t$$

**Summary:** Carries the lost low-order part in $c$; useful for long per-grain accumulations. [S24]

---

#### F82. Fused multiply-add

---

**Purpose:** Compute a product plus a sum with a single rounding.

**Formula:**

$$\operatorname{FMA}(X, Y, Z) = \operatorname{rn}(X\times Y + Z) \quad\text{vs}\quad \operatorname{rn}\big(\operatorname{rn}(X\times Y) + Z\big)$$

**Summary:** More accurate, and a main reason CPU and GPU results differ in the last bits. [S25]

---

#### F83. Floating-point step size

---

**Purpose:** Gap between neighbouring representable values for a given format.

**Formula:**

$$\text{step} = 2^{\,e - m} \text{ for values in } [2^e, 2^{e+1}), \qquad \text{relative step} = 2^{-m}\text{ to }2^{-(m+1)}$$

**Summary:** $m$ mantissa bits: FP8 E4M3 ($m = 3$, 125 mm near 1 m), FP16 ($m = 10$, 0.98 mm), FP32 ($m = 23$, 0.00012 mm). Justifies 32-bit minimum for positions. [S26]

---

#### F84. Reproducible summation by pre-rounding

---

**Purpose:** Sum values to the same bits in any order.

**Formula:**

$$q_i = \operatorname{fl}\big((x_i + \sigma) - \sigma\big), \qquad r_i = x_i - q_i, \qquad \sum_i q_i \text{ exact in any order}$$

**Summary:** $\sigma$ is a power of two chosen from $\max\lvert x_i\rvert$ and $n$; repeat on remainders for accuracy. Alternative to sort-then-sum for deterministic mode. ⚠️ Source recalled. [S27]

---

## 4. List of cited sources

Each entry links to its extract. ⚠️ marks sources whose extract was written from knowledge rather than read from the original.

| Tag | Source | Extract |
| --- | --- | --- |
| S1 | Wolper, Fang, Li, Lu, Gao, Jiang, "CD-MPM: Continuum Damage Material Point Methods for Dynamic Fracture Animation", ACM TOG 38(4), 2019. [DOI](https://doi.org/10.1145/3306346.3322949) | [extract](extracts/01-tearing/wolper-2019-cd-mpm.md) |
| S2 | Wolper et al., "AnisoMPM: Animating Anisotropic Damage Mechanics", ACM TOG 39(4), 2020. [DOI](https://doi.org/10.1145/3386569.3392428) | [extract](extracts/01-tearing/wolper-2020-anisompm.md) |
| S3 | Mandal, Chaudhuri, Chaudhuri, "Remeshing-Free Graph-Based Finite Element Method for Fracture Simulation", Computer Graphics Forum 42(1), 2023. [DOI](https://doi.org/10.1111/cgf.14725) | [extract](extracts/01-tearing/mandal-2023-graph-fem-fracture.md) |
| S4 | O'Brien, Bargteil, Hodgins, "Graphical Modeling and Animation of Ductile Fracture", ACM TOG 21(3), 2002. [Link](https://escholarship.org/uc/item/1pb4c21m) | [extract](extracts/01-tearing/obrien-2002-ductile-fracture.md) |
| S5 | Pfaff, Narain, de Joya, O'Brien, "Adaptive Tearing and Cracking of Thin Sheets", ACM TOG 33(4), 2014. [Link](https://escholarship.org/uc/item/1mc8b47z) | [extract](extracts/01-tearing/pfaff-2014-adaptive-tearing-thin-sheets.md) |
| S6 | Zhu, Bridson, "Animating Sand as a Fluid", ACM TOG 24(3), 2005. [Link](https://www.cs.ubc.ca/~rbridson/docs/zhu-siggraph05-sandfluid.pdf) | [extract](extracts/02-flip-and-granular-liquids/zhu-bridson-2005-sand-as-fluid.md) |
| S7 | Bridson, *Fluid Simulation for Computer Graphics*, 2nd ed., CRC Press, 2015. [Link](https://www.routledge.com/Fluid-Simulation-for-Computer-Graphics/Bridson/p/book/9781482232837) | [extract](extracts/02-flip-and-granular-liquids/bridson-2015-fluid-simulation-book.md) |
| S8 | Jiang, Schroeder, Selle, Teran, Stomakhin, "The Affine Particle-In-Cell Method", ACM TOG 34(4), 2015. [DOI](https://doi.org/10.1145/2766996) | [extract](extracts/02-flip-and-granular-liquids/jiang-2015-apic.md) |
| S9 | Klár, Gast, Pradhana, Fu, Schroeder, Jiang, Teran, "Drucker-Prager Elastoplasticity for Sand Animation", ACM TOG 35(4), 2016. [DOI](https://doi.org/10.1145/2897824.2925906) | [extract](extracts/02-flip-and-granular-liquids/klar-2016-drucker-prager-sand.md) |
| S10 | Blender Foundation, Blender Manual 5.0, "Fluid Domain Settings". [Link](https://docs.blender.org/manual/en/5.0/physics/fluid/type/domain/settings.html) | [extract](extracts/02-flip-and-granular-liquids/blender-manual-fluid-domain-settings.md) |
| S11 | He, Chen, Wang, Yan, Cao, Peng, "A New Adaptive Model for Real-time Fluid Simulation with Complex Boundaries", IEEE CAD/Graphics, 2009. [DOI](https://doi.org/10.1109/CADCG.2009.5246934) | [extract](extracts/02-flip-and-granular-liquids/he-2009-adaptive-sph-complex-boundaries.md) |
| S12 | He, Chen, Wang, Cao, Yan, Peng, "Real-time Adaptive Fluid Simulation with Complex Boundaries", The Visual Computer 26(4), 2010. [DOI](https://doi.org/10.1007/s00371-010-0426-1) | [extract](extracts/02-flip-and-granular-liquids/he-2010-adaptive-fluid-complex-boundaries-journal.md) |
| S13 | Cundall, Strack, "A Discrete Numerical Model for Granular Assemblies", Géotechnique 29(1), 1979. [DOI](https://doi.org/10.1680/geot.1979.29.1.47) | [extract](extracts/03-bearing-shear-contact/cundall-strack-1979-dem.md) |
| S14 | Vetter, "TinyDEM: Minimal Open Granular DEM Code with Sliding, Rolling and Twisting Friction", Computer Physics Communications 320, 2025. [arXiv](https://arxiv.org/abs/2507.12610) | [extract](extracts/03-bearing-shear-contact/vetter-2025-tinydem.md) |
| S15 | Johnson, *Contact Mechanics*, Cambridge University Press, 1985. [DOI](https://doi.org/10.1017/CBO9781139171731) | [extract](extracts/03-bearing-shear-contact/johnson-1985-contact-mechanics.md) |
| S16 | Jop, Forterre, Pouliquen, "A Constitutive Law for Dense Granular Flows", Nature 441, 2006. [arXiv](https://arxiv.org/abs/cond-mat/0612110) | [extract](extracts/03-bearing-shear-contact/jop-2006-mu-i-rheology.md) |
| S17 | Hamlaoui, Oueslati, Lamri, de Saxcé, "Finite Element Analysis of the Plastic Limit Load and the Collapse Mechanism of Strip Foundations with Non-Associated Drucker–Prager Model", European Journal of Environmental and Civil Engineering, 2015. [DOI](https://doi.org/10.1080/19648189.2015.1005162) | [extract](extracts/03-bearing-shear-contact/hamlaoui-2015-non-associated-drucker-prager-footings.md) |
| S18 | Terzaghi, *Theoretical Soil Mechanics*, Wiley, 1943, via the Wikipedia article on bearing capacity. [Link](https://en.wikipedia.org/wiki/Bearing_capacity) | [extract](extracts/03-bearing-shear-contact/terzaghi-1943-bearing-capacity.md) |
| S19 | Green, "Particle Simulation using CUDA", NVIDIA, 2010. [Link](https://developer.download.nvidia.com/assets/cuda/files/particles.pdf) | [extract](extracts/04-gpu-neighbour-search/green-2010-cuda-particles.md) |
| S20 | Hoetzlein, "Fast Fixed-Radius Nearest Neighbors: Interactive Million-Particle Fluids", GTC 2014. [Link](https://ramakarl.com/pdfs/2014_Hoetzlein_FastFixedRadius_Neighbors.pdf) | [extract](extracts/04-gpu-neighbour-search/hoetzlein-2014-fixed-radius-neighbours.md) |
| S22 | Ihmsen, Akinci, Becker, Teschner, "A Parallel SPH Implementation on Multi-Core CPUs", Computer Graphics Forum 30(1), 2011. [Link](https://diglib.eg.org/handle/10.1111/v30i1pp099-112) | [extract](extracts/04-gpu-neighbour-search/ihmsen-2011-parallel-sph-cpu.md) |
| S23 | Band, Gissler, Teschner, "Compressed Neighbour Lists for SPH", Computer Graphics Forum 39(1), 2020. [DOI](https://doi.org/10.1111/cgf.13890) | [extract](extracts/04-gpu-neighbour-search/band-2020-compressed-neighbour-lists.md) |
| S24 | Goldberg, "What Every Computer Scientist Should Know About Floating-Point Arithmetic", ACM Computing Surveys 23(1), 1991. [DOI](https://doi.org/10.1145/103162.103163) | [extract](extracts/05-floating-point-precision/goldberg-1991-floating-point.md) |
| S25 | Whitehead, Fit-Florea, "Precision and Performance: Floating Point and IEEE 754 Compliance for NVIDIA GPUs", NVIDIA. [Link](https://docs.nvidia.com/cuda/archive/13.0.3/floating-point/index.html) | [extract](extracts/05-floating-point-precision/nvidia-floating-point-ieee754-gpus.md) |
| S26 | Micikevicius et al., "FP8 Formats for Deep Learning", arXiv:2209.05433, 2022. [arXiv](https://arxiv.org/abs/2209.05433) | [extract](extracts/05-floating-point-precision/micikevicius-2022-fp8-formats.md) |
| S27 | ⚠️ Demmel, Nguyen, "Fast Reproducible Floating-Point Summation", ARITH 2013. [Slides](https://bebop.cs.berkeley.edu/reproblas/docs/talks/SC13.pdf) | [extract](extracts/05-floating-point-precision/demmel-nguyen-2013-reproducible-summation.md) |

Not cited here: the PyTorch deterministic-algorithms page (no formulas), and Teschner et al. 2003 (S21), whose only formula, the spatial hash, was removed as redundant for bounded domains.
