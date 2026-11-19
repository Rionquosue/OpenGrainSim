# FP8 Formats for Deep Learning

- **Authors:** Paulius Micikevicius et al. (NVIDIA, Arm, Intel)
- **Published:** arXiv:2209.05433, September 2022
- **Source used:** the arXiv paper, read in full: <https://arxiv.org/abs/2209.05433>

## Summary

Proposes two 8-bit floating-point encodings for training and running neural networks: **E4M3** (4 exponent bits, 3 mantissa bits) for weights and activations, and **E5M2** (5 exponent bits, 2 mantissa bits) for gradients, which need more range. FP8 training matched 16-bit training quality on image and language models up to 175 billion parameters. Values are multiplied by a per-tensor scaling factor before conversion so they fit FP8's narrow range.

## Key facts

| Property | E4M3 | E5M2 |
| --- | --- | --- |
| Exponent / mantissa bits | 4 / 3 | 5 / 2 |
| Largest normal value | $1.75\times2^{8} = 448$ | $1.75\times2^{15} = 57{,}344$ |
| Smallest normal value | $2^{-6}$ | $2^{-14}$ |
| Smallest subnormal value | $2^{-9}$ | $2^{-16}$ |
| Infinities | none (range extended instead) | yes (IEEE-style) |
| NaN | one bit pattern | IEEE-style |

**Spacing between neighbouring values.** With $m$ mantissa bits, the gap between consecutive values in $[2^e, 2^{e+1})$ is $2^{e-m}$, a relative step of $2^{-m}$ to $2^{-(m+1)}$:

- E4M3: 6.25%–12.5% relative step.
- E5M2: 12.5%–25% relative step.

For comparison: FP16 (10 bits) 0.05%–0.1%; FP32 (23 bits) about $10^{-7}$.

**Scaling.** Higher-precision values are multiplied by a scale factor before casting to FP8 and divided by it afterwards; for matrix multiplication the unscaling is applied once per output.

## Takeaways for the grain solver

- **FP8 is built for tensor-core matrix maths with per-tensor scaling,** where averaging over thousands of products hides rounding. A particle solver does no matrix multiplication and needs each value to be accurate on its own.
- **Concrete numbers for the concept sheet's table:** near 1 m, E4M3's step is $2^{-3}$ m = 125 mm. A position stored that way cannot resolve grains smaller than a fist.
- **Possible future use:** FP8 or FP16 could store data that is not physics (colour, age, a damage display value) to save memory traffic, as the concept sheet allows.

## Cautions

- The paper describes formats and training results; hardware support for FP8 arithmetic varies by GPU generation and is mostly exposed through matrix-multiply units.
