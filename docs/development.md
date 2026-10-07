---
layout: default
title: Development and extension boundaries
---

# Development and extension boundaries

## Current dependency structure

Kinematics is used by elementary/generated phase space and process HELAS.
Amplitude state, colour contraction and configuration handling are shared.
Utilities own CLI/run-card parsing. Integration backends depend on the common
interface. Each process core combines those libraries with its own model,
HELAS, matrix, JAMP reconstruction and phase-space mapping. Drivers then link
the process core and the requested integration backend.

MG5 `.inc` files are resolved through process `src`, `source/model`, `source`
and process-root include paths. Fortran module files belong to each target's
binary directory. Keep model/HELAS routines process-local until their symbols,
model dependence and gauge-specific variants are reviewed for safe reuse.

## Future development

| Direction | Boundary to preserve |
| --- | --- |
| Standalone Fortran HELAS | Add a reviewed library at `helas/fortran/`; retain MG5-specific routines and provenance |
| C++/CUDA | Add explicit backend targets and language-specific sources; do not share incompatible module/object files |
| Backend-independent histogramming | Add common bin/schema/merge semantics; keep integration-specific state separate |
| Independent diagram generator | Add a generator interface when a real implementation exists; keep the MG5 adapter in `tools/` for Version 0 |

No empty future-backend directories are required now. Older implementations are retained in the private local development repository.
The separate software candidate currently focuses on Fortran.

## Add a process

Import into a fresh directory with the [MG5 adapter](mg5-interface.html), finish
and review physics mappings, and test a separate process build before `ALL`.
Do not duplicate shared amplitude/configuration/parser code. Compatibility
source copies should not replace CMake-generated files by accident.

These pages describe the software architecture; this documentation repository
contains no software implementation or private historical material.
