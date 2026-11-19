# Planned: grain solver (grainsim)

Planning material for a GPU particle solver for sand, grains, liquids and semisolids, run by Riry Tools as a separate process. Nothing here is implemented yet. Moved here from the riryblendworkflow repository, where it was `planned/grainsim/`.

- [concept-sheet.md](concept-sheet.md): the design (goals, architecture, GPU backend, solver, data contract, Blender side, precision, tests, phases, reading list). Exported from the shared concept-sheet doc; the doc is the editable original.
- [architecture.png](architecture.png): the architecture diagram from the concept sheet.
- [formula-catalogue.md](formula-catalogue.md): the formulas sorted into chapters by purpose and solving method (mirrors the doc's *Formula catalogue* tab).
- [formulatable.md](formulatable.md): every formula the solver may use, one block each, with redundant ones removed ([formulatable-removed.md](formulatable-removed.md) lists what was removed and why).
- [extracts/](extracts/README.md): one extract per reference in the reading list, with its key equations and takeaways for this solver.
