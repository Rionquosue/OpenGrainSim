# torch.use_deterministic_algorithms

- **Publisher:** PyTorch documentation (version 2.15 at the time of reading)
- **Link:** <https://docs.pytorch.org/docs/stable/generated/torch.use_deterministic_algorithms.html>
- **Source used:** the documentation page, read in full (Markdown version of the 2.15 page).

## Summary

`torch.use_deterministic_algorithms(True)` makes PyTorch either use a deterministic implementation of each operation or raise an error when only a non-deterministic one exists. It is the switch the grain solver's deterministic mode relies on, and its list of affected operations shows exactly which array operations the solver must avoid or replace.

## Key facts

**Signature:** `torch.use_deterministic_algorithms(mode, *, warn_only=False)`. With `warn_only=True`, operations without a deterministic implementation warn instead of raising an error.

**Operations that switch to a deterministic implementation** when enabled (selected, relevant to the solver):

- `torch.Tensor.index_put()` with `accumulate=False`, and with `accumulate=True` on CPU.
- `torch.Tensor.scatter_add_()` on a CUDA tensor.
- `torch.index_add()` on a CUDA tensor.
- `torch.Tensor.scatter_reduce()` with `reduce='sum'` or `reduce='mean'` on a CUDA tensor.
- `torch.cumsum()` on a CUDA tensor with a floating-point dtype.

**Operations that raise an error** when enabled include some other `scatter_reduce()` modes on CUDA or MPS tensors and further cases of `index_put()` with `accumulate=True`. Check the live list for the installed version: it changes between releases (older versions listed `scatter_add_` and `index_add_` on CUDA as errors).

**Related settings**

- `torch.utils.deterministic.fill_uninitialized_memory`: fills uninitialised memory with a known value when deterministic mode is on.
- The docs warn that deterministic implementations tend to be slower.

## Takeaways for the grain solver

- **Scatter-add of contact corrections is the solver's key non-deterministic operation.** In current PyTorch, deterministic mode gives it a deterministic CUDA path, so the simplest implementation is `index_add_` or `scatter_add_` with deterministic mode on.
- **Do not rely on it alone.** Behaviour has changed between releases and may differ on ROCm and MPS. The solver should pin its PyTorch version and include the concept sheet's determinism test (two runs, bit-identical output) in CI.
- **Fallback:** if deterministic scatter is slow or unavailable on a backend, use sort-by-grain plus a segmented sum over the sorted runs. Note that `torch.cumsum` on CUDA floats is itself only deterministic in this mode, so the cumulative-sum route needs the switch on too.

## Cautions

- The list of affected operations is version-specific; the facts above are for PyTorch 2.15.
