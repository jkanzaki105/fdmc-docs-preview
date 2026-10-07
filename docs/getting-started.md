---
layout: default
title: Getting started
---

# Getting started

## Requirements

Use CMake 3.20 or newer, a GNU Fortran compiler supporting Fortran 2018, a C
compiler and Python 3. GNU-specific compatibility flags are enabled for legacy
MG5 sources. Other compilers have not been established by this
preparation. Python tools use the standard library; the topology generator
supports the FDMC YAML subset rather than arbitrary YAML.

The commands below describe the software workflow. They require access to the
separate private software repository and cannot be run from this documentation-only
repository. Run them from the software repository root.

## Clean build

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DCUBA_ROOT=/path/to/cuba/prefix
cmake --build build -j 4
ctest --test-dir build --output-on-failure
```

By default both processes are built and external Cuba is required. A process-specific
build can use `-DFDMC_FORTRAN_PROCESS=emep_emepz` or `gg_ggg`; `ALL` is the
default. An unknown name fails at configuration and lists available processes.

CMake writes generated Fortran, object files and modules under `build/`.
Keep build output outside the source libraries.

## First calculation

```sh
build/processes/emep_emepz/emep_emepz_fortran_cuba_integrate \
  processes/emep_emepz/cards/param_card.dat \
  processes/emep_emepz/cards/amplitude_config_list.dat \
  5 0 1000 100000 0.02 12345 10000 10000 1000 1
```

Arguments after the cards are graph, configuration, centre-of-mass energy in
GeV, maximum evaluations, relative accuracy, seed, starting/incremental
evaluations, batch size and Cuba cores. Cuba status 0 means the target was met;
nonzero status must be inspected before accepting a result. Output includes the integral and statistical uncertainty in fb.

Keep `param_card.dat` and `ident_card.dat` together: the MG5 model reader needs
both. Model initialization may write `param.log` in the working directory.

Cuba must be installed separately. Use [Integration](integration.html) for
managed runs. Set `-DFDMC_FORTRAN_ENABLE_CUBA=OFF` for amplitude-only builds. Use [Physics and validation](physics.html) before interpreting results.
