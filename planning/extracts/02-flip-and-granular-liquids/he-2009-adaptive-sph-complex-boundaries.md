# A New Adaptive Model for Real-time Fluid Simulation with Complex Boundaries

- **Authors:** Jian He, Xi Chen, Zhangye Wang, Ke Yan, Chen Cao, Qunsheng Peng (State Key Lab of CAD&CG, Zhejiang University)
- **Published:** 11th IEEE International Conference on Computer-Aided Design and Computer Graphics (CAD/Graphics 2009), Huangshan, pp. 45–48 · DOI [10.1109/CADCG.2009.5246934](https://doi.org/10.1109/CADCG.2009.5246934)
- **Expanded journal version:** "Real-time adaptive fluid simulation with complex boundaries", *The Visual Computer* 26(4), 2010, pp. 243–252 · DOI [10.1007/s00371-010-0426-1](https://doi.org/10.1007/s00371-010-0426-1). Its additions are in [he-2010-adaptive-fluid-complex-boundaries-journal.md](he-2010-adaptive-fluid-complex-boundaries-journal.md).
- **Source used:** the conference paper, read in full from a copy purchased by the repository owner. The PDF is not stored in this repository.

## Summary

A GPU smoothed particle hydrodynamics (SPH) liquid that uses particles of different sizes: small particles at the surface, near boundaries, in high-pressure regions and in isolated droplets; large particles deep inside calm liquid. Particles split or merge each step according to a per-particle score (the "character field"). Collisions with detailed scenery are found in two steps: the scene is voxelised once on the GPU, then particles are tested against occupied voxels in three phases. Their brook scene runs 150,000 particles on average against 97,600 triangles at 26.9 frames per second.

## Key equations

**Neighbours with mixed sizes.** Each particle $p_i$ has its own radius $r_i$ and support radius $h_i$. Particle $p_j$ is a neighbour of $p_i$ if

$$\lVert x_i - x_j\rVert \le 2\max(h_i, h_j)$$

The pressure force on $p_i$ is the average of the forces from the "gathering" and "shooting" (scattering) forms of SPH, which keeps forces between different-sized particles balanced.

**Character field.** For particle $i$: $d_i$ is its distance to the liquid surface or the nearest boundary, $f_i^p$ its local pressure, $N_i$ its neighbour count, and $\alpha, \beta, \gamma$ user weights:

$$c_f(x) = \frac{\alpha\,\dfrac{d_i}{d_{max}} + \beta\left(1 - \dfrac{f_i^p}{f_{max}^p}\right) + \gamma\,\dfrac{N_i}{N_{max}}}{\alpha + \beta + \gamma}$$

A low value means the particle needs detail: near the surface or a boundary (small $d_i$), under high pressure (large $f_i^p$), or isolated, like a droplet (small $N_i$).

**Split or merge rule** with a user threshold $T$:

$$\text{action} = \begin{cases} \text{split} & c_f(x) < T \\ \text{merge} & c_f(x) \ge 2T \\ \text{keep} & \text{otherwise} \end{cases}$$

All particles start at the smallest allowed radius. The paper says each particle's new radius comes from "Eq. (3)", but no Eq. (3) appears in the conference paper; the journal version may contain it.

**Boundary pre-subdivision.** Before simulating, collider triangles are subdivided until every edge is shorter than the particle radius, so a bounding-box test is accurate enough:

$$\max\big(\lVert a_j - b_j\rVert,\ \lVert c_j - b_j\rVert,\ \lVert a_j - c_j\rVert\big) < r_i$$

**Collision detection, three phases** (after voxelising the scene into a 256³ grid by GPU rasterisation of thin slices):

1. Build the axis-aligned bounding box of each particle's path from its current to its predicted position.
2. Test those boxes against occupied voxels, in parallel.
3. Only where a box meets an occupied voxel, run the precise particle–triangle test.

**Integration:** explicit leap-frog.

**Reported timings** (ms per frame; adaptive versus fixed-size GPU SPH):

| Scene | Adaptive | Fixed-size | Frames per second |
| --- | --- | --- | --- |
| Floating duck | 22.73 | 32.26 | 44.0 vs 31.0 |
| Armadillo | 37.0 | 90.9 | 28.3 vs 13.0 |
| Brook | 37.17 | 104.93 | 26.9 vs 9.53 |

Re-sampling (split and merge) costs 6.6–10.7 ms per frame, but saves more than that in neighbour search and forces. Collision handling drops from 15–20 ms to under 3 ms.

## Takeaways for the grain solver

- **The character field is a ready-made "where does precision matter" score.** It combines distance to surfaces and boundaries, pressure and isolation. The concept sheet's precise set (particles touching a collider or another object, solved in 64-bit) could use the same kind of score, with a threshold, instead of a simple contact test.
- **Variable particle size fits the phase 2 liquid,** not the grains: grains have physical sizes and cannot split. For liquids it is the particle counterpart of Blender's narrow-band FLIP, cutting particle counts in calm, deep regions.
- **Cheap broad phase for colliders.** "Occupied voxel" bits plus a swept bounding box per particle reject almost all particle–triangle tests. The concept sheet's signed distance field already answers "how far from a surface" for every voxel, so the same grid can serve both purposes.
- **Swept boxes catch fast particles.** Testing the path from current to predicted position, not just the end point, is the standard guard against particles tunnelling through thin colliders, a risk the concept sheet lists for fast-moving objects.
- **Mixed-size neighbour rule:** if mixed grain sizes are allowed, using $2\max(h_i, h_j)$ (or the sum of radii for contacts) and building cells from the largest size matches the concept sheet's plan.

## Cautions

- A 4-page conference paper: the split and merge mechanics (how mass and velocity are shared between child particles, and the radius formula) are not given. The 2010 journal version adds the split pattern and GPU layout, but not these either.
- Splitting and merging change the particle count every step, which complicates the solver's changed-index output (indices must stay stable). Liquids would need persistent IDs separate from array positions.
- The voxel grid is fixed at 256³ over the simulation space, so very large domains lose collision resolution.
