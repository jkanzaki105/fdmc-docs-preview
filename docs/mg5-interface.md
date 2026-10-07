---
layout: default
title: MG5 → FDMC interface
---

# MG5 → FDMC interface

## Import into a fresh process directory

```sh
python3 tools/mg5_fdmc_adapter.py /path/to/MG5_output processes/new_process
```

For multiple subprocesses, select one explicitly:

```sh
python3 tools/mg5_fdmc_adapter.py /path/to/MG5_output processes/new_process \
  --subprocess P1_subprocess_name
```

Use a new destination to avoid retaining stale generated files. Do not
regenerate the working `emep_emepz` mapping as a generic scaffold.

| MG5 input | FDMC destination |
| --- | --- |
| `Cards/` | `cards/` |
| `Source/DHELAS/` | `source/helas/` |
| `Source/MODEL/` | `source/model/` |
| Other `Source/` files | `source/` |
| Selected subprocess | `src/` |

The adapter instruments `matrix.f` to store AMP/JAMP through the shared
amplitude state, generates `get_jamp.f90`, dimension includes and
`mod_params.f90`, and emits process templates for two incoming particles and
at least two outgoing particles. MG5 `TMP_JAMP` intermediates and helicity
tables must remain intact. Unsupported input forms fail rather than guessing
colour or dimension information.

## Complete the process physics

Generated runtime, CMake, drivers and tests are scaffolding. Review every
`USER EDIT/TODO`, supply topology assignments, particle permutations,
propagator identities, masses and widths, and validate phase-space behaviour.
The adapter does not infer these from a diagram automatically.

A new directory with `CMakeLists.txt` is discovered by the top-level build.
Link to the existing shared libraries instead of copying their implementations.
Run amplitude consistency checks against MG5, then kinematic/mapping checks,
then numerical integration comparisons. Higher multiplicity support does not
by itself make a process ready for physical integration.

With private software access, see that repository's `tools/README.md` and
`tools/templates/fortran_process/README.md` for detailed conversion rules.
