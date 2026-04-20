# CLAUDE.md — patchestry

Patchestry: patch ancestry analysis for binary code. Lifts-and-analyzes binary patches to
determine provenance, lineage, and semantic equivalence across patch generations. Used by
Project-Metic as a supply chain security component for SBOM and binary supply chain analysis.

License: Apache 2.0 (upstream: lifting-bits/patchestry, ARPA-H funded).

## What This Repo Is

A binary patch analysis research tool — not a standalone microservice. Patchestry:
- Analyzes binary patches to determine their patch ancestry graph
- Uses LLVM/Clang APIs for binary lifting and semantic analysis
- Integrates into Metic's supply chain security pipeline

## Key Invariants

- **This is a research/analysis tool, not a standalone service.** Patchestry does not have
  an HTTP API. Integration with Metic is via CLI invocation from analysis workflows.
- **LLVM/Clang version coupling.** The codebase links against specific LLVM/Clang versions.
  Do not update the LLVM dependency without verifying ABI compatibility.
- **Apache 2.0 license boundary.** Patchestry is Apache 2.0 licensed (upstream). Code changes
  must remain compatible with Apache 2.0. Do not introduce GPL-only dependencies.

## Dev Commands

See the upstream CI config (`.github/workflows/ci.yml`) for the authoritative build procedure.

```bash
# Build (requires LLVM/Clang toolchain)
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j$(nproc)

# Run tests
ctest --output-on-failure

# Run on a binary patch
./patchestry analyze --binary original.elf --patch patched.elf
```

## Tech Stack

- C++, LLVM/Clang APIs, CMake

## Integration with Project-Metic

- Invoked as a step in `metic-api` supply chain scan workflows
- Results feed into the SBOM explorer panel in the Metic portal
- Patch ancestry reports registered in Mouseion under the supply chain tag

## What NOT to Do

- Do not introduce GPL-only dependencies (license boundary)
- Do not update LLVM without verifying ABI compatibility with the current Clang binding
