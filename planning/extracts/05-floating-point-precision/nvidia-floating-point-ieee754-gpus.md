# Precision and Performance: Floating Point and IEEE 754 Compliance for NVIDIA GPUs

- **Authors:** Nathan Whitehead, Alex Fit-Florea (NVIDIA)
- **Published:** NVIDIA CUDA documentation (white paper, maintained with each CUDA release)
- **Source used:** the CUDA 13.0.3 archived version, read in full: <https://docs.nvidia.com/cuda/archive/13.0.3/floating-point/index.html>

## Summary

Explains why the same calculation can give different results on a CPU and a GPU even when both follow IEEE 754. The main causes are fused multiply-add (FMA), the order in which a parallel reduction adds numbers, compiler options that trade accuracy for speed, and the old x87 CPU instructions that compute with extra precision.

## Key equations and facts

**Fused multiply-add rounds once.** FMA, part of IEEE 754 since 2008 and built into NVIDIA GPUs, computes

$$\operatorname{rn}(X \times Y + Z)$$

with one rounding, while a separate multiply and add round twice: $\operatorname{rn}(\operatorname{rn}(X\times Y) + Z)$.

**Worked example.** For $x = 1.0008$ in four-digit decimal arithmetic, the true $x^2 - 1 = 1.60064\times10^{-4}$. Separate operations give $1.6000\times10^{-4}$; FMA gives $1.6006\times10^{-4}$, the closest four-digit value. In a binary single-precision example, separate operations lose all significant bits and return 0, while FMA is exact.

**Three dot products, three answers.** For $\sum a_i b_i$:

- **Serial:** add products one after another.
- **FMA:** accumulate with FMA at each step.
- **Parallel (tree):** $((a_1 b_1 + a_2 b_2) + (a_3 b_3 + a_4 b_4))$, as a GPU reduction does.

All three are IEEE-compliant and give different results. Results can also change with the number of threads or blocks used.

**Compiler flags (single precision):**

| Mode | `-ftz` | `-prec-div` | `-prec-sqrt` |
| --- | --- | --- | --- |
| IEEE 754 (default) | false | true | true |
| Fast | true | false | false |

`-ftz=true` flushes tiny (subnormal) values to zero. Whether FMA is used also depends on how the code is compiled (`--fmad`).

**CPU differences:** x87 instructions can keep 80-bit intermediates, while SSE uses true 32/64-bit, so even two CPUs can disagree.

## Takeaways for the grain solver

- **CPU and GPU results will differ in the last bits; this is expected, not a bug.** The concept sheet's CPU-against-GPU test checks results within a tolerance, never bit-for-bit.
- **Fix the reduction order** for repeatable runs on one device: sort contacts by grain and sum each grain's run, instead of atomic adds.
- **Avoid fast-math modes for the precise set.** Flushing subnormals or approximate division would undo the point of computing contacts in 64-bit.
- **FMA usually helps accuracy;** leave it on, but expect it to be one reason the CPU path differs.

## Cautions

- The paper is NVIDIA-specific. AMD GPUs (ROCm/HIP) have equivalent FMA behaviour and compiler options, with different flag names.
