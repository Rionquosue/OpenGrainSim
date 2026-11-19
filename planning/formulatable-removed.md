# Removed formulas

Formulas removed from [formulatable.md](formulatable.md) because another formula in the same role is more accurate or more efficient. Numbers are the original IDs before renumbering.

| Original ID | Formula | Reason |
| --- | --- | --- |
| F07 | Tangential to normal stiffness ratio | Derived quantity: the ratio follows directly from F04 and F06 for the same material, so computing it separately duplicates them. |
| F08 | Cattaneo-Mindlin partial slip (**restored as option**) | More accurate than the friction spring only for one monotonic loading path; general load histories need the Mindlin–Deresiewicz rules, which are not in the sources. The static friction spring handles every history at constant cost. |
| F10 | Incremental contact force | Superseded by the static friction spring: same incremental shear memory, but without rotating the stored stretch into the current contact plane, so it drifts as grains roll. Less accurate. |
| F11 | Coulomb friction cap | Merged into the static friction spring, whose slip step applies the same cap (now including cohesion). Keeping both would apply the cap twice. |
| F15 | Stiffness-proportional contact damping (**restored as option**) | Superseded by damping from the coefficient of restitution: same dashpot, but the coefficient comes from a physical, artist-meaningful restitution instead of an arbitrary factor. |
| F16 | Global mass-proportional damping (**restored as option**) | Less accurate: damps absolute velocity, so it also slows free fall and rigid motion. Contact damping from restitution removes energy only at contacts. |
| F18 | Critical time step of a contact spring (**restored as option**) | Superseded by the time step from elastic wave speed, which is derived for the Hertz contacts used here; the spring estimate assumes a constant linear stiffness. |
| F20 | Wall friction velocity clamp (**restored as option**) | Less accurate: a memoryless velocity-level Coulomb rule. The static friction spring applies the same limit to collider contacts and also keeps sticking history. |
| F26 | Mohr-Coulomb yield in von Mises form (**restored as option**) | Grid sand without elasticity (Zhu & Bridson 2005). Superseded by the Drucker–Prager return mapping with Hencky elasticity (Klár et al. 2016), which also stores stress and preserves volume; the source itself notes tall columns are inaccurate. |
| F27 | Frictional stress of flowing sand (**restored as option**) | Part of the same superseded grid-sand model as the Mohr–Coulomb von Mises form. |
| F28 | Rigid-cell stress test (**restored as option**) | Part of the same superseded grid-sand model; the return mapping handles static friction (its case 1) without separate rigid-body projection. |
| F32 | Drucker-Prager outer-cone constants | A third calibration of the same cone, and unverified (standard form, not transcribed). The plane-strain calibration is kept for validation and the friction-angle cone width for the MPM path. |
| F43 | Phase-field crack surface | Not computed at run time: its geometric term is already contained in the damage evolution equation as d − l0²Δd. |
| F45 | Implicit phase-field damage update (**restored as option**) | Less efficient: needs a sparse linear solve on the grid every step. The explicit damage update covers the same model (and isotropic materials as a special case) with a per-particle update. |
| F46 | Local damage rule (**restored as option**) | Less accurate: the source reports cracks that depend on mesh direction and resolution. |
| F52 | Graph FEM plastic flow amount (**restored as option**) | Less accurate: its flow fraction does not scale with the time step, so results change with step size. The stretching plasticity rate includes Δt. |
| F65 | PIC-FLIP velocity blend (**restored**, see below) | Superseded by APIC, which is more accurate (keeps rotation and shear, less noise than FLIP, less smoothing than PIC) at a small, fully parallel per-particle cost. |
| F77 | Spatial hash (**restored as option**) | Less efficient for the solver's bounded domains: hash collisions add extra distance checks, while the grid key has none. |

## Rewritten rather than removed

- **Static friction spring with Coulomb limit** (was F12): absorbs the Coulomb friction cap, with optional cohesion.
- **Bearing capacity factors** (was F42): Terzaghi's approximate $N_q$ and $N_c$ replaced by the exact Prandtl–Reissner values; chart-based $N_\gamma$ replaced by its closed-form approximation.
- **Uniform grid cell index** (was F76): the row-major cell key is dropped in favour of the Morton key, which costs the same and keeps nearby cells close in memory.

## Old to new numbering

| Old | New |
| --- | --- |
| F01 | F01 |
| F02 | F02 |
| F03 | F03 |
| F04 | F04 |
| F05 | F05 |
| F06 | F06 |
| F09 | F07 |
| F12 | F08 |
| F13 | F09 |
| F14 | F10 |
| F17 | F11 |
| F19 | F12 |
| F21 | F13 |
| F22 | F14 |
| F23 | F15 |
| F24 | F16 |
| F25 | F17 |
| F29 | F18 |
| F30 | F19 |
| F31 | F20 |
| F33 | F21 |
| F34 | F22 |
| F35 | F23 |
| F36 | F24 |
| F37 | F25 |
| F38 | F26 |
| F39 | F27 |
| F40 | F28 |
| F41 | F29 |
| F42 | F30 |
| F44 | F31 |
| F47 | F32 |
| F48 | F33 |
| F49 | F34 |
| F50 | F35 |
| F51 | F36 |
| F53 | F37 |
| F54 | F38 |
| F55 | F39 |
| F56 | F40 |
| F57 | F41 |
| F58 | F42 |
| F59 | F43 |
| F60 | F44 |
| F61 | F45 |
| F62 | F46 |
| F63 | F47 |
| F64 | F48 |
| F66 | F49 |
| F67 | F50 |
| F68 | F51 |
| F69 | F52 |
| F70 | F53 |
| F71 | F54 |
| F72 | F55 |
| F73 | F56 |
| F74 | F57 |
| F75 | F58 |
| F76 | F59 |
| F78 | F60 |
| F79 | F61 |
| F80 | F62 |
| F81 | F63 |
| F82 | F64 |
| F83 | F65 |
| F84 | F66 |
| F85 | F67 |

## Later changes

After reading Bridson's book in full: the inviscid Euler equations were replaced by the variable-viscosity Navier–Stokes form (more general; reduces to Euler at zero viscosity), and two formulas were added after the CFL limit (ghost-fluid free surface, surface tension), shifting later numbers:

| Before | After |
| --- | --- |
| F49 | F51 |
| F50 | F52 |
| F51 | F53 |
| F52 | F54 |
| F53 | F55 |
| F54 | F56 |
| F55 | F57 |
| F56 | F58 |
| F57 | F59 |
| F58 | F60 |
| F59 | F61 |
| F60 | F62 |
| F61 | F63 |
| F62 | F64 |
| F63 | F65 |
| F64 | F66 |
| F65 | F67 |
| F66 | F68 |
| F67 | F69 |

## Restored as solver options

PIC, FLIP and APIC are kept as three selectable transfer schemes rather than treated as redundant. The PIC-FLIP blend was restored (now covering pure PIC and pure FLIP as its end points), together with the PIC and FLIP particle-to-grid transfer and the viscosity-matched PIC fraction from Bridson, inserted before the APIC formulas:

| Before | After |
| --- | --- |
| F52 | F55 |
| F53 | F56 |
| F54 | F57 |
| F55 | F58 |
| F56 | F59 |
| F57 | F60 |
| F58 | F61 |
| F59 | F62 |
| F60 | F63 |
| F61 | F64 |
| F62 | F65 |
| F63 | F66 |
| F64 | F67 |
| F65 | F68 |
| F66 | F69 |
| F67 | F70 |
| F68 | F71 |
| F69 | F72 |

## Restored as alternative options

At the owner's request, every removed formula that is an alternative method rather than a duplicate was restored as a selectable option, placed after the formula it is an alternative to and marked *Alternative option* with its trade-off. Original IDs and their new numbers:

| Original ID | New ID |
| --- | --- |
| F08 | F07 |
| F20 | F10 |
| F15 | F13 |
| F16 | F14 |
| F18 | F16 |
| F26 | F23 |
| F27 | F24 |
| F28 | F25 |
| F45 | F44 |
| F46 | F45 |
| F52 | F47 |
| F77 | F76 |

Still removed as true duplicates or derived values: F07 (stiffness ratio, derived from F04 and F06 of the original numbering), F10 (incremental force, an older form of the friction spring), F11 (Coulomb cap, merged into the friction spring), F32 (third, unverified Drucker–Prager calibration) and F43 (crack-surface term, already inside the damage equation).
