# What Every Computer Scientist Should Know About Floating-Point Arithmetic

- **Author:** David Goldberg
- **Published:** ACM Computing Surveys 23(1), March 1991 · DOI [10.1145/103162.103163](https://doi.org/10.1145/103162.103163)
- **Source used:** Oracle's edited reprint (Numerical Computation Guide, Appendix D), read in full: <https://docs.oracle.com/cd/E19957-01/816-2464/ncg_goldberg.html>

## Summary

The classic tutorial on floating-point: how numbers are represented, how rounding error is measured, why guard digits matter, what IEEE 754 guarantees, and how algorithms can be written to stay accurate. It explains every claim with a reason, which makes it the reference for "why did my simulation drift".

## Key equations and facts

**Representation.** With base $\beta$ and precision $p$ digits, a floating-point number is $\pm\, d_0.d_1\ldots d_{p-1} \times \beta^{e}$.

**Units in the last place (ulp) and relative error.** Rounding to nearest gives an error of at most 0.5 ulp. The matching relative error is bounded by the **machine epsilon**

$$\varepsilon = \frac{\beta}{2}\,\beta^{-p}$$

which is $2^{-24} \approx 5.96\times10^{-8}$ for IEEE single precision and $2^{-53} \approx 1.11\times10^{-16}$ for double. The relative error of 0.5 ulp varies by up to a factor of $\beta$ across a binade ("wobble").

**Basic operations are correctly rounded.** IEEE 754 requires $+, -, \times, \div$ and $\sqrt{}$ to return the exact result rounded to the nearest representable value:

$$\operatorname{fl}(a \circ b) = (a \circ b)(1 + \delta), \qquad \lvert\delta\rvert \le \varepsilon$$

**Catastrophic cancellation.** Subtracting two nearly equal numbers that already carry rounding error leaves mostly error. Benign cancellation (of exact inputs) is harmless.

**Floating-point addition is not associative:** $(a + b) + c$ can differ from $a + (b + c)$. A sum's result depends on the order of its terms.

**Kahan (compensated) summation** keeps a running correction term so that summing $n$ numbers has an error bound nearly independent of $n$:

```
sum = 0; c = 0
for x in values:
    y = x - c
    t = sum + y
    c = (t - sum) - y   # the part of y lost when adding
    sum = t
```

## Takeaways for the grain solver

- **Order changes results.** Because addition is not associative, summing contact corrections in a different order (as GPU atomic adds do) changes the last bits. This is the root cause behind the concept sheet's deterministic-mode requirement.
- **Positions relative to the domain centre** keep values small, so one ulp is a smaller distance. In float32, a coordinate near 1 km is quantised to about 0.06 mm; near 1 m, to about 0.1 µm.
- **Overlap is a subtraction of nearly equal numbers** (distance minus sum of radii). Computing it in a local frame, from position differences rather than absolute positions, avoids cancellation.
- **Kahan summation** is a cheap option for long per-grain accumulations, such as many contacts on one grain.

## Cautions

- The reprint is lightly edited from the 1991 paper; the substance is the same.
