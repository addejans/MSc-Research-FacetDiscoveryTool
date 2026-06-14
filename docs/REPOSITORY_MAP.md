# Repository Map

This repo preserves the original MSc research artifacts while making the structure easier to navigate.

## Main documentation

| Path | Purpose |
| --- | --- |
| `README.md` | Modern overview and research framing. |
| `docs/RESEARCH_CONTEXT.md` | Explains why the project matters in modern optimization terms. |
| `docs/PIPELINE.md` | Connects this repo to the Fourier-Motzkin projection pipeline. |
| `docs/REPOSITORY_MAP.md` | This map. |

## Historical notebooks

The notebooks are exploratory research notebooks. They are not intended to be a clean package API.

| Folder | Purpose |
| --- | --- |
| `notebooks/experiments/` | Original notebooks organized by research date and topic. |

Notebook topics include:

- early feature extraction experiments;
- `all_different` constraint experiments;
- `at_least` constraint experiments;
- cycle/multiple-all-different experiments;
- classifier experiments.

## Historical data and artifacts

| Folder | Purpose |
| --- | --- |
| `data/raw/` | Loose raw outputs and helper files from the original work. |
| `bounds/` | Fourier-Motzkin output files related to bound-style cases. |
| `k0/` | Fourier-Motzkin output files for k=0-style cases. |
| `sorted20180211/` | Historical sorted Fourier-Motzkin output files. |
| `sorted20171105_Reduced/` | Historical reduced Fourier-Motzkin output files. |
| `sorted_reduced/` | Additional historical reduced output copy. |
| `artifacts/presentations/` | MSc presentation slides. |
| `artifacts/archives/` | Original zip uploads retained for provenance. |

## How to read this repo

The most important thing is the research pipeline, not the old notebook coding style.

Start here:

1. `README.md`
2. `docs/RESEARCH_CONTEXT.md`
3. `docs/PIPELINE.md`
4. `artifacts/presentations/MSc_Presentation_2.pdf`
5. `notebooks/experiments/Classification_Attempt1.ipynb`

## Recommended future cleanup

A future modernization pass could extract the useful notebook logic into a small Python package:

```text
src/facet_discovery/
  parsing.py
  normalization.py
  features.py
  clustering.py
  evaluation.py
```

That would turn the historical prototype into a reproducible research library.
