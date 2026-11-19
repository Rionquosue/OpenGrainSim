# Real-time Adaptive Fluid Simulation with Complex Boundaries (journal version)

- **Authors:** Jian He, Xi Chen, Zhangye Wang, Chen Cao, He Yan, Qunsheng Peng (Zhejiang University)
- **Published:** *The Visual Computer* 26(4), 2010, pp. 243–252 (online 31 March 2010) · DOI [10.1007/s00371-010-0426-1](https://doi.org/10.1007/s00371-010-0426-1)
- **Source used:** read in full from a copy purchased by the repository owner. The PDF is not stored in this repository.
- **Related:** the 4-page conference version, [he-2009-adaptive-sph-complex-boundaries.md](he-2009-adaptive-sph-complex-boundaries.md). This extract covers only what the journal version adds or changes.

## What the journal version adds

- The SPH force equations for particles of different sizes.
- The geometric distance term split into surface distance and per-boundary distance.
- How a particle is split (into up to seven) and repositioned to avoid pressure spikes.
- The GPU data layout for neighbours and for splitting, including stream compaction.
- Hardware, more timings, comparisons with earlier work, and stated limitations.

## Key equations

**SPH field quantities.** Any quantity $A$ is a kernel-weighted sum over neighbours ($m$ mass, $\rho$ density, $W$ smoothing kernel with support radius $h$):

$$A(x) = \sum_j m_j \frac{A_j}{\rho_j}\, W(x - x_j, h)$$

**Pressure force between particles of different sizes.** The kernel gradient is averaged over the two support radii ($k$ stiffness, $\rho_0$ rest density):

$$F_i^P = -k\, m_i \sum_{j \ne i} m_j \left( \frac{\rho_i - \rho_0}{\rho_i^2} + \frac{\rho_j - \rho_0}{\rho_j^2} \right) \frac{\nabla_i \big(W_{h_i} + W_{h_j}\big)}{2}$$

**Artificial viscosity** ($\eta$ sets the amount; $\Pi_{ij}$ is the standard artificial-viscosity term from the paper's reference [19]):

$$F_i^{Vis} = -\eta\, m_i \sum_{j \ne i} m_j\, \Pi_{ij}\, \frac{\nabla_i \big(W_{h_i} + W_{h_j}\big)}{2}$$

**Distance term, per boundary.** Particle $i$'s distance $d_i$ is the smallest of its distance to the liquid surface $d_{s_i}$ and its distances to each boundary $d_{b_{ij}}$; $d_{max}$ is the largest $d_i$ in the current step:

$$d_i = \min\{d_{s_i},\, d_{b_{i1}},\, d_{b_{i2}}, \ldots\}$$

$$c_f(x) = \frac{\alpha\, \dfrac{d_i}{d_{max}} + \beta\left(1 - \dfrac{f_i}{f_{max}}\right) + \gamma\, \dfrac{N_i}{N_{max}}}{\alpha + \beta + \gamma}$$

The split/keep/merge rule is unchanged from the conference paper (split if $c_f < T$, merge if $c_f \ge 2T$).

**Particle radius.** The text says the new radius "can be computed according to (6)", which is the character field itself; no separate formula maps $c_f$ to a radius. As in the conference version, how mass and velocity are shared between child particles, and how merging picks partners, are not specified.

## Splitting and GPU layout

- **Split pattern:** one particle splits into up to seven (Fig. 4 of the paper). Placed naively, the children overlap and create high local pressure, so they are moved to positions spread out enough to avoid it. The paper reports this noticeably improves stability; it does not give the placement rule.
- **Memory:** the GPU pre-allocates `NmaxSplit × NumParticles` slots, so each particle owns enough space for all its possible children. After splitting, a stream-compaction pass removes the empty slots (the figure illustrates a four-way split).
- **Neighbours:** particles are hashed by grid cell as `[cellIndex, particleIndex]` pairs, sorted with a radix sort, then each particle's neighbour list is written to a 3D texture: (x, y) identifies the particle, z the neighbour slot, and slot 0 holds the neighbour count.
- **Step order:** build neighbour texture → compute forces → split/merge → voxelise and detect collisions → leap-frog integration.
- **Collision precise phase:** for particles inside a box–voxel overlap, the exact test is "similar to a ray reflecting algorithm"; the boundary force follows Müller et al. 2003. Voxelisation is conservative: uncertain voxels are marked occupied.

## Results

- **Hardware:** Intel Core 2 Quad Q9550, NVIDIA GeForce GTX 280, Cg and CUDA.
- **Breaking wave:** about 30,000 particles at about 68 frames per second.
- **Armadillo (300,000 triangles):** 24,000–65,000 particles; 13.1–57.0 frames per second depending on particle count; 28.3 average, falling to about 13 with plain bounding-box collision detection.
- **Brook (97,600 triangles):** 150,000 particles at 26.9 frames per second.
- **Per step for 20,000 particles:** 0.0152 s, against 0.079 s (Kipfer et al.) and 0.071 s adaptive / 0.21 s non-adaptive (Adams et al. 2008).

The per-step timing table matches the conference version, except that the "Integration" and "DoCollision" rows for the floating duck appear swapped between the two versions (0.04 and 1.82 ms).

## Limitations stated by the authors

- Adaptation does not pay off below about 10,000 particles; its advantage grows with particle count.
- Surface extraction (marching cubes) and rendering (POV-Ray) were offline; only the particle simulation is real-time.
- The boundary subdivision "is not physically accurate" but gives convincing, stable results.

## Takeaways for the grain solver

- **Pre-allocate, then compact.** Reserving fixed slots per particle and compacting afterwards is how particle counts can change on a GPU without dynamic allocation. If the phase 2 liquid adapts, this is the pattern, and compaction must be order-preserving to keep results deterministic.
- **Symmetric forces for mixed sizes.** Averaging the kernel gradient over both support radii keeps forces equal and opposite between different-sized particles, which matters for momentum conservation and for the half-shell neighbour search.
- **Distance to each boundary separately** is what the solver's per-collider signed distance fields provide directly: take the minimum over colliders and the surface.
- **Only worth it at scale.** The authors' own break-even of about 10,000 particles suggests adaptivity is a phase 3 optimisation for large liquid scenes, not a version 1 feature.

## Cautions

- The split placement rule, the radius mapping and the merge procedure are still not given; implementing them means designing them, for example from Adams et al. 2008 ("Adaptively sampled particle fluids"), which both versions cite.
