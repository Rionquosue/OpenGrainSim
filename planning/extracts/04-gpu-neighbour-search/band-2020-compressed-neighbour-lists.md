# Compressed Neighbour Lists for SPH

- **Authors:** Stefan Band, Christoph Gissler, Matthias Teschner
- **Published:** Computer Graphics Forum 39(1), 2020, pp. 531–542 · DOI [10.1111/cgf.13890](https://doi.org/10.1111/cgf.13890)
- **Source used:** the authors' copy, read in full: <https://cg.informatik.uni-freiburg.de/publications/2019_CGF_CompressedNeighbors.pdf>

## Summary

Iterative particle solvers reuse each particle's neighbour list many times per step, so they store it, and for billions of particles that storage dominates memory. The paper compresses neighbour lists by storing small differences between sorted indices in a variable number of bytes. It saves up to 87% of the memory, and its neighbour search beats compact hashing in both speed and memory. It ran 1.3 billion particles on a 24-core PC with 172 GB of RAM.

## Key ideas

**Sort first, so neighbours have nearby indices.** Particles are sorted along a space-filling curve; a particle's neighbours then have indices close to each other.

**Store differences, not indices.** A sorted neighbour list $j_1 < j_2 < \ldots$ is stored as differences

$$\Delta_k = j_k - j_{k-1}$$

which are mostly small (+1 and +2 dominate inside a cell cluster, with a few large jumps between clusters).

**Variable-byte encoding (based on Stream VByte).** Control bits say how many data bytes each value uses, and control bytes are stored in a separate stream from data bytes so decoding can run ahead (good for SIMD). The paper's change: **the values 1 and 2 are encoded in the control bits alone, using no data byte at all**, which suits the many +1 and +2 differences.

**Results quoted:** memory savings up to 87%; a DFSPH step for 0.2 million particles in under 40 ms on a 12-core PC; over 7 billion particles with MPI on 112 cores and 880 GB.

## Takeaways for the grain solver

- **Contact lists are the solver's largest memory cost** after positions. The concept sheet keeps them per frame; at 500k grains with about 6–12 contacts each, they are millions of index pairs.
- **Sorted particles make differences small.** Even without full compression, storing contact partner indices as 16-bit differences from the particle's own sorted index would roughly halve contact-list memory, at the cost of a decode step.
- **Not needed for version 1.** Compression pays off at hundreds of millions of particles; the grain solver targets up to about 500k. Keep it as a phase 3 option for very large domains.

## Cautions

- Byte-level variable-length decoding is awkward in PyTorch's whole-array style; it would need a custom kernel to be fast.
