# Theoretical Soil Mechanics: Terzaghi's Bearing Capacity Theory

- **Author:** Karl Terzaghi
- **Published:** John Wiley & Sons, New York, 1943
- **Source used:** ⚠️ **The book was not read** (not freely available). This extract uses the Wikipedia article on bearing capacity (<https://en.wikipedia.org/wiki/Bearing_capacity>), read in full, which gives Terzaghi's equations and factors. **Check against a geotechnical textbook before relying on exact factors.**

## Summary

Terzaghi gave the first complete theory for the ultimate bearing capacity of shallow foundations: the pressure a footing can put on soil before the soil shears out sideways in a "general shear failure". It accounts for cohesion, friction, how deep the footing is buried, the soil's own weight, and the footing's shape. Meyerhof (1951, 1963) and Vesić later refined the factors.

## Key equations

**Ultimate bearing capacity** ($c'$ cohesion, $\sigma'_{zD}$ vertical effective stress at the footing's depth, $\gamma'$ soil unit weight, $B$ footing width):

| Footing | Ultimate bearing capacity |
| --- | --- |
| Strip (long) | $q_{ult} = c' N_c + \sigma'_{zD} N_q + 0.5\,\gamma' B N_\gamma$ |
| Square | $q_{ult} = 1.3\,c' N_c + \sigma'_{zD} N_q + 0.4\,\gamma' B N_\gamma$ |
| Circular | $q_{ult} = 1.3\,c' N_c + \sigma'_{zD} N_q + 0.3\,\gamma' B N_\gamma$ |

**Bearing capacity factors** (friction angle $\phi'$ in degrees; $K_{p\gamma}$ is a passive pressure coefficient):

$$N_q = \frac{e^{2\pi(0.75 - \phi'/360)\tan\phi'}}{2\cos^2(45^\circ + \phi'/2)}, \qquad N_c = \frac{N_q - 1}{\tan\phi'} \;(\phi' > 0), \qquad N_\gamma = \frac{\tan\phi'}{2}\left(\frac{K_{p\gamma}}{\cos^2\phi'} - 1\right)$$

For $\phi' = 0$, Terzaghi's $N_c$ is 5.7 (Meyerhof's is 5.14). A common approximation is $N_\gamma \approx \dfrac{2(N_q + 1)\tan\phi'}{1 + 0.4\sin(4\phi')}$.

**Local shear failure** (loose soils): use $0.867\,c'$ in place of $1.3\,c'$ for square footings and reduced factors $N'_c, N'_q, N'_\gamma$, computed with a reduced friction angle.

## Takeaways for the grain solver

- **A validation test, not a solver feature.** Pressing a flat box into a bed of grains and recording the force at which the bed fails gives a number to compare against $q_{ult}$. It checks that friction and pressure behave like real sand at scale.
- **Bearing capacity grows steeply with friction angle** (the factors rise exponentially with $\phi'$). If simulated sand fails much too early under a weight, the effective friction angle is too low.
- **Self-weight term $\propto B$:** wider footings carry more pressure in frictional soil, which is a useful sanity check when varying object size.

## Cautions

- Engineering formulas with empirical factors; a particle simulation will match only approximately, especially at low grain counts across the footing.
- Factors are as given on Wikipedia, which notes disagreements between sources.
