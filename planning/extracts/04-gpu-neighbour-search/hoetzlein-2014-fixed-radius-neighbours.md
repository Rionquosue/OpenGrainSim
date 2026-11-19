# Fast Fixed-Radius Nearest Neighbors: Interactive Million-Particle Fluids

- **Author:** Rama C. Hoetzlein (NVIDIA)
- **Published:** GPU Technology Conference (GTC) 2014, Santa Clara · talk slides
- **Source used:** the author's slides, read in full: <https://ramakarl.com/pdfs/2014_Hoetzlein_FastFixedRadius_Neighbors.pdf>. Code: Fluids v.3 (open source), linked from <https://ramakarl.com/?p=114>.

## Summary

Hoetzlein replaced the radix sort used in Green's 2010 CUDA particles with a counting sort, which builds the neighbour grid in fewer passes. Together with other optimisations in Fluids v.3, this took an SPH fluid from 16,384 particles at 32 fps (Fluids v.2, 2009) to 193,487 particles at 32 fps on the same GeForce GTX 460M, about 11 times faster. Larger demos reached 4,194,304 particles at 4.2 fps.

## Key steps (algorithm)

The slides compare the two pipelines side by side:

| Step | CUDA Particles (radix sort) | Fluids v.3 (counting sort) |
| --- | --- | --- |
| Assign to cells | `calcHash` | `InsertParticles`: atomic add on each bin's count, which also returns the particle's index within its bin |
| Sort | `thrust::sort_by_key` (radix sort) | `PrefixSum`: exclusive scan of bin counts gives each bin's start offset |
| Place | reorder data | `CountingSort`: write each particle to `offset[bin] + index_in_bin` |
| Query | loop over 27 neighbour cells | loop over 27 neighbour cells |

**Counting sort cost.** One pass to count, one scan over the bins, and one pass to scatter: $O(N + B)$ for $N$ particles and $B$ bins, compared with several passes of a radix sort over 32-bit keys.

## Takeaways for the grain solver

- **Counting sort maps onto PyTorch directly:** `torch.bincount` on cell keys gives bin counts, `torch.cumsum` gives offsets, and a stable sort by key gives the placement. This avoids sorting full 64-bit keys when the number of cells is bounded.
- **Watch determinism.** Hoetzlein's in-bin index comes from atomic adds, so the order inside a bin can change between runs. For bit-identical results, order particles inside each bin by their original index (a stable sort by key does this).
- **Fixed-radius search is enough for grains.** All grains have similar radii, so one cell size works; very mixed radii (beyond about 4:1, as the concept sheet notes) need a different structure.

## Cautions

- Slides, not a paper: no derivations, and the performance figures are from 2013–2014 hardware.
