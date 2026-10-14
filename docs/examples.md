---
layout: default
title: Examples
---

# Examples

| Process | Available checks | Numerical integration |
| --- | --- | --- |
| `emep_emepz` | Amplitude configuration; YAML generation; Cuba smoke runs | Implemented mapping; reference comparison required for release |
| `mummup_vmvmxttxh` | MG5 3.8.1 helicity and legacy phase-space comparisons | Integrated 2→5 mapping; high-statistics reference comparison pending |

## e− e+ → e− e+ Z

The [first calculation](getting-started.html) selects graph 5, configuration 0
and sqrt(s) = 1000 GeV. For a full channel sum use:

```sh
build/processes/emep_emepz/emep_emepz_fortran_cuba_integrate_all \
  processes/emep_emepz/cards/param_card.dat \
  processes/emep_emepz/cards/amplitude_config_list.dat \
  0 1000 100000 0.05 12345 10000 10000 1000 0
```

Every graph uses a deterministic seed. The reported total uncertainty assumes
independent graph estimates. Increase statistics and compare an established
reference before treating this as a physical prediction.


## μ− μ+ → νμ anti-νμ t tbar h

The integrated `mummup_vmvmxttxh` example contains 89 diagrams, one colour
amplitude, 64 helicity combinations and 11 integration dimensions.

```sh
build/processes/mummup_vmvmxttxh/mummup_vmvmxttxh_fortran_driver \
  --run-card processes/mummup_vmvmxttxh/cards/run_card.dat \
  --graph 5 --maxeval 100000 --epsrel 0.02 --cores 0
```

Configuration 0 retains the full coherent amplitude. Alternative configurations
are separate observables and must not be summed as if they were graph channels.
See the [validation record](validation-20261013.html): sampled amplitude and
mapping comparisons passed, while six 100 TeV channels still failed their
requested precision. A high-statistics cross-section comparison remains pending.

## Additional examples

JSON scan definitions live in `examples/`. Historical standalone phase-space
source examples are retained under `examples/legacy_phase_space/`; they are
reference material, not active CMake input or additional validated processes.

## Fixed-point amplitude checks

Both public samples support reproducible momenta and helicity evaluation.
See [Fixed points](fixed-points.html) for same-point MG5 comparisons and
[Tools](tools.html) for an overview. `gg_ggg` is retained locally and excluded
from the current public source because its integration is not ready.
