# Fast Reproducible Floating-Point Summation

- **Authors:** James Demmel, Hong Diep Nguyen
- **Published:** 21st IEEE Symposium on Computer Arithmetic (ARITH 2013), Austin, pp. 163–172
- **Links:** ReproBLAS talk slides (SC13): <https://bebop.cs.berkeley.edu/reproblas/docs/talks/SC13.pdf> · technical report on the follow-up library: <https://www2.eecs.berkeley.edu/Pubs/TechRpts/2015/EECS-2015-229.html>
- **Source used:** ⚠️ **Not read from the paper or slides.** The Berkeley host failed its TLS certificate check, and the paper is not open access. This extract is written from knowledge of the method, checked against abstract-level search results (order-independent summation; indexed floating-point format; Rump-style error-free transformations; the basis of ReproBLAS). **Verify details against the paper before implementing.**

## Summary

Parallel sums give different answers depending on how the work is split, because floating-point addition is not associative. The paper gives a summation method whose result is bit-identical regardless of the order of the terms or the number of processors, at a cost of a few extra operations per term and one extra reduction. It is the basis of the ReproBLAS library (reproducible sum, dot product and 2-norm).

## Key idea (as recalled)

1. **Find a common boundary.** First compute $M = \max_i \lvert x_i\rvert$ (a max is order-independent and exact). From $M$ and the number of terms $n$, choose a power of two $2^{k}$ large enough that $n$ terms rounded to multiples of a fixed small unit cannot overflow it.

2. **Pre-round every term to the same grid.** Split each term with an error-free transformation (Rump's extraction), where $\sigma$ is a power of two set by $M$ and $n$:

$$q_i = \operatorname{fl}\big((x_i + \sigma) - \sigma\big), \qquad r_i = x_i - q_i$$

Every $q_i$ is an exact multiple of the same unit, and $r_i$ is the exact remainder.

3. **Sum the rounded parts exactly.** Adding numbers that are all multiples of the same unit and bounded in size involves no rounding, so $\sum_i q_i$ is the same in any order.

4. **Repeat on the remainders** ("folds", typically 2 or 3) to recover accuracy: each fold sums the $r_i$ of the previous one on a finer grid.

The result is reproducible bit-for-bit, and with $K$ folds its accuracy is comparable to or better than ordinary summation. The Berkeley technical report quotes an arithmetic cost of $7n$ floating-point additions to sum $n$ terms with a 6-word accumulator.

## Takeaways for the grain solver

- **A second route to determinism.** The concept sheet gets reproducible sums by sorting contacts and summing each grain's run in a fixed order. Pre-rounding to a common grid gives the same guarantee without the sort, and in principle also across different numbers of threads.
- **It can be written with whole-array operations.** The extraction step is two additions per term, which suits PyTorch.
- **Use if sorting proves costly.** For version 1, the sort-and-segment-sum approach is simpler; keep this as the alternative.

## Cautions

- Recalled, not transcribed. The exact choice of $\sigma$, the number of folds and the error bounds must come from the paper.
- Reproducible across orderings on the same number format; it does not make CPU and GPU agree if their per-term products differ (for example because of FMA).
