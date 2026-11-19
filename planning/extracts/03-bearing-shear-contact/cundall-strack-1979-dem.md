# A Discrete Numerical Model for Granular Assemblies

- **Authors:** Peter A. Cundall, Otto D. L. Strack
- **Published:** Géotechnique 29(1), 1979, pp. 47–65 · DOI [10.1680/geot.1979.29.1.47](https://doi.org/10.1680/geot.1979.29.1.47)
- **Source used:** read in full from a copy supplied by the repository owner (an OCR scan; equations were rebuilt from imperfect text recognition). The PDF is not stored in this repository.
- **Related:** [vetter-2025-tinydem.md](vetter-2025-tinydem.md) is a modern, open 3D implementation of the same method with Hertz–Mindlin contacts and rolling friction. This extract keeps the original model and its reasoning.

## Summary

The founding paper of the discrete element method (DEM), the most-cited paper in Géotechnique. Granular material is simulated grain by grain (2D discs in the program BALL). Contacts are springs with Coulomb friction, motion is integrated explicitly, and the time step is chosen so small that a disturbance cannot travel past a grain's immediate neighbours in one step. That makes each grain's forces depend only on its current contacts, so no global system of equations is ever solved. The model was validated by reproducing force-chain photographs from a photoelastic disc experiment (De Josselin de Jong and Verruijt 1969).

## Key equations

**Contact test.** Two discs $x$ and $y$ with centre distance $D$ touch only if

$$D < R_{(x)} + R_{(y)}$$

Grains may overlap slightly; the overlap stands in for the real deformation of the grains, which is small compared with the movement of the assembly.

**Contact frame.** Unit normal from $x$ to $y$, and tangent obtained by a clockwise 90° rotation:

$$e_i = \frac{y_i - x_i}{D} = (\cos\alpha, \sin\alpha), \qquad t_i = (e_2, -e_1)$$

**Relative velocity at the contact** (velocities $\dot x_i, \dot y_i$; angular velocities $\dot\theta$, counter-clockwise positive):

$$\dot X_i = (\dot x_i - \dot y_i) - \big(\dot\theta_{(x)} R_{(x)} + \dot\theta_{(y)} R_{(y)}\big)\, t_i$$

**Normal and shear components**, integrated over one step $\Delta t$:

$$\Delta n = \big((\dot x_i - \dot y_i)\, e_i\big)\,\Delta t, \qquad \Delta s = \Big((\dot x_i - \dot y_i)\, t_i - \big(\dot\theta_{(x)} R_{(x)} + \dot\theta_{(y)} R_{(y)}\big)\Big)\,\Delta t$$

**Incremental force–displacement law** (normal and shear stiffnesses $k_n$, $k_s$):

$$\Delta F_n = k_n\,\Delta n, \qquad \Delta F_s = k_s\,\Delta s$$

$$(F_n)_N = (F_n)_{N-1} + \Delta F_n, \qquad (F_s)_N = (F_s)_{N-1} + \Delta F_s$$

Forces are accumulated from increments, so each contact remembers its shear history.

**Coulomb friction cap** ($\phi_\mu$ the smaller of the two grains' friction angles, $c$ the smaller cohesion):

$$(F_s)_{max} = F_n \tan\phi_\mu + c$$

If $\lvert F_s\rvert$ exceeds the cap, it is set equal to the cap, keeping its sign: the contact slides.

**Equations of motion** (moment from shear forces, $\sum M = \sum F_s R$):

$$m\,\ddot x_i = \sum F_i, \qquad I\,\ddot\theta = \sum M$$

**Time-centred integration** (velocities at half steps, positions at whole steps; gravity $g_i$ adds $m g_i$ to the force sum):

$$(\dot x_i)_{N+\frac12} = (\dot x_i)_{N-\frac12} + \frac{\sum F_i}{m}\,\Delta t, \qquad (x_i)_{N+1} = (x_i)_N + (\dot x_i)_{N+\frac12}\,\Delta t$$

and the same for $\theta$ with $\sum M / I$.

**Contact damping** (dashpots at the contact, proportional to stiffness; shear damping is switched off while the contact slides):

$$D_n = c_n\,\dot n, \qquad D_s = c_s\,\dot s, \qquad c_n = \beta k_n, \quad c_s = \beta k_s$$

**Global damping** (dashpots to ground, proportional to mass, $C = \alpha m$, $C^* = \alpha I$), integrated with the central difference so it stays stable:

$$(\dot x_i)_{N+\frac12} = \frac{(\dot x_i)_{N-\frac12}\left(1 - \alpha\frac{\Delta t}{2}\right) + \dfrac{\sum (F_i + D_i)}{m}\,\Delta t}{1 + \alpha\frac{\Delta t}{2}}$$

**Stability.** The explicit scheme is stable only for a fraction of the critical step of a mass on a spring:

$$\Delta t_{crit} = 2\sqrt{m/k}$$

## Findings from the tests

- **Damping does not change the equilibrium force,** only how quickly it is reached. Without damping, forces oscillated about equilibrium by about 15%.
- **Faster loading means bigger oscillations** at the same stiffness; keeping inertial forces small keeps the assembly near equilibrium throughout.
- **Shear-to-normal stiffness ratio.** Mindlin's theory puts $k_s / k_n$ between about 2/3 and 1 for elastic contacts (stated in the scan as a range of "3 to 1", read here as 2/3 to 1; Johnson's Contact Mechanics, eq. 7.43–7.44, gives $k_s/k_n = 2(1-\nu)/(2-\nu)$ = 0.67–0.86, which supports that reading); the tests used 2/3 to 1. The ratio barely matters at low friction (15°), because contacts slip early, but matters noticeably at 30°.
- **Stiff and soft grains behave differently** even at the same stiffness ratio, so stiffness is a real material property, not just a numerical setting.
- **Validation friction values** taken from the photoelastic test: $\tan\phi_\mu$ = 0.45 between discs and 0.17 between discs and walls.
- **Scale:** about 1,500 particles fitted in a minicomputer with 64K 32-bit words.

## Takeaways for the grain solver

- **"Disturbances travel at most one grain per step"** is the same reason the concept sheet substeps: PBD with many small substeps keeps each correction local, just as small DEM time steps do.
- **Accumulated shear force is the friction memory.** DEM keeps $F_s$ per contact from step to step; PBD friction resets each substep. If piles creep in testing, store a per-contact tangential offset in the contact list (the TinyDEM extract gives the full update).
- **Separate wall friction.** BALL uses a different, lower friction for grain–wall contacts (0.17 against 0.45). The solver should give colliders their own friction coefficient rather than reusing the grain–grain one.
- **Mass-proportional global damping** with the central-difference form above is a cheap, unconditionally stable way to let piles settle; it matches the concept sheet's "light damping" step.
- **Take the smaller friction and cohesion of the two grains** at each contact when grains of different materials touch.

## Cautions

- 2D discs only; the 3D extension came later. TinyDEM covers the 3D, rotational and rolling-friction details.
- The PDF is a scan and its text layer has recognition errors; the equations above were rebuilt from it and from the method's standard form. Check against the page images before implementing.
