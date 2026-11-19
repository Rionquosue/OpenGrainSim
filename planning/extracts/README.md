# Reference extracts

One file per reference in the concept sheet's reading list, each with a summary, the key equations, takeaways for the grain solver, and cautions. These are personal notes for later implementation work. The source papers are never stored here; the repository's `.gitignore` blocks PDFs.

**Source column:**

- **Read** — written from the full text of the paper or page (link in the file).
- **⚠️ Recalled** — the source could not be downloaded (paywall, blocked host or bad TLS certificate). Written from knowledge of the work, checked against summaries where noted. Verify those equations against the original before implementing.

Equations were transcribed from text extracted from PDFs, which scrambles fraction and subscript layout. Treat every equation as needing a check against the original before it goes into code.

## 1. Tearing of bread, meat and soft materials

| Extract | Source |
| --- | --- |
| [CD-MPM (Wolper et al. 2019)](01-tearing/wolper-2019-cd-mpm.md) | Read |
| [AnisoMPM (Wolper et al. 2020)](01-tearing/wolper-2020-anisompm.md) | Read |
| [Graph-based FEM fracture (Mandal et al. 2023)](01-tearing/mandal-2023-graph-fem-fracture.md) | Read |
| [Ductile fracture (O'Brien et al. 2002)](01-tearing/obrien-2002-ductile-fracture.md) | Read |
| [Adaptive tearing of thin sheets (Pfaff et al. 2014)](01-tearing/pfaff-2014-adaptive-tearing-thin-sheets.md) | Read |

## 2. FLIP liquids and grain-based liquids

| Extract | Source |
| --- | --- |
| [Animating sand as a fluid (Zhu, Bridson 2005)](02-flip-and-granular-liquids/zhu-bridson-2005-sand-as-fluid.md) | Read |
| [Fluid Simulation for Computer Graphics (Bridson 2015)](02-flip-and-granular-liquids/bridson-2015-fluid-simulation-book.md) | Read: equations, time steps, pressure, particle methods, water and viscosity sections (copy not stored here) |
| [APIC (Jiang et al. 2015)](02-flip-and-granular-liquids/jiang-2015-apic.md) | Read |
| [Drucker–Prager sand (Klár et al. 2016)](02-flip-and-granular-liquids/klar-2016-drucker-prager-sand.md) | Read |
| [Blender Manual: fluid domain settings](02-flip-and-granular-liquids/blender-manual-fluid-domain-settings.md) | Read |
| [Adaptive SPH with complex boundaries (He et al. 2009)](02-flip-and-granular-liquids/he-2009-adaptive-sph-complex-boundaries.md) | Read (purchased copy, not stored here) |
| [Adaptive fluid with complex boundaries, journal version (He et al. 2010)](02-flip-and-granular-liquids/he-2010-adaptive-fluid-complex-boundaries-journal.md) | Read (purchased copy, not stored here) |

## 3. Bearing, shearing and contact forces

| Extract | Source |
| --- | --- |
| [Discrete element method, the original (Cundall, Strack 1979)](03-bearing-shear-contact/cundall-strack-1979-dem.md) | Read (copy not stored here) |
| [TinyDEM: grain contacts with sliding, rolling and twisting friction (Vetter 2025)](03-bearing-shear-contact/vetter-2025-tinydem.md) | Read |
| [Contact Mechanics (Johnson 1985)](03-bearing-shear-contact/johnson-1985-contact-mechanics.md) | Read: Hertz, tangential loading, adhesion and impact sections (copy not stored here) |
| [μ(I) rheology (Jop et al. 2006)](03-bearing-shear-contact/jop-2006-mu-i-rheology.md) | Read |
| [Non-associated Drucker–Prager model and strip footings (Hamlaoui et al. 2015)](03-bearing-shear-contact/hamlaoui-2015-non-associated-drucker-prager-footings.md) | Read (copy not stored here); replaces Drucker & Prager 1952 |
| [Terzaghi bearing capacity (1943)](03-bearing-shear-contact/terzaghi-1943-bearing-capacity.md) | Wikipedia summary read |

## 4. GPU neighbour search and particle data structures

| Extract | Source |
| --- | --- |
| [Particle Simulation using CUDA (Green 2010)](04-gpu-neighbour-search/green-2010-cuda-particles.md) | Read |
| [Fast fixed-radius neighbours (Hoetzlein 2014)](04-gpu-neighbour-search/hoetzlein-2014-fixed-radius-neighbours.md) | Read (slides) |
| [Optimized spatial hashing (Teschner et al. 2003)](04-gpu-neighbour-search/teschner-2003-spatial-hashing.md) | Read |
| [Parallel SPH on multi-core CPUs (Ihmsen et al. 2011)](04-gpu-neighbour-search/ihmsen-2011-parallel-sph-cpu.md) | Read |
| [Compressed neighbour lists (Band et al. 2020)](04-gpu-neighbour-search/band-2020-compressed-neighbour-lists.md) | Read |

## 5. Floating-point precision and reproducibility

| Extract | Source |
| --- | --- |
| [Floating-point arithmetic (Goldberg 1991)](05-floating-point-precision/goldberg-1991-floating-point.md) | Read |
| [Floating point on NVIDIA GPUs (Whitehead, Fit-Florea)](05-floating-point-precision/nvidia-floating-point-ieee754-gpus.md) | Read |
| [FP8 formats (Micikevicius et al. 2022)](05-floating-point-precision/micikevicius-2022-fp8-formats.md) | Read |
| [Reproducible summation (Demmel, Nguyen 2013)](05-floating-point-precision/demmel-nguyen-2013-reproducible-summation.md) | ⚠️ Recalled |
| [torch.use_deterministic_algorithms](05-floating-point-precision/pytorch-deterministic-algorithms.md) | Read |
