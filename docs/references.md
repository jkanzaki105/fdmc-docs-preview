---
layout: default
title: References
---

# References

The scientific basis of this documentation is the MCPS paper listed below.
This project prepares the techniques used in that paper for software distribution.
The paper provides the method, physics applications and published results;
the documentation describes the software and its examples.

{% for ref in site.data.references %}
<h2 id="{{ ref.id }}">{{ ref.title }}</h2>

{{ ref.display_authors }}, *{{ ref.journal }}* **{{ ref.volume }}**, {{ ref.pages }} ({{ ref.year }}).

[Published article](https://doi.org/{{ ref.doi }}) · [arXiv:{{ ref.eprint }}](https://arxiv.org/abs/{{ ref.eprint }})

{{ ref.role }}

{% if ref.note %}{{ ref.note }}.{% endif %}

Citation key: `{{ ref.key }}`.
{% endfor %}

[Download the bibliography in BibTeX format](references.bib).

## Related literature

The MadEvent paper above establishes the original SDE method. Further references
on FD gauge, HELAS, MG5 and integration methods will be
added here as the corresponding documentation is reviewed. Each reference
should be linked from the section that uses it, with its role explained.
