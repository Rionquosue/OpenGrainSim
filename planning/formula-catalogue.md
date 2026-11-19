# Formula catalogue

All 84 formulas from the reference extracts, sorted into chapters by what they are for and which solving method uses them. Numbers (F01–F84) match [formulatable.md](formulatable.md), which holds each formula, its purpose and its sources. Formulas marked **option** are alternatives to the formula listed before them, kept so the solver can offer a choice.

## 1. Grain contact: discrete grains (DEM and PBD)

The version 1 solver's core: how two grains detect each other, push back, stick or slide, lose energy, and how the simulation steps forward.

### 1.1 Contact detection and geometry

| ID | Formula | Purpose |
| --- | --- | --- |
| F01 | Effective contact properties | Combine two bodies' radii, masses and elastic constants into effective values |
| F02 | Contact overlap and normal | Detect a contact and measure overlap and push direction |
| F03 | Relative velocity at the contact point | Sliding velocity at the contact, including grain spin |

### 1.2 Contact elasticity (Hertz–Mindlin)

| ID | Formula | Purpose |
| --- | --- | --- |
| F04 | Hertz normal force | Normal force from overlap; contacts stiffen as pressed |
| F05 | Hertz contact radius, pressure and approach | Contact patch size and peak pressure under load |
| F06 | Mindlin tangential stiffness | Sideways stiffness of a contact before it slips |
| F07 | Cattaneo–Mindlin partial slip (**option**) | Exact micro-slip for one monotonic loading path |
| F08 | Spring-dashpot contact force | Normal and shear contact forces with damping |

### 1.3 Friction

| ID | Formula | Purpose |
| --- | --- | --- |
| F09 | Static friction spring with Coulomb limit | Per-contact friction with memory: stick, then slide at the Coulomb limit |
| F10 | Wall friction velocity clamp (**option**) | Memoryless, velocity-level friction at colliders |
| F11 | Rolling and twisting friction | Stop round grains rolling and spinning too freely |
| F22 | Friction angle to friction coefficient | Convert an angle of repose into a friction coefficient |

### 1.4 Damping and restitution

| ID | Formula | Purpose |
| --- | --- | --- |
| F12 | Damping from coefficient of restitution | Set contact damping from a bounciness value |
| F13 | Stiffness-proportional contact damping (**option**) | Original DEM contact damping with a direct factor |
| F14 | Global mass-proportional damping (**option**) | Settle piles quickly; also slows free fall |
| F21 | Restitution with plastic indentation | Bounciness that falls with impact speed |

### 1.5 Time integration and step size

| ID | Formula | Purpose |
| --- | --- | --- |
| F15 | Explicit time integration | Advance grain velocities and positions each step |
| F16 | Critical time step of a contact spring (**option**) | Stable step for constant-stiffness springs |
| F17 | Time step from elastic wave speed | Stable step for Hertz contacts |

### 1.6 Adhesion and impact

| ID | Formula | Purpose |
| --- | --- | --- |
| F18 | JKR adhesion | Pull-off force for sticky contacts (damp sand, semisolids) |
| F19 | Hertz impact overlap and duration | Peak overlap and collision time |
| F20 | Impact speed for onset of yield | Speed at which an impact starts to dent |

## 2. Granular continuum: MPM and grid sand

Sand treated as a continuous material instead of individual grains: the route for very large grain counts, and the reference for how sand should yield and flow.

### 2.1 MPM framework

| ID | Formula | Purpose |
| --- | --- | --- |
| F62 | MPM governing equations | Mass, momentum and deformation on particles with a background grid; shared by MPM sand and MPM tearing |
| F33 | Hencky elasticity | Elastic energy and stress from logarithmic strain |

### 2.2 Drucker–Prager plasticity

| ID | Formula | Purpose |
| --- | --- | --- |
| F28 | Drucker–Prager yield, plane-strain calibration | Pressure-dependent yield cone matched to Mohr–Coulomb |
| F29 | Non-associated flow rule | Let sand dilate less than its friction angle implies |
| F30 | Drucker–Prager cone width from friction angle | Cone parameter for the return mapping |
| F31 | Drucker–Prager return mapping | Project deformation back onto the friction cone each step |
| F32 | Friction hardening | Raise friction where sand has flowed |

### 2.3 Grid sand without elasticity

| ID | Formula | Purpose |
| --- | --- | --- |
| F23 | Mohr–Coulomb yield in von Mises form (**option**) | Decide whether sand in a grid cell holds or flows |
| F24 | Frictional stress of flowing sand (**option**) | Stress in yielding sand that resists sliding |
| F25 | Rigid-cell stress test (**option**) | Mark grid cells that stay rigid this step |

### 2.4 Rate-dependent granular flow

| ID | Formula | Purpose |
| --- | --- | --- |
| F26 | Inertial number and μ(I) friction | Friction that rises with shear rate in dense flow |
| F27 | μ(I) effective viscosity and yield | 3D μ(I) law as a viscous fluid with a yield stress |

## 3. Validation targets from soil mechanics

Not solver code: reference values that a plate-pressing test in the solver should reproduce.

| ID | Formula | Purpose |
| --- | --- | --- |
| F34 | Prandtl–Hill bearing capacity | Exact failure load of a strip footing on weightless soil |
| F35 | Drescher–Detournay reduced strength | Correct that load for low-dilatancy soil |
| F36 | Prandtl wedge angle | Angle of the wedge that forms under a rough plate |
| F37 | Terzaghi bearing capacity | Failure pressure including soil weight and footing shape |
| F38 | Bearing capacity factors | Factors for the Terzaghi formula from friction angle |

## 4. Tearing and fracture

Three ways to tear food and soft materials, from continuum damage on MPM particles, to breaking bonds between particles, to tearing thin sheets such as crusts and skins.

### 4.1 Continuum damage on MPM particles

| ID | Formula | Purpose |
| --- | --- | --- |
| F39 | Tension-only elastic degradation | Weaken damaged material in tension only, so pieces still collide |
| F40 | NACC yield surface | Crumbling through plastic hardening, without a damage field |
| F41 | Damage evolution with crack irreversibility | Grow damage against a geometric resistance; never heal |
| F42 | Fibre-aware crack driving force | Make cracks follow fibres (meat, string cheese) |
| F43 | Explicit damage update | Per-particle damage step, GPU-friendly |
| F44 | Implicit phase-field damage update (**option**) | Stable at large steps; needs a linear solve |
| F45 | Local damage rule (**option**) | Cheapest damage; mesh-dependent cracks |

### 4.2 Bonds between particles, and ductile stretching

| ID | Formula | Purpose |
| --- | --- | --- |
| F46 | Edge stress and bond breaking | Break a bond when smoothed stress along it is too high |
| F47 | Graph FEM plastic flow amount (**option**) | Rate-limited plastic flow with hardening |
| F48 | Strain split and deviatoric yield | Separate permanent from elastic stretch; test for yield |
| F49 | Plastic flow with plastic limit | Permanent stretch up to a limit before tearing |

### 4.3 Thin-sheet tearing on a mesh

| ID | Formula | Purpose |
| --- | --- | --- |
| F50 | Green strain and membrane stress | In-plane stretch and stress of a sheet |
| F51 | Discrete bending strain and sheet energy | Bending measure and total sheet energy |
| F52 | Separation strength | Mesh-independent test of where and along which plane a sheet tears |
| F53 | Bending fracture factor | Include bending stretch in the tearing test |
| F54 | Stretching plasticity rate and thinning | Permanent stretching that thins the sheet |
| F55 | Bending plasticity update | Permanent bending past a yield curvature |
| F56 | Fracture remeshing size | Refine the mesh where tears may start |

## 5. Liquids on a grid: Navier–Stokes with particle transfers

The grid route for phase 2 liquids: forces and incompressibility are solved on a grid, while particles carry the liquid and its velocity. PIC, FLIP and APIC are three selectable transfer schemes.

### 5.1 Equations and time step

| ID | Formula | Purpose |
| --- | --- | --- |
| F57 | Navier–Stokes momentum with variable viscosity | Equations of motion, including varying viscosity for mud and slime |
| F58 | Pressure projection | Make the velocity field incompressible each step |
| F59 | CFL time-step limit | Cap how far liquid moves per step, gravity included |

### 5.2 Free surface

| ID | Formula | Purpose |
| --- | --- | --- |
| F60 | Ghost-fluid free surface | Put the surface pressure at the true surface inside a cell |
| F61 | Surface tension pressure jump | Pull small drops and splashes toward smaller area |

### 5.3 Particle–grid transfer: PIC, FLIP or APIC

| ID | Formula | Purpose |
| --- | --- | --- |
| F63 | PIC and FLIP particle to grid | Weighted average of particle velocities onto the grid |
| F64 | PIC and FLIP grid to particle | Blend from PIC (smooth) to FLIP (splashy) |
| F65 | PIC fraction from viscosity | Set the PIC/FLIP blend from a physical viscosity |
| F66 | APIC particle to grid | Transfer keeping each particle's rotation and shear |
| F67 | APIC grid to particle | Return an affine velocity field to each particle |

## 6. Liquids on particles: SPH and adaptive resolution

The gridless route for phase 2 liquids: every force is computed between neighbouring particles, which can change size where detail is needed.

### 6.1 SPH core

| ID | Formula | Purpose |
| --- | --- | --- |
| F68 | SPH field interpolation | Evaluate any field from neighbouring particles |
| F69 | SPH pressure force for mixed sizes | Symmetric pressure force between particles of any size |
| F70 | SPH artificial viscosity | Damp relative motion; higher for mud and semisolids |

### 6.2 Adaptive resolution and colliders

| ID | Formula | Purpose |
| --- | --- | --- |
| F71 | Mixed-size neighbour criterion | Symmetric neighbour test for different particle sizes |
| F72 | Adaptive character field | Score where liquid needs fine particles |
| F73 | Split and merge rule | Split or merge particles from that score |
| F74 | Collider pre-subdivision criterion | Shrink collider triangles below particle size for fast tests |

## 7. Neighbour search

Shared by every particle method above: finding which particles are close enough to interact, quickly and in the same order every run.

| ID | Formula | Purpose |
| --- | --- | --- |
| F75 | Uniform grid cell index | Bin particles into cells about one grain diameter wide |
| F76 | Spatial hash (**option**) | Cell lookup for unbounded or very sparse domains |
| F77 | Morton Z-order key | Number cells so neighbours sit close in memory |
| F78 | Counting sort offsets | Place particles into cells without a full sort |
| F79 | Neighbour index delta encoding | Store neighbour lists compactly for huge particle counts |

## 8. Floating-point precision and determinism

Shared by every solver: why positions stay at 32-bit or more, why CPU and GPU results differ, and how to make sums repeat exactly.

| ID | Formula | Purpose |
| --- | --- | --- |
| F80 | Rounding error model and machine epsilon | Bound the error of one floating-point operation |
| F81 | Kahan compensated summation | Accurate long sums, such as many contacts on one grain |
| F82 | Fused multiply-add | One rounding for a multiply-add; a source of CPU/GPU differences |
| F83 | Floating-point step size | Position resolution of each number format |
| F84 | Reproducible summation by pre-rounding | Same sum in any order, for deterministic mode |

This file mirrors the *Formula catalogue* tab of the concept-sheet doc.
