# FDMC documentation preview

Documentation-only review snapshot. **FDMC is a provisional project name.**
The software source is maintained separately in a private repository.

The site source is in `docs/`. Configure GitHub Pages using the `main` branch
and `/docs`. Intended address: https://jkanzaki105.github.io/fdmc-docs-preview/

This preview invites discussion of wording, navigation, scope and presentation.
It is not a software release or a claim of independent physics validation.

Documentation and site assets: BSD-3-Clause; see LICENSE.

## Managing references

Maintain bibliographic metadata in `docs/_data/references.json`. The References
page and downloadable BibTeX are rendered from that single catalogue by Jekyll,
without additional plugins. Keep stable citation keys and HTML IDs; link from
the relevant text to `references.html#<id>` and explain what the paper supports.
Verify author/title/DOI/arXiv metadata against primary sources before adding a
record. Extend the BibTeX template if a new publication type or fields are needed.

The primary scientific reference is `Hagiwara:2026lul` (MCPS). This paper citation
is distinct from a future software-release citation or DOI.
