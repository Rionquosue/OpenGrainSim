# Optimized Spatial Hashing for Collision Detection of Deformable Objects

- **Authors:** Matthias Teschner, Bruno Heidelberger, Matthias Müller, Danat Pomeranets, Markus Gross
- **Published:** Vision, Modeling, and Visualization (VMV) 2003, Munich, pp. 47–54
- **Source used:** the authors' copy, read in full: <https://cgweb.informatik.uni-freiburg.de/publications/2003_VMV_collisionDetectionHashing.pdf>

## Summary

A collision-detection method for deformable tetrahedral meshes that uses a hash table instead of a full 3D grid, so memory does not depend on the size of the scene. Space is divided into cells, each cell is hashed into a 1D table, and only occupied entries are checked. The paper tunes the hash function, table size and cell size, and detects collisions and self-collisions for up to about 20,000 tetrahedra in real time.

## Key equations

**Cell coordinates:** each coordinate is divided by the cell size $l$ and rounded down: $(i, j, k) = \big(\lfloor x/l \rfloor, \lfloor y/l \rfloor, \lfloor z/l \rfloor\big)$.

**Hash function** ($n$ is the hash table size; $\oplus$ is bitwise XOR):

$$\text{hash}(i, j, k) = \big( i\,p_1 \oplus j\,p_2 \oplus k\,p_3 \big) \bmod n$$

$$p_1 = 73856093, \quad p_2 = 19349663, \quad p_3 = 83492791$$

**Tuning results**

- **Cell size:** about the average edge length of the mesh gave the best performance.
- **Table size:** larger tables reduce collisions between unrelated cells; prime table sizes worked best (tests used 99, 199, 299, ...).
- **No table clearing:** each entry stores a timestamp, so stale entries from earlier steps are ignored instead of clearing the whole table every step.

## Takeaways for the grain solver

- **The grain solver's domain is bounded,** so it can use a plain cell key (row-major index inside the domain box) with no hash collisions. Spatial hashing becomes useful if domains are very large and sparse, for example a tall pour into a wide domain.
- **If hashing is ever needed,** these primes are the standard choice, and the timestamp trick avoids clearing large tables on the GPU.
- **Cell size near the interaction distance** is the same rule as the concept sheet's "cell = largest diameter plus margin".

## Cautions

- Hash collisions put unrelated cells in the same bucket; every candidate pair still needs a real distance check.
- Integer multiplication can overflow in 32-bit; use 64-bit keys in PyTorch.
