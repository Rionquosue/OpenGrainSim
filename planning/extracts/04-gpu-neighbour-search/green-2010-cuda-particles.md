# Particle Simulation using CUDA

- **Author:** Simon Green, NVIDIA
- **Published:** NVIDIA CUDA SDK white paper, revision 1.3, May 2010 (updated for CUDA 3.1)
- **Source used:** NVIDIA's copy, read in full: <https://developer.download.nvidia.com/assets/cuda/files/particles.pdf>

## Summary

The reference implementation of a GPU particle system with collisions, from the CUDA SDK "particles" sample. It builds a uniform grid each frame by sorting particles by cell, then resolves collisions with a simple DEM spring–dashpot model by checking the 27 surrounding cells. It compares two ways to build the grid and explains why sorting wins.

## Key steps (algorithm; the paper has no numbered equations)

**Grid**

- Cells are the size of one particle (double its radius), so each particle can only touch particles in its own and the 26 neighbouring cells.
- Cell index from position: $c = \lfloor (x - x_{min}) / \Delta x \rfloor$, flattened to a single integer hash.

**Building the grid by sorting**

1. `calcHash`: one thread per particle computes its cell hash and stores the pair (hash, particle index).
2. Sort the pairs by hash with a fast radix sort (Le Grand, GPU Gems 3).
3. `reorderData`: copy positions and velocities into sorted order, so neighbours sit next to each other in memory.
4. `findCellStart`: one thread per sorted entry compares its hash with the previous entry's; where they differ, it records the start of a cell (and the end of the previous one).

**Building the grid with atomics** (the alternative): each particle atomically increments its cell's counter and writes itself into a fixed-size slot. Writes are scattered and serialised when many particles share a cell, and performance depends on how particles are distributed.

**Collisions:** each particle loops over the 27 cells, and for each overlapping neighbour applies a spring force (pushes apart), a dashpot (damping), a shear force (tangential friction) and an optional attraction.

**Results quoted:** 65,536 particles at about 175 frames per second on a GeForce GTX 280, and about 460 on a GTX 480. Binding the arrays to textures for cached reads improved speed by up to 45% on that hardware.

## Takeaways for the grain solver

- **This is the solver's neighbour search.** Sort by cell, then find each cell's start and end. In PyTorch the steps map to `argsort` (or `sort`) on the cell keys and `searchsorted` for the cell boundaries.
- **Reorder the particle arrays, not just the index.** Sorted data makes neighbour reads coalesced. In PyTorch, gathering the arrays into sorted order once per rebuild has the same effect.
- **Sorting is deterministic; atomics are not.** Besides speed, the sort-based build gives the same result every run, which the deterministic mode requires.
- **Grid generation is not the bottleneck;** the collision loop is. Optimisation effort belongs in the contact step.

## Cautions

- Hardware figures are from 2010 GPUs; the relative conclusions (sorting beats atomics) are what carry over.
- Texture-cache tricks are CUDA-specific and not available through PyTorch.
