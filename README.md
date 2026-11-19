# OpenGrainSim

A GPU particle solver for sand, grains, liquids and semisolids, run as a separate process from Blender (through Riry Tools) and returning only the grains that moved each frame. It targets NVIDIA CUDA and AMD ROCm through PyTorch, with a CPU fallback, on any operating system.

Nothing is implemented yet. The design and research live in [planning/](planning/README.md):

- [Concept sheet](planning/concept-sheet.md): goals, architecture, GPU backend, solver, data contract, Blender side, precision, tests and phases.
- [Formula catalogue](planning/formula-catalogue.md): every formula sorted into chapters by purpose and solving method.
- [Formula table](planning/formulatable.md): one block per formula, with sources.
- [Reference extracts](planning/extracts/README.md): equations and takeaways from each paper.

Source papers are never stored in this repository; `.gitignore` blocks PDF, EPUB and DjVu files.

## Licence

MIT; see [LICENSE](LICENSE).
