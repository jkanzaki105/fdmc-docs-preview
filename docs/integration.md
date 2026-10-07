---
layout: default
title: Integration and run management
---

# Integration and run management

`integration/common` defines the common integrand, option and result types.
The public backend is external Cuba/Vegas. BASES is not included. Common
histogramming remains a future extension.

## Build with Cuba

Install Cuba separately, then run from the repository root:

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DCUBA_ROOT=/path/to/cuba/prefix
cmake --build build -j 4
ctest --test-dir build --output-on-failure
```

CMake finds `cuba.h` and the library below the given prefix. The default enables
Cuba; `-DFDMC_FORTRAN_ENABLE_CUBA=OFF` builds amplitude/kinematics tests only.

## Run card and overrides

```sh
build/processes/emep_emepz/emep_emepz_fortran_driver \
  --run-card processes/emep_emepz/cards/run_card.dat \
  --graph 5 --maxeval 100000 --epsrel 0.02
```

Command-line values override the `&fdmc_run` namelist. Relative card paths are
resolved against the run-card directory. Graph 0 integrates every graph in the
selected configuration. The public run-card schema contains Cuba controls;
old private backend-specific settings are not accepted.

Cuba status 0 means the target accuracy was reached. Nonzero status or a
`failed_graphs` count must be examined before accepting a result. Low-statistic
runs can return estimates without meeting their target.

## Managed runs and scans

```sh
tools/fdmc-run create runs/example \
  --run-card processes/emep_emepz/cards/run_card.dat \
  --param-card processes/emep_emepz/cards/param_card.dat \
  --amp-config processes/emep_emepz/cards/amplitude_config_list.dat \
  --ident-card processes/emep_emepz/cards/ident_card.dat

tools/fdmc-run execute runs/example \
  --executable build/processes/emep_emepz/emep_emepz_fortran_driver --jobs 1
tools/fdmc-run status runs/example
tools/fdmc-run finalize runs/example
```

The manager snapshots inputs, records checksums, manages graph tasks, retries
failures and checks completeness before producing a formal total. Executable
checksums prevent mixing different binaries. `tools/fdmc-scan` expands the JSON
scan examples in `examples/`. See `tools/README.md` for detailed commands.

Independent graph estimates are summed with statistical errors in quadrature.
This aggregation does not establish channel-weighting or physics correctness.
Process-global state is not reentrant; prefer separate graph processes. Use
Cuba `cores=1` with externally parallel tasks to avoid nested parallelism.

## Distribution

FDMC-owned source is BSD-3-Clause. Cuba is externally installed and LGPL v3;
see the software repository's third-party notices (private access required). Source-only publication does not bundle
Cuba. Distribution of linked binaries needs its own LGPL compliance package,
including notices and an appropriate source/relinking or replacement mechanism.
