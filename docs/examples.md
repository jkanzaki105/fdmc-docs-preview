---
layout: default
title: Examples
---

# Examples

| Process | Available checks | Numerical integration |
| --- | --- | --- |
| `emep_emepz` | Amplitude configuration; YAML generation; Cuba smoke runs | Implemented mapping; reference comparison required for release |
| `gg_ggg` | Kinematics, colour/amplitude consistency and configurations | Unimplemented process mapping; deliberately stops |

## e− e+ → e− e+ Z

The [first calculation](getting-started.html) selects graph 5, configuration 0
and sqrt(s) = 1000 GeV. For a full channel sum use:

```sh
build/processes/emep_emepz/emep_emepz_fortran_cuba_integrate_all \
  processes/emep_emepz/cards/param_card.dat \
  processes/emep_emepz/cards/amplitude_config_list.dat \
  0 1000 100000 0.05 12345 10000 10000 1000 1
```

Every graph uses a deterministic seed. The reported total uncertainty assumes
independent graph estimates. Increase statistics and compare an established
reference before treating this as a physical prediction.

## g g → g g g

Build and run its amplitude tests independently:

```sh
cmake -S . -B build-gg -DFDMC_FORTRAN_PROCESS=gg_ggg -DFDMC_FORTRAN_ENABLE_CUBA=OFF
cmake --build build-gg -j 4
ctest --test-dir build-gg --output-on-failure
```

The consistency check samples MG5 helicities and compares reconstructed
colour-summed amplitudes. It does not exercise a completed integration map.

## Additional examples

JSON scan definitions live in `examples/`. Historical standalone phase-space
source examples are retained under `examples/legacy_phase_space/`; they are
reference material, not active CMake input or additional validated processes.
