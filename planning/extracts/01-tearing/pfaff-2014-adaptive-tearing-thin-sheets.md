# Adaptive Tearing and Cracking of Thin Sheets

- **Authors:** Tobias Pfaff, Rahul Narain, Juan Miguel de Joya, James F. O'Brien
- **Published:** ACM Transactions on Graphics 33(4), SIGGRAPH 2014
- **Link:** <https://escholarship.org/uc/item/1mc8b47z>
- **Source used:** the eScholarship copy, read in full (supplied by the repository owner; not stored in this repository). This replaces an earlier version of this extract that was written from memory.

## Summary

Tearing and cracking of thin sheets (paper, rubber, foil, metal, glass) on a triangle mesh that refines itself around crack tips and coarsens again once stress is relieved. It builds on the ARCSim cloth code (Narain et al. 2012, 2013). Its contributions: a fracture criterion that does not depend on triangle size or shape, a "fracture substepping" loop that lets many cracks advance within one time step, plasticity for stretching and bending, and remeshing that adds little artificial stress.

## Key equations

**Elasticity** (per triangle; reference shape embedded in 3D, deformation gradient $F$ is 3×3):

$$G = \tfrac12\big(F^{T}F - I\big), \qquad \sigma = k_s\big((1-\nu)\,G + \nu\operatorname{tr}(G)\,I\big), \qquad k_s = \frac{Y h}{1 - \nu^2}$$

($Y$ Young's modulus, $h$ thickness, $\nu$ Poisson's ratio.)

**Bending** from dihedral angles $\theta_i$ at the three edges (rest angle $\theta_i^R$, edge length $l_i$, edge tangent $t_i$, triangle area $A$):

$$S = \frac{1}{2A}\sum_{i=1}^{3} (\theta_i - \theta_i^R)\, l_i\, t_i t_i^{T}$$

**Energy:**

$$E = \sum_{\text{faces}} \frac{A_i}{2}\, G_i : \sigma_i + \sum_{\text{edges}} k_b\,\frac{l_i^2}{8A_i}\,(\theta_i - \theta_i^R)^2, \qquad k_b = \frac{\alpha\, k_s h^2}{12}$$

$\alpha$ allows a different bending stiffness out of plane (for example rolled steel).

**Fracture criterion: separation strength at a vertex.** For a candidate splitting plane with normal $s$ through the vertex, sum the tensile stress across each side. Negative principal stresses are clamped to zero ($\sigma^+$) so compression never causes fracture. With the stress constant on each surrounding triangle and $t_i$ the tangents between them:

$$q_1 = \sigma_0^{+}(t_0 - s) + \sum_{i=0}^{N-1} \sigma_i^{+}(t_{i+1} - t_i) + \sigma_N^{+}(-s - t_N)$$

$q_2$ is computed the same way on the other side. To ignore unbalanced loads (which would just move the sheet rigidly), the separation strength is the smaller common component:

$$s_{sep} = \min\big(q_1\cdot\hat q,\; -q_2\cdot\hat q\big), \qquad \hat q = \frac{q_1 - q_2}{\lVert q_1 - q_2\rVert}$$

- The vertex fails if $s_{sep}$ exceeds the area toughness $\tau = \tau_V\, h$ (volume toughness times thickness).
- The best plane is found by sampling 200 directions.
- Vertices are skipped cheaply using the bound $s_{sep} \le 2\max_i \lambda_i$, with $\lambda_i$ the largest eigenvalue of each $\sigma_i^{+}$.
- When extending an existing crack, the crack edge is used as one side of the split, which gives curved cracks and prevents cracking backwards.

**Bending fracture.** Bending adds stretch at the sheet's outer surface ($\Delta l = \tfrac12 h\kappa$ for curvature $\kappa$). It is included by replacing $F$ with a bent version before computing $G$, using the second fundamental form with negative eigenvalues flipped positive ($II^*$):

$$F_B = F\left(I + \tfrac12\,\alpha\, h\, II^{*}\right)$$

(The printed equation includes $\alpha$; the derivation in the text gives $\tfrac12 h\kappa$. Check which applies.)

**Stretching plasticity** (multiplicative, after Bargteil et al. 2007). The elastic deformation is $F_{el} = F\,F_p$, with $F_{el} = U\Sigma V^{T}$. Each step, $F_p$ is updated by a fraction $\gamma$ of the volume-preserving part of the elastic stretch:

$$\gamma = \operatorname{clamp}\big(\Delta t\,\mu\,(\sigma_M - \sigma_0),\ 0,\ 1\big), \qquad \sigma_M = \sqrt3\,\Big\lVert \sigma - \tfrac13\operatorname{tr}(\sigma)\, I\Big\rVert$$

with flow rate $\mu$ and yield stress $\sigma_0$. Thickness is updated the same way, $h \leftarrow h\,\det(\Sigma)^{-\gamma/3}$, so stretched regions thin, then bend and break more easily. (The exact placement of the exponents in the $F_p$ update did not survive text extraction; see Eq. 9 of the paper.)

**Bending plasticity** with yield curvature $\kappa_0$:

$$S_p \leftarrow S_p + \frac{S_{el}}{\lVert S_{el}\rVert}\big(\lVert S_{el}\rVert - \kappa_0\big)$$

**Fracture sizing for the remesher** ($x_{min}$ the smallest allowed edge), so candidate crack tips are always at the finest resolution:

$$M_{frac} = \left(\frac{2\lambda}{\tau\, x_{min}}\right)^2 I$$

## Fracture substepping (Algorithm 1)

1. Compute separation strength at all vertices; vertices above $\tau$ form the fracture zone $Z$. Save vertex positions.
2. While $Z$ is not empty:
   1. Grow $Z$ by an $n$-ring of neighbours ($n = 3$ in most examples; at least 2 for bending).
   2. Remesh $Z$ locally at fine resolution.
   3. Run a local "virtual" physics step on $Z$ and recompute separation strengths.
   4. Rewind positions to the saved ones.
   5. Break only the single vertex with the highest strength, along its best plane.
   6. Rebuild $Z$ from vertices still above $\tau$.
3. Positions are unchanged at the end; only the mesh topology has changed. The next full physics step redistributes stress.

## Takeaways for the grain solver

- **Break one at a time, then re-evaluate.** Breaking only the strongest candidate and recomputing locally before the next break is what gives clean cracks instead of shattering. A particle tearing model can do the same: per substep, break at most the most-stressed bond in each neighbourhood.
- **Tension only.** Clamping negative stresses to zero before testing for fracture keeps compressed regions intact; the same rule applies to bond breaking.
- **Toughness scales with thickness** ($\tau = \tau_V h$), and stretched material thins and weakens: useful for crusts and skins modelled as single particle layers.
- **Guide tears with weak lines.** The paper tears paper around printed shapes by placing lower-toughness defect lines; a per-particle or per-bond toughness map would allow perforations and score lines in food.

## Cautions

- Mesh-based and built on adaptive remeshing (ARCSim); none of the remeshing machinery carries over to a fixed-count particle solver.
- Two equations (bending factor, plasticity exponents) are flagged above where the text extraction was ambiguous.
