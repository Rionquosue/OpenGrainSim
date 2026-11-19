# A Parallel SPH Implementation on Multi-Core CPUs

- **Authors:** Markus Ihmsen, Nadir Akinci, Markus Becker, Matthias Teschner
- **Published:** Computer Graphics Forum 30(1), 2011, pp. 99–112 · Eurographics library record: <https://diglib.eg.org/handle/10.1111/v30i1pp099-112>
- **Source used:** the authors' copy, read in full: <https://cg.informatik.uni-freiburg.de/publications/2011_CGF_dataStructuresSPH.pdf>

## Summary

The neighbour search dominates the cost of particle fluids. The paper compares five uniform-grid variants on multi-core CPUs and proposes two improvements: **Z-index sort** and **compact hashing**. Both are the fastest of the five and perform about equally; compact hashing suits large simulations in unbounded domains. It enabled several million particles on a 24-core machine.

## Key ideas

**Z-curve cell order.** Cells are numbered along a space-filling Z-curve (Morton order) by interleaving the bits of the integer cell coordinates:

$$z(i, j, k) = \ldots\, k_1 j_1 i_1\, k_0 j_0 i_0 \quad \text{(bits of } i, j, k \text{ interleaved)}$$

Nearby cells get nearby numbers, so particles sorted by $z$ are close in memory as well as in space.

**Sorting cheaply**

- Store a small *handle* per particle: (cell index, particle index). Sorting handles is far cheaper than sorting particle data.
- Between steps particles barely move, so the handle array is almost sorted, and **insertion sort** beats a parallel sort for it.
- Reorder the full particle arrays only **every 100th step** to restore memory locality.

**Compact hashing**

- A hash table of size $m$ (about twice the number of particles worked well for SPH) points into a compact list of only the $n$ cells that are actually used.
- Neighbour queries traverse only used cells, and memory scales with particles, not with domain volume.

## Takeaways for the grain solver

- **Morton order for cell keys:** using a Z-curve key instead of a row-major key improves memory locality of the sorted arrays at almost no cost. It is a few bit operations in PyTorch.
- **Cheap per-step sorting:** the concept sheet rebuilds neighbours once per frame. Because order changes little between frames, a full reorder of the particle arrays can be done less often (every few frames) while the per-frame rebuild sorts only keys.
- **The CPU fallback** will run through PyTorch's CPU backend; this paper's results suggest it should still use sorted, locality-preserving layouts rather than per-cell linked lists.

## Cautions

- Results are for CPUs; on GPUs the trade-offs between insertion sort and parallel sort differ (insertion sort is sequential).
