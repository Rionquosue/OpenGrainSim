# Blender Manual: Fluid Domain Settings (Mantaflow liquid)

- **Publisher:** Blender Foundation, Blender Manual (5.0)
- **Link:** <https://docs.blender.org/manual/en/5.0/physics/fluid/type/domain/settings.html>
- **Source used:** the manual page, read in full.

## Summary

Blender's built-in liquid (Mantaflow) is a FLIP/APIC solver on a grid. This page documents the settings that matter for comparing it with a custom particle solver: simulation method, sub-stepping, particle sampling, and the narrow band of particles kept near the surface.

## Key settings (no equations on the page; definitions quoted or paraphrased)

**Simulation method**

- **FLIP:** "Produces a very splashy simulation with lots of particles dispersed in the air."
- **APIC:** "Produces a very energetic but also more stable simulation. Vortices within the liquid will be preserved better than with FLIP."
- **FLIP Ratio** (FLIP only): how much FLIP velocity is used when updating particles. 1.0 is pure FLIP, for large volumes and chaotic splashes; lower values give calmer, small-scale liquid.

**Time stepping**

- **CFL number:** maximum travel per time step, in grid cells. If exceeded, the step is subdivided. Larger values mean fewer steps and less accuracy at high speed.
- **Timesteps Maximum / Minimum:** limits on sub-steps per frame. The manual advises raising the maximum when lowering the CFL number.

**Particles**

- **Particle Radius:** in grid-cell units; how much area each particle marks as liquid. Raise it if liquid seems to vanish, lower it if volume is gained.
- **Sampling** and **Randomness:** how many particles are sampled per cell (resampled every step) and how randomly they are placed in inflows.
- **Particles Maximum / Minimum:** per-cell limits.
- **Narrow Band Width:** particles are kept only in a band (in cells) near the surface; the interior is grid-only. The manual recommends keeping it thin, since the surface holds the detail.
- **Fractional Obstacles:** sub-cell obstacle handling, reducing stair-stepping on slanted colliders.

## Takeaways for the grain solver

- **Blender already covers bulk liquid.** The phase 2 liquid should not try to replace Mantaflow for large water bodies. Its value is liquids and semisolids that interact with grains and torn materials, which Mantaflow cannot do.
- **Adopt the same artist vocabulary:** CFL-style substep control, particle radius, and a FLIP/APIC choice will feel familiar to Blender users.
- **Narrow band is a strong optimisation** for any phase 2 grid liquid: simulate particles only near the surface.
- **Fractional obstacles** solve the same problem as the solver's SDF colliders: smooth contact with slanted geometry.

## Cautions

- Describes Blender 5.0; settings and wording can change between releases. Check the manual for the Blender version in use (5.1).
